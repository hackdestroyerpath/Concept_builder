# Входные материалы и registry

[Назад к README](../../README.md)

## Назначение

Основной протокол для `Inbox/`, `input_manifest.json`, служебного registry, зеркалирования причины, ограниченного резерва и правил cleanup/tombstone. `Registry` — это машинный реестр строк JSONL.

## Связанные файлы

- [Service Mode](service_mode.md)
- [Жизненный цикл issue](../issue/issue_lifecycle.md)
- [Inbox](../../Inbox/README.md)
- [Служебный registry](../../Issues/registry.jsonl)

## Компактный и некомпактный вход

Компактный вход можно обработать без `Inbox/<input_id>/`, если все условия истинны:

```yaml
single_turn_complete: true
no_attachment: true
no_multi_issue_split_required: true
no_deferred_user_decision: true
no_need_to_reconstruct_source_later: true
```

Если хотя бы одно условие false, включается ограниченный резерв. Анализ до резерва запрещён.

## Порядок ограниченного резерва

Для некомпактного входа порядок обязателен:

1. создать `Inbox/<input_id>/entry.md` с кратким описанием источника или полным допустимым материалом;
2. создать `Inbox/<input_id>/input_manifest.json`;
3. сохранить нужные вложения;
4. создать или обновить строку registry;
5. создать state задачи и reason, то есть сохранённую причину;
6. только потом анализировать и отвечать.

Если любой шаг сохранения не прошёл, процесс останавливается, а ответ сообщает: `Persistence не выполнен: input/registry не сохранён.`

## Схема input_manifest

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

Hash покрывает содержимое `entry.md` и поля manifest, кроме самого поля `hash`.

## Схема строки registry JSONL

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

## Переходы состояния

```text
proposed -> open -> waiting_user -> approved -> executing -> validating -> closed
proposed -> tombstoned
open|waiting_user|approved|executing|validating -> blocked
blocked -> open|tombstoned
closed -> open only through separate repair issue
```

Прямые переходы `proposed -> executing`, `closed -> executing`, `tombstoned -> open` запрещены без отдельной ремонтной issue.

## Команды

Поддерживаются команды:

- `утверждаю всё`;
- `утверждаю: ID[, ID]`;
- `отклоняю: ID[, ID]`;
- `обсудить: ID`;
- `отложить: ID`;
- `изменить: ID + текст`;
- `добавить: title + reason`;
- `фокус: ID`.

Составные решения применяются атомарно: сначала валидируется весь список команд, затем выполняется одно обновление registry. Если один ID неверен, изменения не применяются частично.

## Зеркало причины

Полный `Reason` в ответе пользователю и `reason.md` должны совпадать побуквенно. Проверка: byte-for-byte comparison UTF-8 после нормализации переносов строк к LF. При несовпадении ответ не отправляется, issue получает `blocked: reason_mirror_mismatch`, а пользователь видит действие ремонта.

Поля ответа для предложенной issue:

```yaml
Reason source: chat|entry.md|file
Reason mirror: exact|blocked
Registry persistence: written|not_written
Next action: approve|discuss|reject|edit
```

## Cleanup и tombstone

Tombstone сохраняет identity/history, то есть идентичность и историю. Минимальные поля:

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

Cleanup разрешён только после проверки обратных ссылок, ссылок registry, ссылок state и связей parent/child. Удаление без tombstone trace запрещено для issue или input, которые уже упоминались в registry или output/report.

## Шаблоны ответа

### Сохранённое предложение issue

```text
Issue создан: <issue_id>
Reason source: <source>
Reason mirror: exact
Registry persistence: written
Доступные действия: утвердить, обсудить, изменить, отклонить, отложить.
```

### Ошибка сохранения

```text
Persistence не выполнен: registry/input не сохранён.
Нельзя объявлять issue созданным или продолжать analysis.
```
