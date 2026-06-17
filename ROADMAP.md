# ROADMAP — continuazione autonoma di ROOT OF TRUST

Questo file è lo **stato** del lavoro autonomo. Un task programmato riprende il progetto
appena i token sono di nuovo disponibili e avanza di **una voce per esecuzione**.

## Protocollo autonomo (regole per ogni esecuzione)
1. Leggi questo file. Prendi **la prima voce non spuntata** (`- [ ]`) dall'alto. Se non ce ne sono, **non fare nulla** (no-op) e termina.
2. Implementa **solo quella voce**.
3. **Veridicità prima di tutto**: ogni carta e ogni voce di legenda deve essere fedele ad Active Directory reale (vedi `CLAUDE.md`). Se una regola non rispecchia AD, è un bug.
4. **Fonte di verità = `data.js`**. `index.html`, `index-immagini.html`, `compendio.html`, `diagramma.html` rendono di conseguenza. Se aggiungi termini, aggiorna anche `INFO`/`KW_INFO` e i riferimenti `TERM_REF`/`KW_REF` verso il compendio.
5. **Verifica** con il preview: carica le pagine toccate, 0 errori console, link/àncore che risolvono. Se hai modificato `index.html`, rigenera il bundle: `node build-standalone.js` nella cartella del progetto.
6. **Commit atomico + push** su `origin main` (gh CLI già autenticato; repo `rAlGRAd/root-of-trust`, Pages: https://ralgrad.github.io/root-of-trust/). Chiudi il messaggio con `Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>`. **Mai** `--force`, **mai** operazioni distruttive, **mai** riscrivere la storia.
7. **Spunta la voce** (`- [x]`) aggiungendo una nota breve (cosa fatto + hash commit). Poi **fermati** (una voce per esecuzione).
8. Se sei senza budget/rate-limited: termina senza modifiche, riproverai alla prossima esecuzione.

## Lavori (in ordine di priorità)
- [ ] **Accessibilità & meta** — aggiungi `<meta name="description">` a tutte le pagine, `alt` descrittivi alle immagini di `index-immagini.html` (es. "tema network/server/…"), stile `:focus-visible` per nav e carte. Verifica nessuna regressione.
- [ ] **Print CSS** — foglio `@media print` in `index.html`: nasconde nav/filtri/ricerca, stampa le carte in griglia pulita (colori leggibili, niente ombre pesanti). Una variante stampabile della galleria.
- [ ] **Keyword negative NO-LINK / NO-SID** — aggiungi in `KW` e `KW_INFO` (significato AD reale), applicale a `Container` (NO-LINK) e `Distribution Group` (NO-SID) in `CARDS`, con `KW_REF` verso compendio §3.4 e §4.2.
- [ ] **Requisito d'ingresso THREAT** — aggiungi un campo `req` alle carte di classe THREAT in `data.js` (es. "richiede Recon", "richiede Session") e rendilo nella carta e nella legenda. Mantieni la fedeltà (prerequisito reale della tecnica).
- [ ] **Espansione Trust** — nuove carte DIRECTORY: `Forest Trust`, `External Trust`, `Shortcut Trust` con voce `INFO` (transitività, SID filtering) e `TERM_REF` verso compendio §11.
- [ ] **Carta gMSA** (classe IDENTITY) — `Group Managed Service Account` con legenda (password 240-bit gestita da AD, difesa al Kerberoasting) e `TERM_REF` §12.2/§4.
- [ ] **Revisione veridicità** — rileggi 5 voci `INFO` e correggi eventuali imprecisioni; annota quali nella nota di completamento.

## Log
<!-- la procedura autonoma aggiunge qui una riga per ogni voce completata -->
