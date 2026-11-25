# BUG-G4 — candidati bug (M-031.8b.1)

CANDIDATO-1: backtrader/analyzers/drawdown.py:108-109 — divisione per zero su _maxvalue
  `self.rets.drawdown = drawdown = 100.0 * moneydown / self._maxvalue`.
  Se il broker ha valore 0 (oppure nessun notify_fund con valore positivo), _maxvalue resta 0.0
  e moneydown = 0.0 -> ZeroDivisionError in `next()`, crash dell'analisi e del run.

CANDIDATO-2: backtrader/analyzers/returns.py:140 — divisione per zero su _tcount
  `self.rets['ravg'] = ravg = rtot / self._tcount`.
  In `stop()`, se nessun sottoperiodo si è completato (_tcount == 0, es. run a tick singolo o
  timeframe non attraversato), rtot / 0 -> ZeroDivisionError, crash nell'analisi Returns.

CANDIDATO-3: backtrader/strategy.py:577 e strategy.py:1619 — default mutabili `qorders=[], qtrades=[]`
  `def _notify(self, qorders=[], qtrades=[])`. Non mutati attualmente, ma sono default condivisi;
  qualsiasi chiamata con argomenti posizionali o futura mutazione li lascerebbe "contaminati"
  tra chiamate. Rischio di corruzione dello stato (logica errata fragile).

Ricerche fatte: leggi sorgenti principali (strategy, broker, feed, cerebro, analytics, functions),
grep TODO/FIXME/XXX, grep mutable default args e divisioni.
