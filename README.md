# Concept Builder

Русскоязычный GitHub-backed starter repository для двух режимов: **Service Mode** поддерживает сам Concept Builder, **Exec Mode** создаёт, редактирует, утверждает и экспортирует сети концептуальных Markdown-страниц.

GitHub является durable source of truth для протоколов, состояния, реестров, issues, concepts и exports. Chat response — только проверяемое зеркало, потому что память в чате, как обычно, притворяется инфраструктурой.

## Startup
1. Установить инструкции из `instructions/service_project_instruction.md` и `instructions/exec_project_instruction.md` в соответствующие ChatGPT Projects.
2. В новом чате отправить `пинг`, `старт`, `start`, `ping` или `1`.
3. Агент загружает `state/system_state.json`, mode-state, этот README и `protocols/README.md`; затем подгружает только нужные протоколы.
4. Любой durable ответ требует GitHub write + readback verification.

## Map
- `instructions/` — канонические Project instruction sources.
- `protocols/` — core/service/exec protocols.
- `schemas/` — JSON Schema и JSONL schema notes.
- `state/` — system/service/exec state и locks.
- `issues/`, `inbox/`, `source_ideas/` — Service Mode workflow storage.
- `concepts/`, `exports/` — Exec Mode concept networks and exports.
- `validation/` — validation and dry-run evidence.

## Invariants
- Один durable data type имеет одного canonical owner.
- Mirrors are labelled: `verbatim`, `summary`, `pointer`, `snapshot`, `cache`.
- No GitHub save claim without readback.
- No broad context load without focus need.
- No destructive cleanup without tombstone/archive/dependency gate.

Implementation handoff: `implementation_report.md`.
