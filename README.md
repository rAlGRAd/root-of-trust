# ROOT OF TRUST

Gioco di carte didattico su **Active Directory** (asimmetrico, ispirato a Netrunner + Slay the Spire 2).
La **carta** dice il processo di gioco; lo **specchietto/legenda** dice cos'è davvero in AD (con comandi reali).

## File
| File | Cosa è |
|---|---|
| `index.html` | Galleria carte + glossario keyword + legenda AD. Ricerca con autocompletamento; finestre overlay che si fissano/chiudono con un click (indipendenti tra loro); rimandi al compendio. |
| `compendio.html` | Compendio dei **processi di Active Directory** in stile rulebook a regole numerate (Cap.Sez.Regola), ordinato per ordine reale di applicazione: identità → autenticazione → autorizzazione → policy → replica → minacce. |
| `diagramma.html` | Diagramma logico di flusso: un'unica catena verticale dall'identità all'accesso; ogni freccia è la procedura che collega i nodi. |
| `root-of-trust-standalone.html` | Versione single-file (galleria + compendio + rimandi interni), comoda per la condivisione. |
| `data.js` | **Unica fonte di verità**: keyword, carte, legenda, riferimenti al compendio. Si modifica qui. |
| `CLAUDE.md` | Contesto di progetto per Claude Code. |

## Aprire
Doppio click su `index.html` (o `compendio.html`). È tutto statico: nessuna installazione, funziona offline da filesystem.

## Lavorarci con Claude Code
```bash
cd root-of-trust
claude
```
Claude Code legge automaticamente `CLAUDE.md`. Esempi di richieste:
- "Aggiungi la carta `gMSA` nella classe IDENTITY con la sua voce di legenda."
- "Introduci la keyword `NO-LINK` e applicala a Container e Distribution Group."
- "Aggiungi la riga 'requisito d'ingresso' alle carte THREAT."

## Regola d'oro
Ogni carta e ogni voce di legenda deve essere **fedele ad Active Directory reale**. La giocabilità viene dopo la veridicità.

## Versione
v0.2 — prototipo iterabile. Costi e livelli di Noise sono segnaposto da bilanciare.
