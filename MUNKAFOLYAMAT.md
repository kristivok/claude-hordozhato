# Munkafolyamat – így dolgozott a koordinátor és az agent-csapat

Esettanulmány: egy webes projekt teljes átvizsgálása. A feladat: „nézd át 5 agenttel a teljes weboldalt, legalább 10 kör beszélgetésben elemezd velük, mi kell még, javítsd is ki, és vonj be egy teszter-agentet”.

Az eredmény: minden magas és közepes súlyosságú lelet javítva, jelentősen bővült tesztsor, élesítés automatikus utóellenőrzéssel.

A szabályok azonosítói (pl. MUN-03): [`SZABALYOK.md`](SZABALYOK.md). A sablonok: [`sablonok/`](sablonok/).

## 1. Szerepek

| Szerep | Ki | Feladat | Amit NEM csinál |
|---|---|---|---|
| Koordinátor | fő agent | Előkészítés, körök kiadása, döntések, integráció, teljes tesztsor, commit, push, deploy, dokumentáció. | Nem ír hosszú kódot, ha agent is megírhatja; apróságot viszont helyben elintéz. |
| Szakterületi agentek (5) | AUTH (hitelesítés, munkamenet, kripto), AKL (jogosultság, adatszivárgás), INP (bemenet, fájlok, SSRF, mentés), FRONT (frontend-biztonság, UX, a11y), OPS (üzemeltetés, integritás, deploy, megfigyelés) | Átvizsgálás, kereszt-ellenőrzés, javítás a saját fájlkörben, kereszt-review. | Nincs git, deploy, gráf-frissítés és böngésző; idegen fájlt nem szerkeszt. |
| Teszter | külön agent | A futó alkalmazás dinamikus tesztelése böngészővel és HTTP-vel, minden szerepkörrel; regressziós csomag. | Nem szerkeszt kódot. |

A szakterületek úgy vannak kijelölve, hogy a fájlkörök a lehető legkevésbé fedjék egymást: egy fájlnak egy gazdája van.

## 2. Előkészítés (a koordinátor, az első kör előtt)

1. **Kontextus:** szabálytár, memória, feladatnapló, kódgráf; a skill-fájlok útvonalának összegyűjtése.
2. **Közös eligazítás** egyetlen fájlban (`BRIEF.md`, sablon: [`sablonok/AGENT-BRIEF.md`](sablonok/AGENT-BRIEF.md)): projektleírás, első olvasmányok, kötelező szabályok (mit nem szabad), tesztparancsok, jelentésformátum. Az agent-prompt így rövid: „olvasd a BRIEF-et + a te területed”.
3. **Agent-azonosítók rögzítése** fájlban (`AGENTS.md`), hogy a körök SendMessage-dzsel folytathatók legyenek, egy esetleges kontextus-tömörítés után is.
4. **Tesztkörnyezet a teszternek:** a helyi adat másolata külön tárolóba, ideiglenes engedmények (pl. 2FA-kényszer ki) csak a másolatban, saját port.
5. **Tiszta kiindulás:** a függő munka commitolva, így a későbbi diffek csak az agentek munkáját mutatják.

## 3. A 10 kör

| Kör | Cél | Ki mit csinál | Kimenet |
|---|---|---|---|
| 1 | Független átvizsgálás | 5 agent csak olvas a saját területén; a teszter dinamikusan hibát keres. | `<KÓD>-r1.md` – egy lelet egy sor |
| 2 | Kereszt-ellenőrzés | Mindenki **mások** leleteit próbálja megcáfolni (doubt-driven); a teszter a kódból talált leleteket reprodukálja. | Megerősítve / cáfolva / részben, új súlyosság, duplikátumok |
| 3 | Javítási terv | A koordinátor dönt (mi javul, mi elfogadott), és diszjunkt fájlkört oszt ki; az agentek leletenként tervet adnak (fájl:függvény, teszt), az idegen fájlba kellő változtatást „KÉRÉS →” sorban. A teszter regressziós csomagot készít. | `<KÓD>-r3.md` + `REGRESSZIO.md` |
| 4 | Megvalósítás | Párhuzamosan, a saját fájlkörben, saját tesztekkel; menet közben szűrt tesztfutás, a végén a teljes sor. | Kód + tesztek; a koordinátor teljes futása |
| 5 | Kereszt-review | Körbeforgó párosítás (pl. AUTH→INP, AKL→AUTH, INP→AKL, OPS→FRONT, FRONT→OPS) a koordinátor által kimentett patch-fájlokon. A teszter lefuttatja a regressziót a javított kódon, majd szabad támadó próbát végez. | blokkoló / javítandó / apróság |
| 6 | Review-javítás | A gazda javítja a saját fájljában talált review-leleteket. | Zöld teljes sor |
| 7 | Teszter-leletek javítása + 2. átvizsgálás | A javítók a teszter új leleteit javítják; két agent a még nem érintett területeket nézi át. | Új leletek + ötletek |
| 8 | Javítás + teszter-ellenőrzés | Javítás, közben a teszter ellenőrzi az előző kör változásait. | PASS/FAIL lista |
| 9 | 2. kereszt-review | Csak a 6–8. körben született változásokra (az 5. kör már átnézte a többit); utolsó apró UI-javítások. | + „maradék kockázat a területemen” |
| 10 | Zárás | Záró javítások → koordinátor: teljes sor, verzió, commit → a teszter végső füstpróbája → élesítés jóváhagyással, utóellenőrzés. | Kiadás, `AUDIT2.md` |

Minden kör utasítása egy `ROUND<n>.md` fájlba kerül, az agentnek csak egy rövid üzenet megy: „olvasd a ROUND<n>.md-t, a te sorod: …” (sablon: [`sablonok/KOROK.md`](sablonok/KOROK.md)).

## 4. Koordinátori döntések (amit nem az agentek döntenek el)

- **Duplikátumok összevonása:** egy ID marad, a többi visszavonva (pl. AUTH-02 = AKL-05).
- **Elfogadott kockázat:** dokumentálva, nem javítva.
- **Elvi döntés a szabályok alapján:** ha egy agent javaslata kötelező szabályba ütközik (pl. BIZT-08), elutasítva, indoklással.
- **Kockázatos konfig-változás** (pl. webszerver-konfiguráció): csak élesítés utáni automatikus ellenőrzéssel és kész visszaállítási lépéssel.
- **Fájl-gazdaság átadása:** ha egy javítás gazda nélküli fájlt érint, a koordinátor kifejezetten kiosztja.

## 5. Integráció (a koordinátor, minden javító kör után)

1. Teljes tesztsor (minden nyelv tesztjei + lint + biztonsági lint), a számokkal együtt jelentve.
2. Sorvég- és BOM-ellenőrzés az összes változott fájlon (az agentek gyakran CRLF-et vagy BOM-ot írnak).
3. Commit területenként (a fájl → gazda leképezéssel), értelmes üzenettel, `Co-Authored-By` sorral.
4. Patch-fájlok kimentése a következő review-körhöz (`git show <commit> > final-<KÓD>.patch`), mert az agentek nem futtatnak git-et.
5. A végén: verzióemelés, tag, privát push, gráf-frissítés, feladatnapló, audit-dokumentum, memória.

## 6. Élesítés

1. Hálózat nélküli önteszt.
2. Élesítés **csak kifejezett jóváhagyással** (HITEL-03). Az automatikus engedély-ellenőrző egyszer meg is állította a deployt: nem kerülgettük, hanem megkérdeztük a felhasználót.
3. Utóellenőrzés: ping 200, védett utak 403, cache-fejlécek (`immutable` / `no-cache`), titokfájl 403, és piros eredmény esetén visszaállítási útmutató.
4. Böngészős füstpróba élesben (belépőoldal, konzol).
5. Utána a tesztkörnyezet kódja is frissül.

## 7. Tapasztalatok, csapdák

| Csapda | Mi történt | Megoldás |
|---|---|---|
| Használati keret | 6 agent, agentenként 600–700 ezer token kontextus; a 7. kör elején elfogyott az 5 órás keret. | A szűk körök és rövid jelentések segítenek. A keret visszaállása után az agentek SendMessage-dzsel folytathatók; az elvégzett munkájuk megmaradt. |
| Kontextus-tömörítés | A koordinátor hosszú beszélgetése tömörítődhet. | A fájlba írt állapot (`AGENTS.md`, `ROUND*.md`, `<KÓD>-r*.md`) miatt nem vész el semmi. |
| CRLF/BOM | Az agentek Windows-sorvéget vagy BOM-ot írnak. | Ellenőrzés integráláskor; a BOM csak ott marad, ahol kell (PowerShell 5.1). |
| Teszt-port | A teszt-adatmásolat konfigjában más port volt, mint amit a szerver használt, így a CSP minden assetet blokkolt. | A tesztkörnyezetet a konfig `base_url`-jével egyező porton kell futtatni. |
| Időfüggő tesztek | Másodperc-határon bukó teszt. | Rögzített „most” időpont a tesztben. |
| Kódolási hiba a dokumentációban | A feladatnapló egy része cp1250-ként kétszer kódolva (mojibake) került be. | Soronkénti visszakódolás, ellenőrzés. |
| Ékezetes útvonal | A PHP-tesztek Git Bashből elromlanak. | PowerShellből vagy ékezetmentes útvonalról futtatni. |
| Párhuzamos szerkesztés közbeni tesztfutás | Egy agent teljes futása más agent félkész munkáján bukott. | Menet közben szűrt tesztek; teljes futás a kör végén. Az idegen bukást jelenteni kell, nem javítani. |
| Agent „feltételezése” | Néhány lelet a kódolvasás alapján valósnak tűnt. | A 2. kör (cáfolási kísérlet + teszter-reprodukció) kiszűri; minden lelethez fájl:sor kell. |

## 8. Amit a felhasználó a végén megkap

- Mi változott (területenként), mi a teendője (pl. egyszeri kézi lépések, élesítés utáni ellenőrzés), mi maradt nyitva (maradék kockázat), fejlesztési ötletek.
- Dokumentáció: az audit-összefoglaló (leletek, állapot, maradék kockázat, ötletek), a feladatnapló bejegyzése, a biztonsági audit fejezete, és egy üzemeltetési gyorslap.
