# BMAD Starter

Przenośny snapshot oficjalnej instalacji [BMAD Method](https://github.com/bmad-code-org/BMAD-METHOD). Repo nie zawiera `node_modules`, lockfile'a ani zależności npm — można je sklonować w środowisku bez dostępu do npm, a następnie przenieść wymagane katalogi do repo aplikacji.

## Snapshot

Wygenerowano oficjalnym instalatorem `bmad-method@6.12.0` dnia 2026-09-18.

| Element | Wersja |
| --- | --- |
| BMad Core | 6.12.0 |
| BMad Method (BMM) | 6.12.0 |
| Test Architect (TEA) | v1.27.1 |
| BMad Loop | v0.11.1 |

Zainstalowane integracje: Claude Code, Codex, Cursor i GitHub Copilot. Snapshot zachowuje 21 shimów zgodności v6, więc starsze wywołania skilli nadal przekierowują do ich następców.

## Co przenieść do projektu

Skopiuj odpowiednie katalogi do katalogu głównego projektu:

| Narzędzie | Katalogi |
| --- | --- |
| Każdy projekt z BMAD | `_bmad/` |
| Claude Code | `.claude/skills/` |
| Codex, Cursor lub GitHub Copilot | `.agents/skills/` |
| Definicje agentów GitHub Copilot | `.github/agents/` |

`_bmad-output/` jest miejscem na artefakty projektu (plany, specyfikacje i wyniki testów); nie trzeba go kopiować ze startera.

Po skopiowaniu zmień przede wszystkim `project_name` w `_bmad/config.toml` oraz dane użytkownika w `_bmad/config.user.toml`. Trwałe wspólne nadpisania umieszczaj w `_bmad/custom/config.toml`; prywatne w `_bmad/custom/config.user.toml`.

## Moduły

- **BMM** — standardowy przepływ planowania i budowania.
- **TEA** — strategia testów, ATDD, automatyzacja, traceability i bramki NFR.
- **BMad Loop** — następca wycofanego Story Automatora. Jego skille są w snapshotcie, ale automatyczne pętle wymagają osobnej konfiguracji narzędzia przez skill `bmad-loop-setup` (oraz `uv`) w konkretnym projekcie.

Nie dołączono wyspecjalizowanych modułów BMad Builder, Creative Intelligence Suite ani Game Dev Studio — można je dobrać później do konkretnego typu projektu.

## Odświeżenie snapshotu

W środowisku mającym Node.js i dostęp do npm uruchom w czystej kopii repo:

```bash
npx --yes bmad-method@latest install \
  --directory . \
  --modules bmm,tea,bmad-loop \
  --tools claude-code,codex,cursor,github-copilot \
  --all-stable --shims --yes
```

Pełne, przypięte metadane instalacji (wersje, commity źródeł i kanały) są w `_bmad/_config/manifest.yaml`.
