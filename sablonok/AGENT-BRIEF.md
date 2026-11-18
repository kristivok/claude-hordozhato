# Közös eligazítás – <projekt> <feladat>, <kiinduló verzió> → <cél verzió>

> Sablon. Egyetlen fájl, amit minden agent elsőként olvas. Tedd a session munkamappájába (pl. `<SP>\audit\BRIEF.md`), ne a repóba. A `<…>` részeket töltsd ki.

Projekt: `<projekt útvonala>` (éles: <URL vagy „nincs”>). Stack: <nyelvek, keretrendszer, adatbázis>.
Mi védendő: <személyes adatok, szerepkörök, titkos kulcsok, külső szolgáltatások>.
Frontend-szabályok: <pl. szigorú CSP, nincs inline, DOM csak biztonságos helperrel>.

## Első olvasmány (csak ami a területedhez kell)
- Kódkeresés előtt `graphify query "<kérdés>"` (a projektgyökérből); nyers fájlt csak a megtalált sorokra olvass. Navigáció: <wiki / kódtérkép útvonala>.
- <szerződés / API-leírás>, <előző audit – amit ott már javítottak vagy elfogadtak, azt ne hozd újra, hacsak nem regresszió>, <egyéb tervek>.
- A promptban megadott skill-fájlokat olvasd el (útmutatónak; szkriptet nem futtatunk belőlük).

## Szabályok (kötelező)
- NE futtass git-et, deployt vagy gráf-frissítést. Az éles rendszerhez NE küldj kérést – csak helyi kód + helyi devserver.
- NE olvasd: <konfig titkokkal>, <hitelesítő tároló>, szolgáltatásfiók-JSON, kulcs, jelszó. Titkot ne írj ki.
- Szerkeszteni CSAK akkor szabad, ha a koordinátor kiosztja a fájlt. Addig csak olvasás.
- Szerkesztésnél: LF sorvég, BOM nélkül, a meglévő stílus, minimális (sebészi) diff; új viselkedéshez teszt.
- Tesztek: <JS-parancs>; <PHP/egyéb parancs, honnan futtatandó>. Menet közben csak a saját tesztjeidet futtasd szűrővel, a végén egyszer a teljes sort; ha más területén bukik, jelezd, ne javítsd.
- Böngészőt CSAK a TESZTER használ.
- Nyelv: magyar, tömör. Nincs bevezető, nincs összefoglaló-ismétlés.

## Jelentésformátum (minden körben)
Egy lelet = egy sor:
`<ID> | <krit/magas/közepes/alacsony/ötlet> | <fájl:sor> | <hiba egy mondatban> | <konkrét kihasználás / hibás forgatókönyv> | <javaslat> | <ráfordítás S/M/L>`
- Az ID-előtag a területed kódja (pl. AUTH-03).
- Csak ellenőrzött lelet (a kódban megnézted); a bizonytalant jelöld: `(feltételezés)`.
- Legfeljebb ~25 lelet, súlyosság szerint rendezve. A végén 1–3 sor fejlesztési ötlet.
- Ha egy lelet más terület fájljait érinti: `→<KÓD>`.
- Ha visszatérő hibába futottál (környezet, eszköz, szintaxis), vagy tévedést javítottál: a jelentés végén `TANULSÁG: <jel (hibaüzenet-részlet)> → <teendő>`. A tanulságnaplóba nem írsz; azt a koordinátor teszi meg (TAN-02).

A jelentést mentsd ide: `<SP>\audit\<KÓD>-r<kör>.md`, és a válaszod is ez legyen (max ~60 sor).
