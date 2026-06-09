# Execution Mode protocol

[Назад к README](../../README.md)

Связанные файлы:
- [Startup protocol](../common/startup.md)
- [Focus packet](../common/focus_packet.md)
- [State update](../common/state_update.md)
- [Execution index state](../../State/execution_index_state.json)
- [Concepts root](../../Concepts/root.md)
- [Repository file index](../../Repository/file_index.jsonl)
- [Repository link graph](../../Repository/link_graph.md)

## Назначение

`Execution Mode` ведёт пользовательские концепции в `Concepts/`.
Этот режим работает с конкретной concept folder и не обслуживает системные протоколы как основную задачу.

## Границы режима

Разрешено:

- создавать новую concept folder после явного запроса пользователя;
- вести Markdown-файлы конкретной концепции;
- обновлять concept manifest, structure и state;
- создавать concept-level issue;
- готовить export концепции;
- обновлять `State/execution_index_state.json`, если меняется список или active concept.

Запрещено:

- менять `Protocols/`, `State/state_schema.md`, project instructions и repository map как основную работу;
- создавать demo concepts без реального пользовательского запроса;
- читать все concepts сразу без причины;
- утверждать, что state или export сохранены, если GitHub-запись не прошла.

## Создание концепции

Новая концепция создаётся только после явного запроса пользователя.
Путь концепции: `Concepts/<concept_slug>/`.

Минимальный состав:

```text
README.md
manifest.jsonl
structure.md
state.json
Issues/registry.jsonl
```

## Concept issue workflow

1. Принять пользовательский запрос внутри active concept.
2. Проверить, нужен ли concept-level issue.
3. Для complex issue создать `Concepts/<concept_slug>/Issues/active/<issue_id>/`.
4. Зафиксировать reason, state, requirements, plan, solution, contract и output, если применимо.
5. Обновить concept issue registry.
6. Изменить concept files.
7. Обновить manifest, structure, concept state и execution index state.
8. Проверить persistence перед ответом.

## Export

Export разрешён только из согласованного состояния концепции.
Перед export нужно проверить:

```yaml
concept_state_current: true
manifest_current: true
structure_current: true
open_blocking_issues: []
export_target_known: true
```

## Ответ пользователю

Ответ должен содержать изменённые файлы, commit sha, текущий state flag и следующий допустимый шаг.
