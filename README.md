# backtrader

BackTesting Engine: motore di backtesting per strategie su dati finanziari storici (Python).

## Requisiti

- Python 3 (classifiers setup.py: 3.2–3.7+; usato per la maggior parte dei datacenter).
- Nessuna dipendenza di system library verificata; il pacchetto installa via pip.

## Installazione

```bash
pip install backtrader
```

Da sorgente (worktree/clone del repo):

```bash
pip install .
```

Verifica rapida:

```bash
python -c "import backtrader; print(backtrader.__version__)"
```

## Uso

Esempio minimo (crossover di due SMA):

```python
import datetime
import backtrader as bt

class SmaCross(bt.SignalStrategy):
    params = dict(sma1=10, sma2=20)
    def __init__(self):
        sma1 = bt.ind.SMA(period=self.params.sma1)
        sma2 = bt.ind.SMA(period=self.params.sma2)
        crossover = bt.ind.CrossOver(sma1, sma2)
        self.signal_add(bt.SIGNAL_LONG, crossover)

cerebro = bt.Cerebro()
cerebro.broker.set_cash(10000)
data = bt.feeds.YahooFinanceData(dataname='YHOO',
                               fromdate=datetime.datetime(2011,1,1),
                               todate=datetime.datetime(2012,12,31))
cerebro.adddata(data)
cerebro.addstrategy(SmaCross)
cerebro.run()
```

Sample disponibili in `samples/` (es. `samples/sigsmacross/sigsmacross.py`, eseguibili con `python samples/sigsmacross/sigsmacross.py`).

## Struttura delle cartelle

- `backtrader/` — nucleo del pacchetto (`cerebro.py` motore, `strategy.py`, `feeds/`, `indicators/`, `analyzers/`, `brokers/`, ...).
- `samples/` — esempi di strategie.
- `tests/` — test unitari.
- `docs/` — documentazione di lavoro.
- `setup.py`, `README.rst`, `changelog.txt`.

## Test

```bash
python -m unittest discover -s tests
```

Nota: il download dati YahooFinanceData dipende da servizi online; i feed storici possono variare.

Istruzioni d'uso: docs/ISTRUZIONI.md

Scheda tecnica: docs/SCHEDA-TECNICA.md
