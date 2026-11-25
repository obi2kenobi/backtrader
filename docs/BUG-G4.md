# BUG-G4 — candidati bug (M-031.8b.1)

## BUG-1
Dove: backtrader/analyzers/drawdown.py:108-109 — `next()`, `100.0 * moneydown / self._maxvalue`.
Gravità: media — crash dell'analisi (e del run) quando broker valore 0 / _maxvalue non impostato.
Prova: riproduzione diretta su instance `DrawDown.__new__`: `_maxvalue=0.0`, `_value=0.0` -> ZeroDivisionError ("division by zero").
Correzione: guard `if self._maxvalue > 0.0` — drawdown = 0.0 altrimenti. Commit 1e73951.
Test: riproduzione dopo fix — `next()` esce 0.0 senza eccezione. Test di regressione repo non eseguibili in ambiente (matplotlib/pandas mancanti, pre-esistente).

## BUG-2
Dove: backtrader/analyzers/returns.py:140 — `stop()`, `rtot / self._tcount`.
Gravità: media — crash dell'analisi Returns se nessun sottoperiodo si è completato (_tcount == 0).
Prova: divisione `rtot / _tcount` con `_tcount=0` -> ZeroDivisionError.
Correzione: `ravg = rtot / self._tcount if self._tcount > 0 else rtot`. Commit a7ba9c2.
Test: riproduzione dopo fix — ravg = rtot (fallback), nessun ZeroDivisionError. Test di regressione repo non eseguibili in ambiente (matplotlib/pandas mancanti, pre-esistente).

CANDIDATO-3: backtrader/strategy.py:577 e strategy.py:1619 — default mutabili `qorders=[], qtrades=[]`
  `def _notify(self, qorders=[], qtrades=[])`. Non mutati attualmente, ma sono default condivisi;
  qualsiasi chiamata con argomenti posizionali o futura mutazione li lascerebbe "contaminati"
  tra chiamate. Rischio di corruzione dello stato (logica errata fragile).

Ricerche fatte: leggi sorgenti principali (strategy, broker, feed, cerebro, analytics, functions),
grep TODO/FIXME/XXX, grep mutable default args e divisioni.
