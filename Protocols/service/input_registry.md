# Input and registry

[Назад к README](../../README.md)

## Назначение

Primary protocol для `Inbox/`, `input_manifest.json`, service issue registry, reason mirror, limited reserve и cleanup/tombstone policy.

## Связанные файлы

- [Service Mode](service_mode.md)
- [Issue lifecycle](../issue/issue_lifecycle.md)
- [Inbox](../../Inbox/README.md)
- [Service issue registry](../../Issues/registry.jsonl)

## Limited reserve order

Для non-compact input порядок обязателен:

1. создать `Inbox/<input_id>/entry.md` с source summary;
2. создать `input_manifest.json`;
3. сохранить needed attachments;
4. создать или обновить registry row;
5. создать issue state и reason;
6. только потом анализировать и отвечать.

Если любой шаг persistence не прошёл, workflow останавливается.

## input_manifest schema

```json
{
  "input_id":"input_YYYYMMDD_HHMMSS_slug",
  "source":"chat|file|github|manual",
  "received_at":"ISO-8601",
  "reserved_before_analysis":true,
  "entry_path":"Inbox/<input_id>/entry.md",
  "attachments":[],
  "linked_issue_ids":[],
  "cleanup_status":"active|archived|tombstoned",
  "cleanup_reason":null,
  "language":"ru|mixed_allowed",
  "hash":"sha256:<hex>"
}
```

## Registry row JSONL schema

Одна строка `Issues/registry.jsonl`:

```json
{
  "issue_id":"svc_YYYYMMDD_slug",
  "scope":"service",
  "type":"simple|complex|linked",
  "status":"proposed|open|waiting_user|approved|executing|validating|closed|blocked|tombstoned",
  "title":"...",
  "reason_path":"Issues/active/<issue_id>/reason.md",
  "state_path":"Issues/active/<issue_id>/state.json",
  "source_input_id":"input_...",
  "parent_id":null,
  "child_ids":[],
  "linked_issue_ids":[],
  "depends_on":[],
  "blocks":[],
  "affected_files":[],
  "next_expected_step":"...",
  "created_at":"ISO-8601",
  "updated_at":"ISO-8601",
  "closed_at":null
}
```

## State transitions

```text
proposed -> open -> waiting_user -> approved -> executing -> validating -> closed
proposed -> tombstoned
open|waiting_user|approved|executing|validating -> blocked
blocked -> open|tombstoned
```

Запрещены прямые переходы `proposed -> executing`, `closed -> executing`, `tombstoned -> open` без отдельного repair issue.

## Commands

Поддерживаются: `утверждаю всё`, `утверждаю: ID`, `отклоняю: ID`, `обсудить: ID`, `отложить: ID`, `изменить: ID`, `добавить: title + reason`, `фокус: ID`.

Combined decisions применяются атомарно: сначала валидируется весь список команд, затем выполняется один registry update.

## Reason mirror

Полный Reason в ответе пользователю и `reason.md` должны совпадать побуквенно. Проверка: byte-for-byte comparison UTF-8. При mismatch ответ не отправляется, issue получает `blocked: reason_mirror_mismatch`.

## Cleanup and tombstone

Tombstone сохраняет history. Минимальные поля: `tombstoned_at`, `tombstone_reason`, `replaced_by`, `references_repaired`, `links_removed_or_redirected`. Cleanup разрешён только после проверки backlinks, registry references и parent/child links.
