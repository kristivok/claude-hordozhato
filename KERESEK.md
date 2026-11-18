# Kéréseim – állandó munkamód-kérések

Az általam kifejezetten kért, minden projektre érvényes munkamód, általánosítva. Mindegyikből szabály lett (azonosítók: `SZABALYOK.md`, tömören: `CLAUDE.md`). A projektspecifikus kérések itt nem szerepelnek.

| # | Kérés (összefoglalva) | Szabály |
|---|---|---|
| 1 | Minden válasz ultra-tömör („caveman”), magyarul is; részletesebben csak kérésre. | KOM-01 |
| 2 | Kód csak privát tárhelyre; publikus csak külön, kifejezett engedéllyel (repó, gist, release, Pages sem). | VER-03 |
| 3 | Élesítésnél a meglévő tárhely többi részéhez nem szabad nyúlni. | HITEL-04 |
| 4 | Adminnak kötelező a 2FA. | BIZT-07 |
| 5 | Titkos kulcs nem kerülhet webes felületre. | BIZT-08 |
| 6 | Kérések összegyűjtése táblába, terv; annyi agent, amennyi kell; skillek hozzárendelése agentenként (a fő agentnek is); tesztek, teljes ellenőrzés, záró biztonsági audit. | TERV-01, MUN-01, MUN-02, BIZT-01 |
| 7 | Teljesen reszponzív felület; telefonon a fő nézetet ne vegyék el a menük. | UI-01 |
| 8 | Más felhasználó jogosultsági szintjét csak admin lássa. | ADAT-02 |
| 9 | A naplók ne a webszerveren gyűljenek. | ÜZEM-06 |
| 10 | Ne legyen ismétlődő menünév (kreatív, egyedi nevek). | UI-06 |
| 11 | Minden módosítás után verzió, push és kódgráf-frissítés; köztes push; dokumentált alfeladatok, hogy a keret elfogyásakor folytatható legyen. | VER-01, VER-02, TERV-02 |
| 12 | A személyes kéréslista-fájl csak helyben marad. | VER-04 |
| 13 | Automatikus, ellenőrzött mentés. | ÜZEM-04 |
| 14 | Ne törjenek a gombok és címek (tördelés-felmérés). | TESZT-03, UI-02 |
| 15 | Mindig párhuzamos agentek, a feladathoz illő skillekkel; kódgráf a kereséshez; token-hatékonyság minden válasznál és kódgenerálásnál. | MUN-01, MUN-05, MUN-06 |
| 16 | Kártyás oldalak rés nélkül, dinamikusan; skeleton betöltés; ⓘ-tippek. | UI-02, UI-05 |
| 17 | Tesztkörnyezet élesítés előtt; nagy tervet előbb tervező, aztán átvizsgáló agent. | ÜZEM-01, TERV-05 |
| 18 | Minden részletes elem felugró ablakban; egysoros, kompakt listák; kattintható kereszthivatkozások. | UI-03, UI-04 |
| 19 | Teljes átvizsgálás több agenttel, többkörös beszélgetésben, + teszter-agent; minden javítás elvégzése. | MUN-07, MUN-08, BIZT-01 |
| 20 | Élesítés csak kifejezett jóváhagyással. | HITEL-03 |
| 21 | A munkamód-tudás külön repóban legyen, ne a projektrepóban. | – |
| 22 | A hibák és tévedések rögzüljenek egy tanulságnaplóban, és minden agent ezek szerint dolgozzon, hogy ne ismétlődjenek. | TAN-01…04 |
| 23 | Hordozható, publikus memória más gépekre (pl. munkahelyi környezet): érzékeny adat nélkül, a forrásgép frissíti, a többi gép időnként ellenőrzi, és jóváhagyással lehúzza. | TAN-05, FRISS-01…04 |
