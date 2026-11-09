# CLAUDE.md

Projektregeln stehen in `AGENTS.md` (wird automatisch geladen). Diese Datei
ergänzt nur, wie Claude Code mit seinen beiden Helfern arbeitet.

## Rechte und linke Hand

Zwei feste Helfer-Agents liegen in `.claude/agents/`:

| Name | Agent | Rolle | Darf ändern? |
|------|-------|-------|--------------|
| **Finn** | `finn-rechte-hand` | Umsetzung einfacher, klar umrissener Aufgaben | ja (kein Commit/Push) |
| **Ida** | `ida-linke-hand` | Recherche, Analyse, Tests/Checks, Diff-Review | nein (nur lesen + Checks) |

### Wann delegieren (automatisch, ohne Rückfrage)

- **An Finn:** kleine Fixes mit bekannter Ursache, Renames, Boilerplate,
  Tests ergänzen, bestehendes Muster übertragen, Doku/Config, Lint- und
  Typecheck-Fehler, mechanische Änderungen über mehrere Dateien.
- **An Ida:** „Wo ist X implementiert?", Aufrufpfade verfolgen, Usages vor
  einem Refactor sammeln, Module/Doku zusammenfassen, Tests/Typecheck laufen
  lassen und Fehler berichten, fertigen Diff gegenlesen, geltende Regeln
  heraussuchen.
- **Selbst behalten:** Architektur- und Designentscheidungen, unklare
  Anforderungen, schwer zu findende Bugs, Schema/Migrationen, Auth und
  Permissions, Approval-/Budget-Logik, Telemetry, Sicherheitsrelevantes und
  alles, was Finn oder Ida zurückgeben.

### Zusammenspiel

- Unabhängige Aufgaben parallel an Finn und Ida geben und währenddessen selbst
  am schwierigen Teil arbeiten.
- Typischer Ablauf: Ida klärt den Kontext → Haupt-Agent entscheidet → Finn setzt
  die einfachen Teile um → Ida prüft das Ergebnis.
- Finn und Ida können selbst keine Sub-Agents starten. Sie liefern bei Bedarf
  einen **Delegationsplan** (welcher Agent, welcher Auftrag, parallel/sequenziell),
  den der Haupt-Agent ausführt. So steuern sie die übrigen Agents
  (`Explore`, `codemod-runner`, `token-auditor`, `general-purpose`, …).
- Ergebnisse der Helfer vor Commit kurz selbst prüfen; Commit und Push macht
  nur der Haupt-Agent.
