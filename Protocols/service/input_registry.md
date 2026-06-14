# Входные материалы и registry

[Назад к README](../../README.md)

## Назначение

Основной protocol для `Inbox/`, `input_manifest.json`, service issue registry, reason mirror, limited reserve и cleanup/tombstone policy.

## Связанные файлы

- [Service Mode](service_mode.md)
- [Issue lifecycle](../issue/issue_lifecycle.md)
- [Inbox](../../Inbox/README.md)
- [Service issue registry](../../Issues/registry.jsonl)

## Compact и non-compact input

Compact input можно обработать без `Inbox/<input_id>/`, если все условия истинны:

```yaml
single_turn_complete: true
no_attachment: true
no_multi_issue_split_required: true
no_deferred_user_decision: true
no_need_to_reconstruct_source_later: true
```

Если хотя бы одно условие false, включается limited reserve order. Анализ до reserve запрещён.

## Limited reserve order

Для non-compact input порядок обязателен:

1. создать `Inbox/<input_id>/entry.md` с source summary или полным допустимым материалом;
2. создать `Inbox/<input_id>/input_manifest.json`;
3. сохранить needed attachments;
4. создать или обновить registry row;
5. создать issue state и reason;
6. только потом анализировать и отвечать.

Если любой шаг persistence не прошёл, workflow останавливается, а response сообщает: `Persistence не выполнен: input/registry не сохранён.`

## input_manifest schema

```json
{
  "input_id": "input_YYYYMMDD_HHMMSS_slug",
  "source": "chat|file|github|manual",
  "received_at": "ISO-8601",
  "reserved_before_analysis": true,
  "entry_path": "Inbox/<input_id>/entry.md",
  "attachments": [],
  "linked_issue_ids": [],
  "cleanup_status": "active|archived|tombstoned",
  "cleanup_reason": null,
  "language": "ru|mixed_allowed",
  "hash": "sha256:<hex>"
}
```

Hash покрывает content файла `entry.md` и поля manifest, кроме самого поля `hash`.

## Registry row JSONL schema

Одна строка `Issues/registry.jsonl`:

```json
{
  "issue_id": "svc_YYYYMMDD_slug",
  "scope": "service",
  "type": "simple|complex|linked|child",
  "status": "proposed|open|waiting_user|approved|executing|validating|closed|blocked|tombstoned",
  "title": "...",
  "reason_path": "Issues/active/<issue_id>/reason.md",
  "state_path": "Issues/active/<issue_id>/state.json",
  "source_input_id": "input_...",
  "parent_id": null,
  "child_ids": [],
  "linked_issue_ids": [],
  "depends_on": [],
  "blocks": [],
  "affected_files": [],
  "next_expected_step": "...",
  "created_at": "ISO-8601",
  "updated_at": "ISO-8601",
  "closed_at": null
}
```

## State transitions

```text
proposed -> open -> waiting_user -> approved -> executing -> validating -> closed
proposed -> tombstoned
open|waiting_user|approved|executing|validating -> blocked
blocked -> open|tombstoned
closed -> open only through separate repair issue
```

Запрещены прямые переходы `proposed -> executing`, `closed -> executing`, `tombstoned -> open` без отдельного repair issue.

## Commands

Поддерживаются команды:

- `утверждаю всё`;
- `утверждаю: ID[, ID]`;
- `отклоняю: ID[, ID]`;
- `обсудить: ID`;
- `отложить: ID`;
- `изменить: ID + текст`;
- `добавить: title + reason`;
- `фокус: ID`.

Combined decisions применяются атомарно: сначала валидируется весь список команд, затем выполняется один registry update. Если один ID invalid, изменения не применяются частично.

## Reason mirror

Полный `Reason` в ответе пользователю и `reason.md` должны совпадать побуквенно. Проверка: byte-for-byte comparison UTF-8 после нормализации line endings к LF. При mismatch response не отправляется, issue получает `blocked: reason_mirror_mismatch`, а пользователь видит repair action.

Поля response для proposed issue:

```yaml
Reason source: chat|entry.md|file
Reason mirror: exact|blocked
Registry persistence: written|not_written
Next action: approve|discuss|reject|edit
```

## Cleanup and tombstone

Tombstone сохраняет identity/history. Минимальные поля:

```json
{
  "tombstoned_at": "ISO-8601",
  "tombstone_reason": "...",
  "replaced_by": null,
  "references_repaired": true,
  "links_removed_or_redirected": [],
  "registry_row_preserved": true
}
```

Cleanup разрешён только после проверки backlinks, registry references, state references и parent/child links. Удаление без tombstone trace запрещено для issue/input, которые уже упоминались в registry или output/report.

## Response templates

### Saved issue proposal

```text
Issue создан: <issue_id>
Reason source: <source>
Reason mirror: exact
Registry persistence: written
Доступные действия: утвердить, обсудить, изменить, отклонить, отложить.
```

### Persistence failure

```text
Persistence не выполнен: registry/input не сохранён.
Нельзя объявлять issue созданным или продолжать analysis.
```
