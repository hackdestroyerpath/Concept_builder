# Связанные и сложные issue

[← Назад к README](../../README.md)

Связанные файлы:
- [Жизненный цикл issue](issue_lifecycle.md)
- [Загрузка контекста](context_loading.md)
- [Обновление state](state_update.md)
- [Execution Mode](../execution/execution_mode.md)

## Назначение

Протокол описывает complex issue, child issue, зависимости, порядок выполнения, распространение результата, защиту от бесконечной декомпозиции и очистку. Без этого агент начинает “логически делить задачу”, что обычно означает “плодить папки, пока все не забудут исходную цель”.

## Complex issue

Issue считается complex, если requirements распадаются на самостоятельные результаты, требуют разных approvals, имеют зависимости или не проверяются одним contract.

После утверждения типа `complex` агент:
1. перечитывает `reason.md`, `requirements.md` и зависимости;
2. создаёт parent `solution.md` как план декомпозиции;
3. предлагает child issue candidates;
4. фиксирует для каждого child: title, reason, parent_id, expected output, dependencies и acceptance implication;
5. обновляет registry и parent state;
6. ждёт approval пользователя.

## Типы связей

| Relation | Значение | Блокирует выполнение |
|---|---|---|
| `parent_id` / `child_ids` | иерархия complex issue | parent ждёт blocking children |
| `blocks` | текущий issue блокирует другие | downstream issue ждут |
| `depends_on` | текущий issue ждёт другие issue | да |
| `uses_output_of` | текущий issue использует output другого issue | да, пока output не создан |
| `related_to` | смысловая связь без жёсткой зависимости | нет |

`linked_issue_ids` можно использовать как summary, но канонические связи хранятся в relation fields.

## Readiness gate

Issue готов к выполнению, если закрыты blocking dependencies, доступен required output, parent не блокирует child, а related issues просмотрены без лишней загрузки контекста.

Если gate не пройден, issue получает blocker reason: `waiting_dependency`, `waiting_output`, `parent_blocked` или `relationship_conflict`.

## Propagation

После closure агент обновляет parent state, downstream issue из `blocks`, `depends_on`, `uses_output_of`, registry rows и `output/report.md` закрытого issue.

## Recursion safeguard

Декомпозиция останавливается, если child не уменьшает неопределённость, не имеет собственного reason, повторяет output parent или создаёт approval-циклы без пользы.

Default policy:

```text
max_active_depth = 3 active complex levels
```

Перед входом в child агент сохраняет parent summary: parent goal, why child exists, return condition, context to keep, context to drop.

## Tombstone и очистка

Rejected или physically deleted issue не исчезает бесследно. Compact trace сохраняется в `registry_archive.jsonl` или `tombstones.jsonl`: issue_id, title, source_input_id, parent_id, decision, decision_reason, reason_hash, output_report_path, deleted_paths, decided_at, decided_by.

Heavy folders удаляются только после tombstone, проверки dependencies и parent/export-gated решения.
