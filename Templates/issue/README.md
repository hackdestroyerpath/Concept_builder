# Issue template

[Назад к issue lifecycle](../../Protocols/issue/issue_lifecycle.md)

## Назначение

Summary-шаблон структуры service или concept issue. Primary workflow описан в [issue_lifecycle.md](../../Protocols/issue/issue_lifecycle.md); этот файл не заменяет protocol.

## Связанные файлы

- [Issue lifecycle](../../Protocols/issue/issue_lifecycle.md)
- [Complex linked issue](../../Protocols/issue/complex_linked.md)

## Состав

```text
state.json
reason.md
qa.md                  # только если нужны вопросы
requirements.md
plan.md
solution.md
contract.md
output/report.md
output/attachments/    # только если есть вложения
```

## Правила

- `output/report.md` — единственное допустимое имя отчёта.
- Empty decorative files не создаются.
- Reason mirror проверяется byte-for-byte, если issue создаётся из user-facing reason.
- Closure требует registry/state persistence, output/report, parent-child propagation and link/orphan check.
- Concept issue дополнительно обновляет local manifest, structure, concept state and local registry.
