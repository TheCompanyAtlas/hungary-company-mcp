# Magyar cégnyilvántartás MCP

Connect your AI assistant to about 535,000 Hungarian companies and 1.6 million linked officers and shareholders through [The Company Atlas](https://thecompanyatlas.com) and the Model Context Protocol: search by company name, tax number (adószám), company registration number (cégjegyzékszám), legal form, status, city and TEÁOR activity code, open a company's officers and shareholders, or find the companies linked to a person.

Ez az MCP-kapcsolat lehetővé teszi, hogy az AI-asszisztens magyar cégadatokban keressen:

- Cégek keresése név, adószám, cégjegyzékszám, cégforma, állapot, település, irányítószám, TEÁOR-kód és alapítási dátum szerint
- Cégadatlap a székhellyel, a fő tevékenységgel, valamint az e-mail-címmel, telefonszámmal és weboldallal, ha ezek rendelkezésre állnak
- A cég vezető tisztségviselői és tagjai, szerepkörrel és beosztással (például ügyvezető, igazgatósági tag, cégvezető)
- Személyek keresése név szerint: mely cégekben vezető tisztségviselők vagy tagok

A címek, születési dátumok és üzletrész-adatok nem szerepelnek az adatokban.

Az MCP-kapcsolatot a The Company Atlas biztosítja. Ez a tároló nem futtat külön szervert.

## Csatlakozás

```text
https://thecompanyatlas.com/mcp/hungary
```

Adja hozzá ezt az URL-t egyéni MCP-csatlakozóként a Claude-ban, a Cursorban vagy más, MCP-t támogató alkalmazásban, majd jelentkezzen be Google-fiókkal, amikor a rendszer kéri.

- Átvitel: Streamable HTTP
- Hitelesítés: Google OAuth
- Dokumentáció: [thecompanyatlas.com/tools/hungary-company-registry](https://thecompanyatlas.com/tools/hungary-company-registry)

## Eszközök

### `Hungary_search_companies`

Cégek keresése. A szűrők ÉS kapcsolatban állnak; legalább egyet meg kell adni.

| Paraméter | Típus | Leírás |
|---|---|---|
| `q` | szöveg, opcionális | Cégnév részlete, 3–160 karakter |
| `formCode` | szöveg, opcionális | Cégforma kódja: `09` Kft., `06` Bt., `10` Zrt./Nyrt., `03` Kkt., `11` egyéni cég, `17` fióktelep, `01` alapítvány, `02` egyesület vagy szövetkezet |
| `status` | felsorolás, opcionális | `Aktív`, `Aktív - Végrehajtás alatt`, `Aktív - Felszámolás alatt`, `Inkatív - törölve` |
| `city` | szöveg, opcionális | A székhely címének részlete, például `Debrecen` |
| `postcode` | szöveg, opcionális | Pontos, négyjegyű irányítószám |
| `teaor` | szöveg, opcionális | TEÁOR-kód eleje, 2–4 számjegy, például `62` vagy `5510` |
| `incorporatedFrom` / `incorporatedTo` | dátum, opcionális | Alapítás dátuma, `ÉÉÉÉ-HH-NN` |
| `page` / `limit` | egész szám, opcionális | Lapozás, `limit` legfeljebb 50 (alapértelmezés: 20) |

Visszatérési érték: `{ data, total, page, limit }`. Az `id` a `HU` előtag és az adószám első nyolc számjegye (törzsszám); az `id2` a cégjegyzékszám.

### `Hungary_get_company`

Egy cég teljes adatlapja adószám (8 vagy 11 számjegy, például `23513651-2-13`) vagy cégjegyzékszám (például `01-10-040944`) alapján.

| Paraméter | Típus | Leírás |
|---|---|---|
| `number` | szöveg | Adószám vagy cégjegyzékszám |

A keresési mezőkön túl tartalmazza az elérhetőségeket (`other`: e-mail, telefon, weboldal) és a vezető tisztségviselőket és tagokat (`parties`), szerepkörrel, típussal (magánszemély vagy cég) és beosztással.

### `Hungary_search_people`

Cégek keresése egy vezető tisztségviselő vagy tag neve alapján. A nevet magyar sorrendben, vezetéknévvel kezdve és ékezetekkel kell megadni.

| Paraméter | Típus | Leírás |
|---|---|---|
| `name` | szöveg | Személy vagy cég nevének részlete, legalább 4 karakter |
| `role` | felsorolás, opcionális | Szerepkör, például `director` vagy `shareholder` |
| `page` / `limit` | egész szám, opcionális | Lapozás, `limit` legfeljebb 50 (alapértelmezés: 20) |

Minden találat tartalmazza a cég összefoglaló adatait. A nevek nem egyediek, ezért két találatot csak ellenőrzés után tekintsen ugyanannak a személynek.

## Példák

- *„Budapesti Zrt.-k számítógépes programozás tevékenységgel (TEÁOR 62).”*
- *„Felszámolás alatt álló cégek Debrecenben.”*
- *„2024 óta alapított szállodák (TEÁOR 5510).”*
- *„A Richter Gedeon Nyrt. (01-10-040944) igazgatósági tagjai.”*
- *„Mely cégekben tag Mészáros Lőrinc?”*

## Beállítás — Claude

1. Nyissa meg a [claude.ai/customize/connectors](https://claude.ai/customize/connectors) oldalt
2. Kattintson az **Add custom connector** gombra
3. Adja meg a **Hungary Companies** nevet, és illessze be a `https://thecompanyatlas.com/mcp/hungary` címet
4. Mentse, majd jelentkezzen be Google-fiókkal, amikor a rendszer kéri
