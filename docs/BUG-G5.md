# BUG-G5 — bug gravi (M-032.8b)

Bug diversi da quelli di docs/BUG-G4.md (drawdown.py:103, returns.py:140, default mutabili strategy.py).

## BUG-1
Dove: backtrader/analyzers/leverage.py:70 — `GrossLeverage.next()`, `(self._value - self._cash) / self._value`.
Gravità: alta — crash dell'analisi (e dell'intero run Cerebro) quando il valore del portafoglio è 0 (conto azzerato / margine esaurito).
Prova: `GrossLeverage.__new__` con `_value=0.0, _cash=100.0` → PRIMA: `ZeroDivisionError: division by zero`.
Correzione: guard `if self._value else 0.0` — leverage 0.0 quando il valore è 0. Commit 4f3c101.
Test: stessa riproduzione DOPO → `next()` completa, `rets = {'dt': 0.0}`, nessuna eccezione.

## BUG-2
Dove: backtrader/analyzers/calmar.py:107 — `Calmar.on_dt_over()`, `math.log(self._values[-1] / self._values[0])`.
Gravità: alta — crash del run se il valore iniziale è 0 (ZeroDivisionError) o se uno dei due valori è negativo (ValueError: log di numero negativo; possibile con short/leva che portano l'equity sotto zero).
Prova: `values=[0.0, 100.0]` → PRIMA: `ZeroDivisionError: division by zero`; `values=[-50.0, 100.0]` → PRIMA: `ValueError: expected a positive input, got -2.0`.
Correzione: guard su `v0 <= 0.0 or self._values[-1] <= 0.0` → `rann = 0.0` (caso degenere, Calmar non definibile). Commit 38c4c67.
Test: DOPO → `[0.0,100.0] → 0.0`, `[-50.0,100.0] → 0.0`, `[100.0,200.0] → 0.3466` (valore corretto), nessuna eccezione.

## BUG-3
Dove: backtrader/mathsupport.py:39 — `average()`, `math.fsum(x) / (len(x) - bessel)`; usato con `bessel=True` da `analyzers/vwr.py:157` (VWR) e `analyzers/sharpe.py:189` (SharpeRatio con stddev_sample).
Gravità: alta — crash del run quando VWR/Sharpe campione ricevono un solo sottoperiodo: `len(x) - bessel == 0` → ZeroDivisionError opaco dentro la libreria.
Prova: `standarddev([0.5], bessel=True)` → PRIMA: `ZeroDivisionError: division by zero`.
Correzione: guard esplicita — `n = len(x) - bessel; if n <= 0: raise ValueError(...)` con messaggio diagnostico; caso normale invariato. Commit afa4b4d.
Test: DOPO → `standarddev([0.5], bessel=True)` solleva `ValueError: average: not enough samples (len=1, bessel=True)`; `standarddev([0.5, 1.5], bessel=True) = 0.7071` (corretto).

## Test di regressione repo
`tests/test_analyzer-sqn.py` non eseguibile in ambiente: `ImportError: Matplotlib seems to be missing` — pre-esistente, riprodotto identico su `origin/master` (dipendenze matplotlib/pandas assenti). Nessun peggioramento attribuibile ai fix.
