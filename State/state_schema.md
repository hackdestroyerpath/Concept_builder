# Схема state

[← Назад к README](../README.md)

Связанные файлы:
- [Service state](service_state.json)
- [Execution index state](execution_index_state.json)
- [Обновление state](../Protocols/common/state_update.md)
- [Загрузка контекста](../Protocols/common/context_loading.md)

## Назначение

Файл задаёт минимальную схему state-файлов `Concept Builder`. State нужен для восстановления работы в новом чате без чтения всего репозитория.

## Обязательные поля верхнего state

```json
{
  "state_id": "...",
  "mode": "service|execution",
  "current_phase": "...",
  "current_focus": "...",
  "focus_stack": [],
  "active_protocols": [],
  "allowed_context": [],
  "blocked_context": [],
  "pending_user_action": "...",
  "next_expected_step": "...",
  "last_persisted_at": "...",
  "state_revision": 0,
  "state_hash": "sha256:...",
  "context_summary": "...",
  "status": "active|waiting_user|blocked|closed"
}
```

## Обязательные поля issue state

```json
{
  "issue_id": "...",
  "type": "simple|complex|unknown",
  "parent_id": null,
  "child_ids": [],
  "qa_status": "not_evaluated|required|skipped|answered",
  "requirements_status": "missing|draft|approved",
  "plan_status": "missing|draft|approved",
  "solution_status": "missing|draft|approved",
  "contract_status": "missing|draft|approved",
  "closure_status": "open|closed",
  "relationships": {
    "blocks": [],
    "depends_on": [],
    "uses_output_of": [],
    "related_to": []
  }
}
```

## Правило `state_hash`

`state_hash` считается как SHA-256 от канонического JSON с пустым значением `state_hash`. Перед записью агент увеличивает `state_revision`, обновляет `last_persisted_at`, пересчитывает hash и сохраняет файл в GitHub до ответа пользователю.

## Когда state читать обязательно

- при запуске нового чата;
- перед записью в GitHub;
- после утверждения, отклонения, обсуждения, добавления или смены фокуса;
- перед выполнением solution или export;
- при низкой уверенности в контексте.
