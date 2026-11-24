# CLAUDE.md — backtrader

Scopo: motore di backtesting di strategie su dati finanziari storici (Python 3.2+), v1.9.76.123.

## Struttura
- `backtrader/`: nucleo (cerebro.py, strategy.py, analyzers/ 15 file, feeds/, indicators/ 124 classi, brokers/, commissions/, dataseries.py).
- `samples/`: esempi (es. samples/sigsmacross/).
- `tests/`: test unitari.
- `docs/`: documentazione di lavoro (non parte del pacchetto).
- `setup.py`, `README.md` (italiano), `README.rst`, `changelog.txt`.

## Comandi
- Install: `pip install .` (pip install backtrader non verificato).
- Verifica: `python3 -c "import backtrader; print(backtrader.__version__)"` → 1.9.76.123 (eseguito, OK).
- Test: `python -m unittest discover -s tests` (non eseguito in questa revisione).
- Esecuzione sample: non verificato.

## Convenzioni
- Entry point: `backtrader.Cerebro`; strategie con `bt.Strategy`/`bt.SignalStrategy` (signal_add).
- Indicatori: `bt.ind.*` (es. SMA, CrossOver); 124 classi, 123 escludendo la base Indicator.
- Feed: `bt.feeds.*` — YahooFinanceData (online, dipendente da servizi esterni), Pandas, CSV.

## Cosa non toccare
- Nessun file di codice in questa revisione; solo docs/.
- Segreti non presenti; niente valori segreti nei file.

## Rimanendo fuori perimetro
- Non modificare README.md oltre alle correzioni necessarie; non aggiungere dipendenze; non toccare codice.

## Rimando
- Guida LLM completa: docs/LLM-GUIDA.md
