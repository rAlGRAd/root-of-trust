# ROOT OF TRUST — contesto di progetto

Gioco di carte **didattico** su **Active Directory (Windows Server / AD DS)**. Serve a studiare AD trasformandolo in un gioco di carte asimmetrico ispirato a *Netrunner* (struttura Corp/Runner) e *Slay the Spire 2* (impianto delle carte). Materia di riferimento: modulo **MOD 3.3 (AD DS — BERNABEI)**.

## Direttiva primaria: VERIDICITÀ
Il fine ultimo dell'endgame è secondario. La priorità assoluta è che **ogni carta e ogni voce di legenda corrisponda al comportamento reale di Active Directory**. Se una regola di gioco non rispecchia AD, è un bug da correggere — non una licenza creativa. Le spiegazioni reali vanno ancorate alle fonti del modulo; le tecniche d'attacco (classe THREAT) sono reali ma esterne al materiale d'esame (contraltare offensivo).

## Due livelli, sempre separati
- **Carta** = il *processo di gioco* (cosa fa l'elemento al tavolo). Solo relazioni + parole-chiave, **nessuna definizione**.
- **Specchietto/Legenda** = *Active Directory reale* (cosa è davvero: ruolo, funzionamento, comandi). È il livello educativo.

## Architettura (statica, nessun build step)
- `index.html` — galleria carte + glossario keyword + legenda. Contiene **solo** stili e logica (render, tooltip hover, legenda linkabile, filtri per classe).
- `data.js` — **unica fonte di verità** dei contenuti: `A`, `KW`, `CARDS`, `INFO`. Caricato via `<script src="data.js">` (niente `fetch`, così il file si apre offline con doppio click).
- `compendio.html` — regolamento/compendio v0.1 in stile rulebook (motore di gioco, zone, turno, win condition, set iniziale, specchietti-acronimo nel margine).

Aprire `index.html` o `compendio.html` direttamente nel browser. **Non** introdurre bundler, framework o `localStorage`/`fetch`: deve restare apribile da filesystem.

## Modello dati (`data.js`)

### `KW` — glossario parole-chiave (meccaniche)
Array di `[NOME, fazione, descrizioneProcesso]` con fazione `"d"|"a"|"n"` (def/atk/neutro). Descrive **solo il processo di gioco**, mai il significato AD.

### `CARDS` — le carte
```js
{ n, fac, cost, ty, cls, rel, kw }
```
- `n` — nome dell'elemento AD (in **inglese**, com'è nella realtà). È anche la chiave verso `INFO`.
- `fac` — `"def"` (Domain Admin/blu) · `"atk"` (Threat Actor/rame) · `"neu"` (motore neutro/bruno).
- `cost` — numero = Azioni per giocarla; `"S"` = elemento **di sistema** (regola ambientale, non si gioca).
- `ty` — tipo: `Oggetto | Operazione | Tecnica | Power | Ticket | Stato | Ruolo | Partizione | Protocollo`.
- `cls` — classe: `PROTOCOL | DIRECTORY | IDENTITY | ACCESS | POLICY | TOPOLOGY | THREAT` (guida i filtri e l'ordine della legenda).
- `rel` — interazioni con altri elementi. Sintassi: `[NomeElemento]` viene reso come chip-elemento; `A` (costante = freccia `→`) separa i passaggi. **Solo processi**, niente definizioni.
- `kw` — array di keyword; **ognuna deve esistere in `KW`**.

### `INFO` — legenda reale (lo specchietto)
Mappa `{ "NomeCarta": { r, c?, g } }`:
- `r` — **Ruolo & funzionamento** in AD reale (sintesi comprensibile, dalle fonti). Mostrato sia in hover sia in legenda.
- `c` — (opzionale) **Comandi/strumenti** reali (cmdlet PowerShell, snap-in, eseguibili).
- `g` — **In gioco**: il ponte verso il processo della carta (richiama le keyword in MAIUSCOLO, evidenziate automaticamente).

**Invariante:** ogni `CARDS[i].n` deve avere una voce `INFO[n]`. Le voci THREAT iniziano con `[Tecnica d'attacco]` e includono la difesa reale.

## Come aggiungere una carta
1. In `data.js` → `CARDS`: aggiungi l'oggetto. Usa `[Elemento]` in `rel` e keyword già presenti in `KW`.
2. In `data.js` → `INFO`: aggiungi la voce `{ r, c?, g }` con la spiegazione AD reale (fonti) + il ponte di gioco.
3. Se introduci una nuova meccanica, aggiungi prima la keyword in `KW` (descrivendo **solo il processo**).
4. Nessun'altra modifica: `index.html` rende tutto automaticamente (filtri, slug `#leg-…`, tooltip, legenda).

## Convenzioni
- Nomi elementi AD in inglese; prosa in **italiano**.
- Acronimi sempre esplicitati nella legenda (`r`), perché il modulo è organizzato attorno agli acronimi.
- Niente over-formatting; densità tecnica in stile rulebook Null Signal.
- Palette: `--def #27597C` · `--atk #A8472A` · `--neu #6B6256`. Font: Space Grotesk (display), Spectral (corpo), IBM Plex Mono (record/keyword/comandi).

## Backlog (prossime iterazioni)
- **Bilanciamento**: costi in Azioni, soglia `Trace`/rilevamento, livelli di `Noise` (ora segnaposto).
- **Keyword negative**: introdurre `NO-LINK` / `NO-SID` per rendere espliciti i "non può" (es. `Container`, `Distribution Group`).
- **FSMO**: valutare se accorpare i 5 ruoli in una sola carta multi-modalità.
- **THREAT**: aggiungere una riga `requisito d'ingresso` (es. "richiede 1 Recon", "richiede Session") leggibile senza regolamento.
- **Multi-dominio & Trust**: archi inter-forest, transitività, SID filtering (espansione "foresta a più alberi").
- **Deck scenario / Disaster Recovery**: guasti, restore authoritative/non-authoritative, Recycle Bin, FSMO seize.
- **Modalità solitario**: Threat Actor scriptato per studio individuale.
- Decisione aperta: target **tavolo fisico** (carte stampabili) vs **digitale** (Access Check/token automatizzati).
