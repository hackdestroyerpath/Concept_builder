# Протокол Service Mode

[Назад к README](../../README.md)

## Назначение

Основной протокол обслуживания самого `Concept Builder`: протоколы, схемы состояния, инструкции проекта, карты репозитория, служебные issue и проверки. Этот режим не ведёт пользовательские концепции как основную работу.

## Связанные файлы

- [Протокол запуска](../common/startup.md)
- [Входные материалы и registry](input_registry.md)
- [Жизненный цикл issue](../issue/issue_lifecycle.md)
- [Обновление состояния](../common/state_update.md)
- [Состояние service](../../State/service_state.json)

## Разрешено

- менять `README.md`, `Repository/file_index.jsonl`, `Repository/link_graph.md`, `Checks/final.md`;
- менять `Instructions/`, `State/`, `Protocols/`, `Templates/`;
- вести служебные issue и registry;
- выполнять проверки ссылок, сиротских файлов и языка;
- исправлять дефекты системных файлов после проверки изменения.

## Запрещено

- создавать демонстрационные concepts без реального пользовательского запроса;
- загружать handoff, audit, checkpoint, task-state archives в рабочий репозиторий;
- менять файлы concept как основную задачу, кроме записанного межрежимного ремонта;
- объявлять запись или успешную проверку без evidence.

## Проверка изменения системных файлов

Изменение системных файлов разрешено только если выполнено одно из условий:

```yaml
approved_service_issue_exists: true
requirements_approved: true
solution_or_contract_approved: true
affected_files_listed: true
persistence_plan_known: true
```

Узкое исключение допустимо для аварийного ремонта README, state, hash, ссылок или финальной проверки. Исключение записывается в состояние или доказательства и проверяется после записи. После завершения ремонта `allowed_exception` возвращается в `null`.

## Рабочий процесс

1. Запустить [startup.md](../common/startup.md).
2. Зарезервировать вход по [input_registry.md](input_registry.md), если запрос некомпактный или должен продолжаться.
3. Создать или обновить служебную issue через [issue_lifecycle.md](../issue/issue_lifecycle.md).
4. Проверить gate, то есть условие допуска изменения системных файлов.
5. Изменить рабочие файлы.
6. Обновить registry, индекс, карту репозитория и нужное состояние.
7. Выполнить проверки ссылок, сиротских файлов и языка.
8. Ответить пользователю только после сохранения через GitHub.

## Компактное ремонтное исключение

Для компактной repair-задачи без отдельной папки issue допускается direct patch, если:

```yaml
user_request_is_current_turn: true
affected_files_are_known: true
no_new_product_feature_added: true
state_update_or_final_evidence_records_exception: true
validation_run_after_write: true
```

После закрытия ремонта `allowed_exception` возвращается в `null`; история ремонта хранится в `Checks/final.md`, `context_summary` и внешнем архиве доказательств, а не в активной поблажке.

## Условие закрытия

```yaml
production_files_written: true
file_index_updated_or_unchanged_with_reason: true
link_graph_updated: true
state_updated: true
registry_consistent: true
broken_active_links: []
orphan_active_files: []
dev_only_files_in_production: []
user_response_ready: true
```
