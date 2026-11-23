# claude-hordozhato

Hordozható Claude Code-memória: válaszstílus, kötelező agent-munkaszabályok, állandó kérések, munkafolyamat, sablonok és egy közös tanulságnapló – **személyes, projekt- és hálózati adatok nélkül**. Arra való, hogy egy nem saját környezetben futó gép (pl. munkahelyi) Claude-ja ugyanígy dolgozzon, és időnként lehúzza a friss tapasztalatokat.

| Fájl | Mi ez |
|---|---|
| `CLAUDE.md` | A betöltendő fájl: válaszstílus + kötelező szabályok + frissítési szabály; importálja a `TANULSAGOK.md`-t. |
| `TANULSAGOK.md` | Közös hibanapló: egy sor egy tanulság (jel = hibaüzenet-részlet → teendő). |
| `TANULSAGOK-ARCHIVUM.md` | A szabállyá általánosított vagy összevont, már nem aktív tanulságok (nem töltődik be, csak visszakereséshez). |
| `SZABALYOK.md` | A teljes szabálytár indoklással és ellenőrzési móddal. |
| `KERESEK.md` | Az állandó munkamód-kéréseim, és hogy melyikből melyik szabály lett. |
| `MUNKAFOLYAMAT.md` | Többkörös, több agentes átvizsgálás leírása (szerepek, körök, integráció, csapdák). |
| `sablonok/` | Agent-eligazítás, kör-sablonok, indító prompt. |
| `skills/caveman/` | A tömör válaszmód skillje. |
| `githooks/pre-commit` | Titokszűrő (csak a karbantartónak). |

## Beállítás egy új gépen

> Előbb nézd meg, megengedi-e a munkahelyi MI-/eszközhasználati szabályzat külső konfiguráció betöltését. Ha ütközik, a céges szabály az erősebb (ezt a `CLAUDE.md` is kimondja).

1. Klónozás a home mappába:
   ```bash
   git clone <REPO-URL> ~/claude-hordozhato
   ```
2. A gép globális `~/.claude/CLAUDE.md`-jébe (ha nincs, hozd létre) írd be ezt a sort:
   ```
   @~/claude-hordozhato/CLAUDE.md
   ```
3. Helyi tanulságnapló (ide írja a gép Claude-ja a saját tanulságait; a `~/.claude/rules/` mappa minden munkamenetbe betöltődik):
   ```bash
   mkdir -p ~/.claude/rules && printf '# Helyi tanulságok\n\n| ID | Dátum | Terület | Jel | Mi történt → mit csinálj | Szabály |\n|---|---|---|---|---|---|\n' > ~/.claude/rules/TANULSAGOK-HELYI.md
   ```
4. (Opcionális) caveman skill:
   ```bash
   mkdir -p ~/.claude/skills && cp -r ~/claude-hordozhato/skills/caveman ~/.claude/skills/
   ```
5. Frissítés-ellenőrzés munkamenet-induláskor – a `~/.claude/settings.json`-ba (meglévő `hooks` mellé fésüld be). A hook csak lekéri (`fetch`) és jelzi az újat; lehúzni Claude csak a jóváhagyásod után fog (FRISS-02), így egy esetleg feltört fiókból sem kerülhet automatikusan új utasítás a gépre:
   ```json
   {
     "hooks": {
       "SessionStart": [
         {
           "hooks": [
             { "type": "command", "command": "R=\"$HOME/claude-hordozhato\"; git -C \"$R\" fetch -q 2>/dev/null && [ -n \"$(git -C \"$R\" log --oneline HEAD..origin/main 2>/dev/null)\" ] && echo 'claude-hordozhato: új verzió érhető el (FRISS-02)' || true", "timeout": 20 }
           ]
         }
       ]
     }
   }
   ```
   Hook nélkül is működik: a `CLAUDE.md` FRISS-01 szabálya szerint Claude naponta legfeljebb egyszer maga ellenőrzi.

Ellenőrzés: indíts új munkamenetet, és kérdezd meg: „mi a KOM-01 és az L-005?”.

## Frissítés

- **Fogyasztó gép (pl. munkahelyi):** csak olvas. A hook vagy a FRISS-01 jelzi az újat, Claude megmutatja a változást, és jóváhagyás után lehúzza; a változás a következő munkamenettől él. A gép saját tanulságai a helyi `TANULSAGOK-HELYI.md`-be mennek; ha egy általános és nem érzékeny, Claude jelzi, és a karbantartó viszi át ide.
- **Forrásgép (karbantartó, csak ott):** a TAN-05 szabály szerint minden új, nem érzékeny kérés, szabály és tanulság tisztított változata ide is bekerül: szűrés → commit → push jóváhagyással. A `githooks/pre-commit` a titoknak tűnő értékeket megállítja, és ha a gépen van privát szűrőlista (`~/.claude/hooks/publikus_szuro.py`), azzal is átnézi a commitot. Bekapcsolás: `git config core.hooksPath githooks`.

## Mi NEM kerülhet ide

Név, e-mail, telefonszám, domain, felhasználónevet tartalmazó útvonal, IP-cím, belső hálózati vagy eszközrészlet, projekt-, üzleti vagy pénzügyi adat, harmadik fél adata, jelszó, kulcs, token és munkamenet-azonosító. Kétes esetben kimarad.
