---
repo: backtrader
software:
  - backtrader (libreria Python)
  - samples/ (script di esempio)
  - tools/bt-run.py
  - tools/yahoodownload.py
  - tools/rewrite-data.py
  - tests/ (suite di test)
aggiornato: 2026-11-25
---

# Istruzioni d'uso — backtrader

Motore di backtesting di strategie su dati finanziari storici (Python), v1.9.76.123.

## backtrader (libreria)
### Requisiti
- Python 3 (classifiers setup.py: 3.2–3.7+). Nessuna system library verificata.
### Installazione
```bash
pip install backtrader
# oppure da sorgente:
pip install .
```
### Avvio / utilizzo
Entry point `backtrader.Cerebro`: crea strategie con `bt.Strategy` o `bt.SignalStrategy`, aggiungi feed, `cerebro.run()`.
Esempio minimo: crossover di due SMA (vedi README.md per il codice completo).
### Verifica
`python3 -c "import backtrader; print(backtrader.__version__)"` → `1.9.76.123` (eseguito, OK).
### Problemi noti
YahooFinanceData dipende da servizi online esterni; i feed storici possono variare. pyfolio esterno deprecato/legato a dipendenza esterna.

## samples/
### Requisiti
- backtrader installato (vedi sopra).
### Installazione
Nessuna installazione: script eseguibili nel repo.
### Avvio
```bash
python samples/sigsmacross/sigsmacross.py
```
(circa 70 esempi in `samples/`; molti usano feed Yahoo online.)
### Uso
Ciascun sample è uno script standalone che illustra un pattern (indicatori, order, analyzer, ecc.).
### Verifica
Non eseguito in questa revisione (alcuni sample richiedono rete).
### Problemi noti
Alcuni sample (es. Yahoo-based) richiedono rete; non sempre eseguibili offline.

## tools/bt-run.py
### Requisiti
- backtrader installato.
### Installazione
Nessuna.
### Avvio
```bash
python tools/bt-run.py
```
Invoca `backtrader.btrun.btrun()` (runner da riga di comando).
### Uso
Runner CLI che legge script di strategie e li esegue.
### Verifica
Non eseguito in questa revisione.
### Problemi noti
Non documentato a parte il sorgente; uso limitato.

## tools/yahoodownload.py
### Requisiti
- Python 3; rete (download da servizi esterni).
### Installazione
Nessuna.
### Avvio
```bash
python tools/yahoodownload.py --help
```
(uso da riga di comando; vedere argparse nel sorgente).
### Uso
Scarica dati storici Yahoo Finance per feed locali.
### Verifica
Non eseguito (richiede rete e parametri).
### Problemi noti
Dependente da servizi online; esterni potrebbero non essere raggiungibili.

## tools/rewrite-data.py
### Requisiti
- Python 3; backtrader installato.
### Installazione
Nessuna.
### Avvio
```bash
python tools/rewrite-data.py --help
```
### Uso
Converte/riscrive file di dati in formati gestiti dai feed (BTCSV, VChart, ecc.).
### Verifica
Non eseguito (richiede file di input).
### Problemi noti
Non documentato oltre il sorgente.

## tests/
### Requisiti
- backtrader installato; dipendenze di test non verificate.
### Installazione
Nessuna.
### Avvio
```bash
python -m unittest discover -s tests
```
### Uso
Suite di test unitari; ~85 file di test.
### Verifica
Non eseguito in questa revisione (vedi CLAUDE.md).
### Problemi noti
Alcuni test potrebbero fallire per dipendenze esterne non presenti.

## Segreti
Nessun segreto nel repo. Nessuna variabile necessaria.
