# Guida LLM — backtrader

Scopo: repository di backtrader (backtesting di strategie su dati storici), mantenuto in modo orientato al LLM.

## Struttura reale
- `backtrader/`: nucleo — cerebro.py (motore), strategy.py, analyzers/, feeds/, indicators/, brokers/, commissions/, dataseries.py.
- `samples/`: esempi di strategie; `tests/`: test; `docs/`: documentazione di lavoro (non parte del pacchetto).
- `setup.py`, `README.rst`, `README.md` (italiano), `changelog.txt`, `pypi.sh`.

## Installazione / requisiti
- README.md (italiano) creato per questo audit; installazione pip via `pip install backtrader`.
- Python 3.2+ (classifiers setup.py); installazione via `pip install backtrader` (README:134-136).
- Nessun requisito di system library verificato oltre a Python.

## Entry point
- `backtrader.Cerebro` (`cerebro.py`) — pipeline principale: `cerebro.addstrategy(...)`, `cerebro.adddata(...)`, `cerebro.run()`.
- Strategie dai README: `bt.SignalStrategy` con `signal_add` (strategy.py:1524).

## Data feed
- Feed inclusi in `backtrader/feeds/`: YahooFinanceData (yahoo.py), Pandas data (pandafeed.py), CSV e altri (btcsv, csvgeneric, oanda, quandl, ibdata, ...).

## Strategie
- `bt.Strategy` e `bt.SignalStrategy` (strategy.py). Esempio SmaCross con `bt.ind.SMA` + `bt.ind.CrossOver`.

## Analyzers
- 15 analyzers in `backtrader/analyzers/` (annualreturn, calmar, drawdown, returns, sharpe, tradeanalyzer, pyfolio, ...).

## Indicatori
- 124 classi in `backtrader/indicators/*.py`, 123 escludendo la base `Indicator` (criterio AST verificato).

## Comandi
- `pip install backtrader`: non verificato (non eseguito).
- `python -c "import backtrader; print(backtrader.__version__)"`: non verificato.
- Esecuzione sample: non verificato.

## Punti fragili
- Feed Yahoo dipendono da servizi esterni/online; versione 1.9.76.123 (changelog.txt:1, backtrader/version.py).
- pyfolio integration disdeprecata/legata a pyfolio esterno.
- Documentazione datata (2021-07-18) per indici e changelog.

## Documenti datati
- Nessun archivio ufficiale dentro `docs/`; `docs/_inventario.tsv` e `_revisione.tsv` sono appunti di revisione (2026-11).

## Istruzioni d'uso
Vedi `docs/ISTRUZIONI.md` per istruzioni complete di uso di ogni software del repo.

## Scheda tecnica
Vedi `docs/SCHEDA-TECNICA.md` per la scheda tecnica sintetica del repo.
