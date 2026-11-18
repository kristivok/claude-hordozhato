# Kör-sablonok (ROUND1…10) és agent-prompt

> Minden kör utasítása egy `ROUND<n>.md` fájlba kerül a session munkamappájában. Az agent csak egy rövid üzenetet kap: „<n>. kör: olvasd a ROUND<n>.md-t – a te sorod: <KÓD>. Mentés: <SP>\audit\<KÓD>-r<n>.md.”

## Első agent-prompt (az 1. körben, agentenként)
```
Te vagy a <KÓD> agent (<terület>) egy <N> fős átvizsgáló csapatban (több körös beszélgetés lesz a koordinátorral; ez az 1. kör).
Először olvasd el: <SP>\audit\BRIEF.md – a szabályai kötelezők.
Skillek (olvasd): <skill-fájl 1>, <skill-fájl 2>, <skill-fájl 3>.
Területed (CSAK OLVASÁS): <fájlok, osztályok, témák felsorolva – mit keressen>.
Cél: valós, kihasználható hibák + megerősítési ötletek. Kimenet: a BRIEF formátuma, ID-előtag <KÓD>-, mentés: <SP>\audit\<KÓD>-r1.md.
```
Teszter-agentnél: helyi devserver külön adatmásolattal és porttal, helyi teszt-hitelesítő (értékét ne írja ki), tesztfiókokat létrehozhat (jelszó fájlba, ne a válaszba), szerepkörök listája, támadó próbák listája (TESZT-05), a végén a böngésző-méretet állítsa vissza.

## ROUND2 – kereszt-ellenőrzés
```
CSAK OLVASÁS / helyi futtatás. Mind a <N> jelentés: <SP>\audit\*-r1.md.
Feladat: a kiosztott IDEGEN leleteket próbáld megcáfolni (kódban/futtatással). Mindegyikre egy sor:
<ID> | MEGERŐSÍTVE / CÁFOLVA / RÉSZBEN | új súlyosság | indoklás (fájl:sor) | javítás pontosítása
Utána: duplikátumok, és a saját r1-leleteid visszavonása/átsorolása.
Kiosztás: <KÓD> ellenőrzi: <ID-k> … | TESZTER dinamikusan reprodukálja: <ID-k> (REPRODUKÁLVA / NEM + lépések).
```

## ROUND3 – javítási terv + fájlkiosztás
```
Még NEM szerkesztesz. Döntések: <mi javul, mi elfogadott, elvi döntések>.
Kiosztás (fájlkör → leletek): <KÓD>: <fájlok> (<ID-k>) …
Válasz: leletenként `<ID> | fájl:függvény | a változás lényege | teszt`, utána „KÉRÉS →<KÓD>” sorok (idegen fájlba kellő snippet), kockázat / visszafelé-kompatibilitás (kell-e migráció).
TESZTER: regressziós lista (ID → ellenőrző lépés, elvárt eredmény) + újrafuttatható szkript; még ne futtasd.
```

## ROUND4 – megvalósítás
```
Szerkesztés engedélyezve, CSAK a saját fájlkörödben. Koordinátori döntések / kiegészítések: <…>.
Munkaszabályok: LF, BOM nélkül; menet közben szűrt teszt; a végén teljes sor; idegen fájl → KÉRÉS; nincs git/deploy.
Jelentés: ID → KÉSZ / RÉSZBEN / KIHAGYVA (ok) | fájlok | tesztek + eredmény | KÉRÉS-sorok | ismert maradék.
```

## ROUND5 – kereszt-review
```
CSAK OLVASÁS. Diffek: <SP>\audit\diff-<KÓD>.patch. Kiosztás (körbeforgó): <A>→<B diffje> …
Formátum: <REV-ID> | blokkoló / javítandó / apróság | fájl:sor | hiba | javaslat – vagy „nincs blokkoló”. Stílus-ízlésről nem írsz.
TESZTER: regressziós csomag a javított kódon + szabad támadó próba az új viselkedésen.
```

## ROUND6 – review-javítás
```
A review-leletek javítása a gazda fájljában. Kiosztás: <KÓD>: <REV-ID-k> (+ új fájl-gazdaság, ha kell).
```

## ROUND7–8 – teszter-leletek + második átvizsgálás
```
Javítás: <KÓD>: <TEST-ID-k>.
Második átvizsgálás (csak olvasás, max 10 lelet + 3–5 ötlet a felhasználónak): <KÓD>: <eddig nem nézett utak>.
TESZTER: az előző kör ellenőrzései + teljes regresszió + célzott próbák az új viselkedésre.
```

## ROUND9 – második kereszt-review
```
Csak a 6–8. körös változásokra (a szerzők r6–r8 jelentéseiben megnevezett függvények), csak olvasás.
A végén 1–2 sor: „maradék kockázat a saját területemen” (a felhasználói összefoglalóhoz).
```

## ROUND10 – zárás
```
Záró javítások (<REV-ID-k>) → koordinátor: teljes sor, verzió, commit → TESZTER: végső regresszió + füstpróba minden fő nézeten, admin + néző + link szereppel, 375/1280 px, dupla katt, a 9–10. kör új viselkedése; a végén devserver le, böngészőméret vissza.
```
