---
repo: backtrader
linguaggi: Python (3.2+)
stato: attivo (ultima release 1.9.76.123, 2021)
aggiornato: 2026-11-26
---

# Scheda tecnica — backtrader

## Scopo
Motore di backtesting di strategie su dati finanziari storici. Pipeline: feed dati → strategia con indicatori → broker simulato → analyzer di risultato. Versione 1.9.76.123 (`backtrader/version.py`, verificato con `python3 -c "import backtrader; print(backtrader.__version__)"` → 1.9.76.123, OK).

## Architettura
Libreria Python a pacchetto unico `backtrader/`. Il motore `Cerebro` (cerebro.py) orchestra: dati (feed.py, feeds/), strategia (strategy.py, signal.py), indicatori (indicator.py, indicators/), ordini e broker (order.py, broker.py, brokers/, commissions/, fillers.py), analisi (analyzer.py, analyzers/, observers/), serie temporali (dataseries.py, linebuffer.py, lineseries.py). CLI runner in btrun/ (`tools/bt-run.py`). Meta-programmazione via metabase.py.

## Componenti
| Nome | Ruolo | File |
| --- | --- | --- |
| Cerebro | motore/pipeline principale | backtrader/cerebro.py |
| Strategy / SignalStrategy | API strategie utente | backtrader/strategy.py |
| Indicator (base) | base indicatori (124 classi, 123 concrete) | backtrader/indicator.py, indicators/ |
| Feed (base) | base feed dati | backtrader/feed.py, feeds/ |
| Analyzer (base) | metriche di risultato (15 analyzer) | backtrader/analyzer.py, analyzers/ |
| Broker | esecuzione simulata ordini | backtrader/broker.py, brokers/ |
| CommissionInfo | schemi commissioni | backtrader/comminfo.py, commissions/ |
| DataSeries / LineBuffer | serie temporali lazy | backtrader/dataseries.py, linebuffer.py |
| Order | ciclo di vita ordini | backtrader/order.py |
| Sizer | dimensionamento posizioni | backtrader/sizer.py, sizers/ |
| Store | connettori broker live | backtrader/store.py, stores/ |
| btrun | runner CLI | backtrader/btrun/, tools/bt-run.py |

## Dati e interfacce
- Ingresso: serie storiche OHLCV da CSV (btcsv.py, csvgeneric.py), Pandas DataFrame (pandafeed.py), Yahoo Finance online (yahoo.py), Oanda/IB/Quandl (feeds/ oanda.py, ibdata.py, quandl.py).
- Interfacce utente: classi `bt.Cerebro`, `bt.Strategy`, `bt.SignalStrategy` (signal_add), `bt.ind.*`, `bt.feeds.*`, `bt.analyzers.*`, `bt.brokers.*`.
- Uscita: risultati run via analyzer (returns, sharpe, drawdown, tradeanalyzer, pyfolio, ...) e grafici (plot/, matplotlib opzionale).
- Dati di esempio in datas/; strategie di esempio in samples/ (~70 script).

## Dipendenze
- Python 3.2+ (classifiers setup.py: 3.2–3.7).
- Nessuna dipendenza runtime obbligatoria (`install_requires` vuoto in setup.py).
- Opzionale: matplotlib (extra `plotting`, setup.py extras_require).
- Esterne/non incluse: pyfolio (analyzer pyfolio), feed online Yahoo/Oanda/IB.

## Configurazione
- Nessun file di configurazione; parametri via `params` delle classi Strategy/Indicator e argomenti Cerebro.
- Variabili d'ambiente usate dal codice: non verificato.

## Build e test
- Install: `pip install .` (da sorgente) o `pip install backtrader` (PyPI); da sorgente verificato OK in M-016/M-031.
- Smoke: `python3 -c "import backtrader; print(backtrader.__version__)"` → 1.9.76.123, eseguito OK.
- Test: `python3 -m unittest discover -s tests` → eseguito 2026-11-26: `Ran 0 tests — NO TESTS RAN` (gli script in tests/ non usano unittest.TestCase riconoscibile da discover; eseguibili singolarmente, non verificato).
- CI: `.travis.yml` presente (storico, non verificato).

## Deploy
- Pacchetto PyPI `backtrader` (setup.py, pypi.sh); uso come libreria importata. Nessun servizio/demone.

## Rischi e debito tecnico
- Progetto non più mantenuto attivamente (changelog fermo al 2021-07-18); SyntaxWarning su Python recenti (cerebro.py:670,712, escape non validi).
- Feed Yahoo dipendono da servizi online esterni: rotture API possibili.
- Integrazione pyfolio legata a dipendenza esterna deprecata.
- Test suite non auto-rilevabile da `unittest discover` (vedi Build e test).
- Documentazione ufficiale datata; docs/ di lavoro è revisione 2026-11 (LLM-GUIDA.md, ISTRUZIONI.md).
