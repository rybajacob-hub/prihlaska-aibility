# REDEPLOY — jak znovu nahrát aktualizovaný build

Živá URL: https://rybajacob-hub.github.io/prihlaska-aibility/
Repo: https://github.com/rybajacob-hub/prihlaska-aibility (public)

## Co se publikuje (a co NE)

Publikuje se JEN postavená landing + soubory, na které odkazuje relativně:

- `index.html`            ← z iCloudu `landing-prihlaska/index.html` (POZOR: fixne se `../landing/` → `./landing/`)
- `prvni-30-dnu.md`
- `_flow-loop-engineering.md`
- `_pribeh.html`
- `landing/index.html`    ← z iCloudu `landing/index.html`
- `ukazky/newsletter-01.md`
- `ukazky/linkedin-post.md`

NIKDY nekopíruj: `NAPADY.md`, `KONTEXT.md`, `STAV.md`, `ZMENY-*`, `hodnoty-*`,
`prihlaska-expert-panel-*`, `oponentura-*`, `podklady-*`, `airtable-*`, `MEMORY*`,
`memory/`, `_jak-vznikala-preview.html`, `ukazky/*expert-panel*`, `ukazky/*anotovany*`,
`ukazky/*vznik*`, `ukazky/*fact*`, cokoliv s tajnými firemními čísly.

## Pozn.: download odkazy (prihlaska.md / prvni-30-dnu.md)

Tlačítka „⬇️ prihlaska.md" a „⬇️ prvni-30-dnu.md" NEstahují soubory ze serveru —
markdown je vložený přímo v `index.html` (v `<script type="text/markdown">`) a stahuje
se jako Blob přes JavaScript. Takže i po přesunu do rootu fungují bez `.md` souborů na serveru.

## Re-push po finální revizi obsahu

POZOR: `.git` v iCloudu je rozbitý — v iCloud složce NEspouštěj git.
Pracuj vždy jen v této čisté staging složce mimo iCloud.

```bash
SRC="$HOME/Library/Mobile Documents/com~apple~CloudDocs/Second-Brain/1-Projects/_Aibility"
DEST="$HOME/Projects/prihlaska-aibility-pages"

# 1) Přenes aktualizované soubory z iCloudu (nezasahuje do .git v DEST)
rsync -a "$SRC/landing-prihlaska/index.html"        "$DEST/index.html"
rsync -a "$SRC/prvni-30-dnu.md"                      "$DEST/prvni-30-dnu.md"
rsync -a "$SRC/_flow-loop-engineering.md"           "$DEST/_flow-loop-engineering.md"
rsync -a "$SRC/_pribeh.html"                         "$DEST/_pribeh.html"
rsync -a "$SRC/landing/index.html"                   "$DEST/landing/index.html"
rsync -a "$SRC/ukazky/newsletter-01.md"             "$DEST/ukazky/newsletter-01.md"
rsync -a "$SRC/ukazky/linkedin-post.md"             "$DEST/ukazky/linkedin-post.md"

# 2) Znovu fixni relativní odkaz na kampaň (root deploy)
cd "$DEST"
sed -i '' 's#href="\.\./landing/"#href="./landing/"#' index.html

# 3) Rychlá kontrola, že neuniklo nic tajného
find . -path ./.git -prune -o -type f -print | \
  grep -iE 'NAPADY|KONTEXT|STAV|ZMENY|hodnoty-|expert-panel|oponentura|podklady-|airtable-|MEMORY|git-broken|jak-vznikala|anotovany|vznik|fact' \
  && { echo "!!! TAJNY SOUBOR — NEPUSHOVAT !!!"; exit 1; } || echo "OK: cisto"

# 4) Commit + push
git add -A
git commit -m "Aktualizace obsahu prihlasky"
git push
```

GitHub Pages se po pushi sám přebuilduje (~30–90 s).
