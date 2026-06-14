# Сложные и связанные issue

[Назад к жизненному циклу issue](issue_lifecycle.md)

## Назначение

Основной протокол для complex issue, child issue, linked issue, защит зависимостей, готовности и распространения результатов.

## Связанные файлы

- [Жизненный цикл issue](issue_lifecycle.md)
- [Входные материалы и registry](../service/input_registry.md)
- [Execution Mode](../execution/execution_mode.md)

## Критерии сложности

Complex issue нужна, если задача имеет независимые рабочие части, утверждение child issue, разные scope, цепочку dependency, отдельный output contract, риск частичного утверждения или риск рекурсии.

## Поля parent issue

Parent state добавляет:

```json
{
  "decomposition_reason": "...",
  "child_candidates": [],
  "approved_child_ids": [],
  "rejected_child_ids": [],
  "parent_acceptance_logic": "all_children_closed|selected_children_closed|manual_parent_review",
  "summary_propagation_required": true
}
```

## Рабочий процесс утверждения child issue

1. Parent предлагает child candidates как строки `proposed`, а не как строки `open`.
2. Пользователь может утвердить все, утвердить выбранные, отклонить выбранные, обсудить или изменить child candidates.
3. Утверждённые children переходят в `open` и получают skeleton из state, reason и requirements.
4. Отклонённые children получают tombstone row с reason.
5. Parent может выполнять только части, не заблокированные children; parent закрывается только когда утверждённые children закрыты, waived или явно superseded.

## Схема связей

```json
{
  "parent_id": null,
  "child_ids": [],
  "blocks": [],
  "depends_on": [],
  "uses_output_of": [],
  "related_to": [],
  "relationship_status": "proposed|approved|satisfied|blocked|tombstoned",
  "propagation_required": true,
  "propagation_targets": []
}
```

## Готовность linked issue

Issue может выполняться только если:

```yaml
dependencies_closed_or_waived: true
required_outputs_exist: true
no_cycle_in_dependency_graph: true
parent_allows_execution: true
blocking_children: []
contract_allows_cross_file_change: true
```

Если linked issue использует output другой issue, путь `output/report.md` и commit SHA должны быть записаны до выполнения. Зависимая issue не стартует, пока нужный output не сохранён и не проверен.

## Защитные правила

- Максимальная глубина: 3, если пользователь явно не утвердил более глубокое деление.
- Максимум предложенных children за один проход: 7.
- Граф зависимостей должен быть ациклическим.
- Child не может менять parent files, если договор это не разрешает.
- Parent не закрывается результатом одной краткой сводки.
- Отклонённый child сохраняет tombstone; удаление без следа запрещено.
- Межрежимное изменение переводится в служебную issue.

## Распространение после закрытия

После закрытия child/linked agent обновляет:

1. child output/report;
2. child state;
3. parent summary и state;
4. downstream issue state, если `blocks` или `uses_output_of` изменились;
5. строки registry;
6. link graph или manifest, если изменились файлы.

Если propagation fails, closure блокируется со статусом `blocked: propagation_failed`.

## Пример dry-run

```yaml
parent: svc_parent
children_proposed: [svc_child_a, svc_child_b]
user_decision: approve svc_child_a, reject svc_child_b
result:
  svc_child_a: open
  svc_child_b: tombstoned
  parent_acceptance_logic: selected_children_closed
  propagation_required: true
```
