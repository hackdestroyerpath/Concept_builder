# Focus packet

[Назад к README](../../README.md)

## Назначение

Единственный primary source для focus packet. Focus packet фиксирует минимальный context bundle, который агент должен собрать при startup, resume или focus-loss recovery.

## Связанные файлы

- [Startup protocol](startup.md)
- [Context loading](context_loading.md)
- [State schema](../../State/state_schema.md)
- [State update](state_update.md)

## Schema

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

## Hard recovery condition

Если любое из полей `state_file`, `state_revision_loaded`, `current_entity_id` при active entity, `active_state_files` или `active_protocols` не восстановлено, agent не продолжает содержательную работу. Он должен:

1. выставить `context_confidence=low`;
2. открыть README, file index, link graph, relevant state и startup/context protocol;
3. восстановить missing fields;
4. сохранить relevant state, если recovery меняет state;
5. только после этого продолжать задачу.

## Authority rule

Другие файлы могут кратко перечислять focus packet, но не имеют права вводить альтернативную схему. При конфликте побеждает этот файл, иначе опять получится бюрократия с несколькими королями на одном табурете.
