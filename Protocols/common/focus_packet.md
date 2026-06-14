# Пакет фокуса

[Назад к README](../../README.md)

## Назначение

Единственный основной источник для пакета фокуса. Пакет фокуса фиксирует минимальный набор контекста, который агент собирает при запуске, продолжении или восстановлении потерянного фокуса.

## Связанные файлы

- [Протокол запуска](startup.md)
- [Загрузка контекста](context_loading.md)
- [Схема состояния](../../State/state_schema.md)
- [Обновление состояния](state_update.md)

## Схема

```yaml
context_bundle_id: string
mode: service|execution|concept|issue
state_file: path
state_revision_loaded: integer|null
state_hash_loaded: string|null
current_focus: string|null
current_entity_id: string|null
parent_anchor: string|null
focus_stack: []
active_state_files: []
active_protocols: []
allowed_context: []
blocked_context: []
source_files_loaded: []
output_files_expected: []
pending_user_action: string|null
next_expected_step: string
reload_reason: startup|resume|focus_loss|state_conflict|user_request|link_repair|export_precheck
context_confidence: high|medium|low
health_signal:
  state_loaded: true|false
  protocols_loaded: true|false
  pending_action_checked: true|false
  persistence_required_before_response: true|false
```

## Жёсткое условие восстановления

Если любое из полей `state_file`, `state_revision_loaded`, `current_entity_id` при активной сущности, `active_state_files` или `active_protocols` не восстановлено, агент не продолжает содержательную работу. Он должен:

1. выставить `context_confidence=low`;
2. открыть README, индекс файлов, карту связей, нужное состояние и протоколы запуска и контекста;
3. восстановить недостающие поля;
4. сохранить нужное состояние, если восстановление его меняет;
5. только после этого продолжать задачу.

## Правило авторитета

Другие файлы могут кратко перечислять пакет фокуса, но не имеют права вводить альтернативную схему. При конфликте побеждает этот файл.
