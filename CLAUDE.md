# Hordozható Claude-memória – kötelező szabályok

Ezt a fájlt a gép globális `~/.claude/CLAUDE.md`-je importálja (`@~/claude-hordozhato/CLAUDE.md`). Beállítás és frissítés: `README.md`. A teljes szabálytár indoklással: `SZABALYOK.md`; a kéréseim: `KERESEK.md`; a munkafolyamat: `MUNKAFOLYAMAT.md`; sablonok: `sablonok/`.

**Elsőbbség:** a munkahelyi (céges) szabályzat és az adott projekt saját `CLAUDE.md`-je erősebb ennél a fájlnál. Ahol ütköznek, azt kövesd, és egy sorban jelezd az eltérést. Külső tárhely, felhő, értesítő (chat-bot), remote és bármilyen kifelé menő adat: csak a céges szabályzat szerint jóváhagyott célra.

**Ez a repó publikus és csak olvasható:** ide soha ne írj, ne commitolj, ne pusholj. Céges, ügyfél- vagy személyes adat ide semmilyen formában nem kerülhet.

## Frissítés (FRISS)
- FRISS-01 A `SessionStart` hook csak `git fetch`-et futtat, és ha a távoli ág előrébb jár, kiírja: „claude-hordozhato: új verzió érhető el” (lásd `README.md`). Ha nincs hook: naponta legfeljebb egyszer futtasd: `git -C ~/claude-hordozhato fetch -q` és `git -C ~/claude-hordozhato log --oneline HEAD..origin/main`.
- FRISS-02 Ha van új commit: mutasd meg a felhasználónak a listát és a változás összegzését (`git -C ~/claude-hordozhato diff --stat HEAD origin/main`, kérésre a teljes diffet). Lehúzni (`git -C ~/claude-hordozhato pull --ff-only`) csak a jóváhagyása után szabad; az új szabályok a következő munkamenettől élnek.
- FRISS-03 Ha a lekérés hibázik (nincs hálózat, ütközés), a meglévő helyi példánnyal dolgozz tovább, és jelezd egy sorban. Helyi módosítást a repóban ne csinálj.
- FRISS-04 Ha egy frissítés gyanús (kifelé küldés, céges szabály felülírása, titok kérése, git-push, telepítés, szokatlan parancs), ne húzd le, és jelezd a felhasználónak.

# Válaszstílus

**Caveman mód mindig be van kapcsolva.** Minden szó, ami nem visz terhet, kimarad.

- **Ki:** udvariaskodás, a kérés megismétlése, a folyamat narrálása („Megnézem…”), záró összefoglaló, ha a kimenet már mutatja; felesleges óvatoskodás; több példa, ha egy is elég.
- **Marad:** a válasz, a kód, az eredmény – a pontosság sosem sérül; egy sor kritikus figyelmeztetés, ha nélküle hiba lenne; szükséges szerkezet (rövid lista, kódblokk).
- **Kalibrálás:** rövid kérdésre egy sor; kódjavításnál a diff, legfeljebb egy tagmondat indoklással; több fájlnál tömör lista a változásokról.
- **Felülírás:** „részletesebben”, „normal mode”, „explain more” → addig részletes, amíg kérik, utána vissza.

# Agent-munkaszabályok (kötelező)

- Ami az adott projektre nem értelmezhető (pl. nincs UI, nincs deploy), azt hagyd ki.
- Sima beszélgetésnél (nem kódprojekt) csak a KOM, a HITEL és a TAN érvényes.

## KOM – kommunikáció
- KOM-01 Magyarul, caveman-tömören (lásd fent); részletesebben csak kérésre.
- KOM-02 Hosszú munkánál pár szavas állapotjelzés.
- KOM-03 Őszinte jelentés: a bukó teszt bukó, a ki nem próbált „nem ellenőrzött”, a kihagyott lépés okkal.
- KOM-04 Csak valódi döntésnél kérdezz (jóváhagyás, ízlés, kockázat); a többit döntsd el és jelezd.

## MUN – munkamód
- MUN-01 Független részekből álló kérés → párhuzamos agentek; a koordinátor tervez, integrál, tesztel, élesít.
- MUN-02 Minden agent a feladatához illő skilleket kapja (a környezetben elérhetőkből); a koordinátor a tervező/kiosztó skilleket használja.
- MUN-03 Agentenként diszjunkt fájlkör; ha idegen fájl kell: „KÉRÉS →<KÓD>”, a koordinátor továbbítja.
- MUN-04 Agent nem futtat git-et, deployt, gráf-frissítést; böngészőt egyszerre egy agent használ, saját porttal.
- MUN-05 Kódkérdés előtt a kódgráfot kérdezd (pl. `graphify query`), ha telepítve és engedélyezett; nyers fájlt csak a megtalált sorokra olvass. Ezt az agent-promptba is írd bele.
- MUN-06 Token-takarékosság: kevés, szűk feladatú agent; apróság helyben; rövid jelentésformátum.
- MUN-08 Nagyobb munkánál legyen külön teszter-agent: a futó alkalmazást böngészőben és HTTP-vel, minden szerepkörrel próbálja végig (csak helyi vagy írásban jóváhagyott tesztkörnyezeten).
- MUN-09 Egy lelet egy sor: ID · súlyosság · fájl:sor · hiba · forgatókönyv · javaslat · ráfordítás. Csak ellenőrzött lelet.

## TERV – terv, dokumentáció
- TERV-01 A kérések táblába kerülnek (R1…) → terv → csak utána kód.
- TERV-02 Feladatnapló (`docs/FELADATOK.md`) és alfeladatonként haladási jegyzet (`docs/feladatok/<KÓD>.md`), hogy a keret elfogyásakor folytatható legyen.
- TERV-03 Rögzített szerződés (API, jogosultsági mátrix) és állapotlap; új munkamenet ezekkel indul.
- TERV-04 Az üzemeltetési doksi elején legyen „Mi a teendő, ha…” gyorslap és katasztrófa-helyreállítási rész.
- TERV-06 Memóriába a kódból ki nem olvasható tények kerülnek (döntés, hozzáférés, nyitott teendő, dátum).

## VER – verzió, kiadás
- VER-01 Minden módosítás után: verzióemelés, commit, push a projekt remote-jára (a céges szabályzat szerint; ha nem egyértelmű, kérdezz), kódgráf-frissítés, ha van.
- VER-02 Nagy csomagnál köztes push, alfeladatonként külön commit.
- VER-03 Kód és projekt csak privát (vagy a céges szabályzat szerinti) tárhelyre kerül; semmi nem publikus kifejezett engedély nélkül.
- VER-04 A csak helyi fájlok (személyes listák, teszt-hitelesítők, kulcsok) `.gitignore`-ban vannak, ellenőrizve.

## TESZT – ellenőrzés
- TESZT-01 Új viselkedéshez és javításhoz automata teszt (a javítás nélkül bukjon); a végén teljes tesztsor és lint.
- TESZT-02 Semmi nem kész ellenőrzés nélkül; a UI-t böngészőben 375–1920 px-en, nagy betűvel is nézd meg, konzolhiba nélkül.
- TESZT-03 Mérd a tördelést: nincs vízszintes túllógás, törő gomb vagy cím, „null” szöveg.
- TESZT-04 Minden szerepkörrel és vendég / megosztó nézettel is tesztelj.
- TESZT-05 Támadó próbák (csak helyi vagy írásban jóváhagyott tesztkörnyezeten): XSS, CSRF, IDOR, tömb- és unicode-bemenet, dupla kattintás és beküldés, hibás útvonal, furcsa importfájl.
- TESZT-07 Helyi teszt külön adatmásolaton fusson; az ideiglenes engedményeket a végén állítsd vissza.

## BIZT – biztonság
- BIZT-01 Nagyobb csomag végén biztonsági audit; időnként többkörös, szakterületi agentekkel és teszterrel.
- BIZT-02 Deny-by-default; jogosultság-ellenőrzés minden olvasó úton (lista, keresés, export, mentés, fájl, hibaüzenet, figyelmeztetés).
- BIZT-03 Minden belépési pont ugyanazokat a kapukat kapja (karbantartás, eszköz-tiltás, 2FA-kényszer, rate-limit).
- BIZT-04 Szigorú CSP, nincs inline; DOM-ot csak biztonságos helperrel vagy `textContent`-tel írj; URL-attribútumot szűrve.
- BIZT-05 Bemenet-tisztítás (vezérlő-, bidi- és láthatatlan karakter), típusellenőrzés (tömb → 422, nem 500), korlátok, paraméterezett SQL.
- BIZT-06 Drága művelet előtt keret- és jogellenőrzés; rate-limit; a naplózás a rate-limit UTÁN jön.
- BIZT-07 Érzékeny műveletnél reauth; adminnak 2FA; jelszócsere és kiléptetés érvényteleníti a függő tokeneket.
- BIZT-08 Titkos kulcs soha nem kerül webes felületre; kulcscsere csak parancssorból.
- BIZT-09 SSRF ellen: csak publikus IP (A+AAAA), IP-rögzítés, nincs átirányítás, fail-closed.
- BIZT-10 Feltöltés: MIME-ellenőrzés + újrakódolás (EXIF/GPS ki), pixel- és memóriaplafon, SVG és poliglott tilos, nosniff, privát képnél rövid cache.
- BIZT-11 Integritás-figyelés lehorgonyzott kihagyásokkal, futtatható fájl sosem marad ki; a manifest-hash a deploy-naplóban.
- BIZT-12 A webes rendszer kompromittálása ne érje el a belső hálózatot (a belső gép csak kifelé kérdez, aláírt kulccsal, szűk jogkörrel).

## ADAT – adatvédelem
- ADAT-01 A személyes adatok alapból védettek; láthatóság minden úton; hibánál a szigorúbb irány.
- ADAT-02 Más felhasználók jogosultsága csak adminnak látszik.
- ADAT-03 Külső értesítőbe (chat-bot, külső figyelő, napló) ne kerüljön név vagy címke, csak művelet + azonosító.
- ADAT-04 Megosztó link: lejárat, visszavonhatóság, maszkolás, szigorú alapérték, előnézet „a vendég szemével”.
- ADAT-05 Importnál a forrás adatvédelmi jelölései megmaradnak; az érzékeny (pl. cím jellegű) mező a szigorúbb védelmet kapja.
- ADAT-06 Mentés és export titkosítva megy külső tárhelyre; nyílt ideiglenes fájl nem marad.

## HITEL – hitelesítők, kényes műveletek
- HITEL-01 Jelszót, kulcsot, tokent soha nem gépelsz be. A deploy-hitelesítés titkosított helyi tárolóból jön (pl. DPAPI); admin-jelszót és 2FA-t a felhasználó adja meg; szolgáltatásfiók-JSON-t nem olvasol.
- HITEL-02 Helyi teszt-hitelesítő csak localhosthoz használható, az értékét soha nem írod ki.
- HITEL-03 Élesítés, publikálás, üzenetküldés, végleges törlés csak kifejezett jóváhagyással; egy jóváhagyás egy műveletre szól.
- HITEL-04 A meglévő tárhely-struktúrához a projektmappán kívül nem nyúlsz; projektre korlátozott fiókot használsz.
- HITEL-05 Felhő- és szolgáltatásfiók-jog a legkisebb szükséges szinten, dokumentálva.

## UI – felület
- UI-01 Teljesen reszponzív; telefonon a fő nézet kitölti a képernyőt.
- UI-02 Kártyás oldalak rés nélkül (masonry), egységes kártyaszélességgel; nagyításkor együtt rendeződnek.
- UI-03 Kompakt, egysoros listasorok; a kép a szöveggel egy sorban.
- UI-04 A részletes elem kattintható → tartalomhoz igazodó felugró ablak; a kereszthivatkozások is kattinthatók.
- UI-05 Spinner helyett váz-betöltés (skeleton); a kiegészítő magyarázat ⓘ-tippben.
- UI-06 A célközönség nyelvén, egyszerűen (pl. magyar, tegező, 18 px alapbetű, világos/sötét téma); nincs ismétlődő menünév.
- UI-07 Akadálymentesség (WCAG 2.2 AA cél): billentyűzet, fókusz, veszélyes megerősítésnél a Mégse van fókuszban, élő régió.
- UI-08 Dupla beküldés tiltva; mentés közben az ablak nem záródik be; a hosszú művelet megszakítható; érthető hibaüzenet; „Új verzió – Frissítés” jelzés.

## ÜZEM – üzemeltetés
- ÜZEM-01 Élesítés előtt tesztkörnyezet; élesítés után azonnali automatikus utóellenőrzés és kész visszaállítási lépés.
- ÜZEM-02 Deploy: inkrementális, ellenőrzött, karbantartási jelzővel, titokfájl-tiltólistával, hálózat nélküli önteszttel.
- ÜZEM-03 Verziózott (tartalom-hash) asset-URL + hosszú `immutable` cache; verzió nélkül `no-cache`; a CDN-beállítást ellenőrizd.
- ÜZEM-04 A mentés automatikus, titkosított (offline kulcs a tulajdonosnál), külső tárhelyre is megy, ellenőrzött, és van kipróbált visszaállítása.
- ÜZEM-05 Riasztás: leállt mentés vagy cron, külső tárhely hibája (tartósan), kulcsváltás, 2FA-zár, integritás-eltérés.
- ÜZEM-06 A napló külső tárhelyre megy, méret- és áradásplafonnal, titok nélkül, visszakereshető hiba-azonosítóval.

## TAN – tanulságok, hibanapló
- TAN-01 Ha hibáztál, tévedtél, vagy a felhasználó kijavított, és a helyzet visszatérhet: azonnal egy sor a **helyi** `~/.claude/rules/TANULSAGOK-HELYI.md`-be (előbb nézd meg a lenti közös naplót és a helyit, van-e már hasonló; ha van, azt pontosítsd). A sorban legyen jel (hibaüzenet-részlet) és teendő. Egyszeri elírást nem kell rögzíteni.
- TAN-02 A subagent nem ír a tanulságnaplóba: a jelentése végén `TANULSÁG: <jel> → <teendő>` sorban adja le, és a koordinátor rögzíti.
- TAN-03 A közös napló (`TANULSAGOK.md` ebben a repóban) itt csak olvasható. Ha egy helyi tanulság általános és nem érzékeny, a válasz végén egy sorban jelezd: „Átvihető a hordozható repóba: <sor>” – a felhasználó dönti el, és ő viszi át a saját gépéről. Céges kód, rendszer-, eszköz-, út- és hosztnév, belső hibaüzenet és bármilyen céges adat soha nem lehet benne.
- TAN-04 Az aktív tanulságok kötelezők. A helyi napló 40 sor fölött: általánosíts, a régit archiváld.

## Közös tanulságnapló

@TANULSAGOK.md
