# Issue lifecycle

[Назад к README](../../README.md)

## Назначение

Primary protocol для service-level и concept-level issue: resume, reason, QA, requirements, plan, solution, contract, execution, output/report и closure.

## Связанные файлы

- [Service Mode](../service/service_mode.md)
- [Execution Mode](../execution/execution_mode.md)
- [Input registry](../service/input_registry.md)
- [Complex linked issue](complex_linked.md)
- [Issue template](../../Templates/issue/README.md)

## Issue folder

```text
state.json
reason.md
qa.md
requirements.md
plan.md
solution.md
contract.md
output/report.md
```

`output/report.md` является единственным допустимым именем отчёта.

## Lifecycle gates

1. Resume: открыть registry row, local state, links and focus packet.
2. QA: создать `qa.md`, только если есть реальные вопросы; иначе записать `qa_decision_reason`.
3. Requirements: `requirements.md` имеет status `missing|draft|approved`.
4. Plan starts only after approved requirements.
5. Solution starts only after approved plan.
6. Contract starts only after approved solution.
7. Execution starts only when affected files and persistence plan are known.
8. Output report records commit, changed files, checks, registry/state status, link/orphan result, language gate, residual risks and next step.
9. Closure allowed only after registry, state, output/report, parent-child propagation and persistence are verified.

## Requalification

Issue requalifies when simple becomes complex, service becomes concept, child issue or dependency appears, requirements scope changes, or output affects another issue.
