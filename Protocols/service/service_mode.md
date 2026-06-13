# Service Mode protocol

[Назад к README](../../README.md)

## Назначение

Primary protocol для обслуживания самого `Concept Builder`: protocols, state schema, project instructions, repository maps, service issue и validation. Этот режим не ведёт пользовательские концепции как основную работу.

## Связанные файлы

- [Startup protocol](../common/startup.md)
- [Input registry](input_registry.md)
- [Issue lifecycle](../issue/issue_lifecycle.md)
- [State update](../common/state_update.md)
- [Service state](../../State/service_state.json)

## Разрешено

- менять `README.md`, `Repository/file_index.jsonl`, `Repository/link_graph.md`, `Checks/final.md`;
- менять `Instructions/`, `State/`, `Protocols/`, `Templates/`;
- вести service-level issue и registry;
- выполнять link/orphan/language validation;
- исправлять system-file defects после mutation gate.

## Запрещено

- создавать demo concepts без реального пользовательского запроса;
- загружать handoff/audit/checkpoint/task-state archives в production repo;
- менять concept files как основную задачу, кроме documented cross-mode repair;
- объявлять persistence или pass без evidence.

## System-file mutation gate

Изменение system files разрешено только если выполнено одно из условий:

```yaml
approved_service_issue_exists: true
requirements_approved: true
solution_or_contract_approved: true
affected_files_listed: true
persistence_plan_known: true
```

Narrow exception допустим для emergency repair README/state/hash/link, но exception должен быть записан в state/output evidence и закрыт последующей validation.

## Workflow

1. Запустить [startup.md](../common/startup.md).
2. Зарезервировать input по [input_registry.md](input_registry.md), если запрос не compact или должен продолжаться.
3. Создать или обновить service issue через [issue_lifecycle.md](../issue/issue_lifecycle.md).
4. Проверить mutation gate.
5. Изменить production files.
6. Обновить registry, repository index/map и relevant state.
7. Выполнить link/orphan/language checks.
8. Ответить пользователю только после GitHub persistence.

## Closure gate

```yaml
production_files_written: true
file_index_updated: true
link_graph_updated: true
state_updated: true
registry_consistent: true
broken_active_links: []
orphan_active_files: []
dev_only_files_in_production: []
user_response_ready: true
```
