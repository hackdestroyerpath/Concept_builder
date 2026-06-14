# Шаблон issue

[Назад к жизненному циклу issue](../../Protocols/issue/issue_lifecycle.md)

## Назначение

Краткий шаблон структуры служебной или концептной issue. Основной рабочий процесс описан в [issue_lifecycle.md](../../Protocols/issue/issue_lifecycle.md); этот файл не заменяет протокол.

## Связанные файлы

- [Жизненный цикл issue](../../Protocols/issue/issue_lifecycle.md)
- [Сложные и связанные issue](../../Protocols/issue/complex_linked.md)

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
- Пустые декоративные файлы не создаются.
- Reason mirror проверяется byte-for-byte, если issue создаётся из причины, показанной пользователю.
- Закрытие требует сохранённых registry/state, output/report, распространения parent-child и проверки ссылок.
- Concept issue дополнительно обновляет local manifest, structure, concept state и local registry.
