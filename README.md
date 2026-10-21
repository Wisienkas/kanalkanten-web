# Kanalkanten – hjemmeside

Beboersiden for Ejerforeningen Kanalkanten. Ren statisk HTML og CSS: ingen build, ingen database, ingen server-side kode.

```
.
├── README.md
└── public/              # alt her bliver udgivet
    ├── index.html
    ├── style.css        # fælles styling for alle sider
    ├── husorden.html
    ├── projekter.html
    ├── nyheder/         # nyhedsindlæg (undersider)
    ├── images/
    └── dokumenter/      # PDF'er, fx husorden.pdf
```

Kun indholdet af `public/` udgives. Alt andet i repoet (README m.m.) bliver ikke serveret.

## Kør lokalt

Åbn `public/index.html` direkte i browseren, eller start en lille server:

```sh
python3 -m http.server -d public 8000
# åbn http://localhost:8000
```

## Før den går live

1. Søg efter `ADMIN_URL` og ret til adressen på administrationssiden.
2. Læg husordenen i `public/dokumenter/husorden.pdf`.
3. Udskift `public/images/hero.jpg` med et rigtigt foto (ca. 1600 px bredt).
4. Ret eller slet eksempelindlæggene i `public/nyheder/`.

## Nyt nyhedsindlæg

1. Kopiér `public/nyheder/velkommen.html` til en ny fil, fx `public/nyheder/vinterklargoering.html`.
2. Ret titel, dato og tekst.
3. Tilføj et `<a class="news-item">`-kort øverst i `public/nyheder/index.html`, og evt. på forsiden.

## Udseende

Al styling ligger i `public/style.css`. Farver ændres øverst under `:root`.

## Deploy

Sitet er bare filer, så deploy er at kopiere `public/` til webserverens rodmappe. Eksemplet bruger `rsync` til `/srv/kanalkanten` på en server, men det virker med enhver webserver (Caddy, nginx, Apache m.fl.) der serverer den mappe.

```sh
# Tør-kørsel først: viser hvad der ville ændres, uden at gøre det
rsync -avzn --delete public/ bruger@server:/srv/kanalkanten/

# Selve deploy
rsync -avz --delete public/ bruger@server:/srv/kanalkanten/
```

Bemærk:
- **Efterstillet `/` på `public/`** er vigtigt. Uden den havner filerne i `/srv/kanalkanten/public/`.
- **`--delete`** fjerner filer på serveren, som ikke længere findes i repoet, så serveren altid matcher repoet. Brug tør-kørslen først.
- Brugeren på serveren skal have skriveadgang til `/srv/kanalkanten`, og filerne skal kunne læses af webserveren.

### Eksempel på webserver-opsætning (Caddy)

```
kanalkanten.example.dk {
    root * /srv/kanalkanten
    file_server
    encode gzip
}
```

## Bidrag

Rettelser og nye indlæg er velkomne som pull requests. Husk at repoet er offentligt: læg ikke personoplysninger (navne på beboere, telefonnumre, mailadresser, interne dokumenter) i repoet eller i `public/`.
