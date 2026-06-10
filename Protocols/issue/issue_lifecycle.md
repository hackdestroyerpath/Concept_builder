# Issue lifecycle

[Назад к README](../../README.md)

Связанные файлы:
- [Startup protocol](../common/startup.md)
- [Focus packet](../common/focus_packet.md)
- [State update](../common/state_update.md)
- [Service Mode](../service/service_mode.md)
- [Execution Mode](../execution/execution_mode.md)
- [State schema](../../State/state_schema.md)

## Назначение

Протокол описывает жизненный цикл service-level и concept-level issue.
Issue используется, когда задача требует восстановления контекста, проверки требований, плана, результата или контракта.

## Когда issue нужен

Issue нужен, если задача меняет несколько файлов, state, registry, protocol, manifest или structure, либо должна быть продолжена в новом чате.
Simple task можно выполнить без отдельной issue folder, если изменение атомарное и сразу проверяемое.

## Типы issue

- `simple` — малое атомарное изменение без отдельной папки issue.
- `complex` — задача с отдельной папкой issue и полным lifecycle.
- `service` — обслуживание системы.
- `concept` — работа внутри конкретной концепции.

## Структура complex issue

```text
state.json
reason.md
requirements.md
plan.md
solution.md
contract.md
output/report.md
```

Файлы создаются только если применимы. Пустая декоративная папка не является прогрессом, как бы грустно это ни было для любителей папок.

## Resume

При продолжении issue агент открывает registry, `state.json`, проверяет статусы и загружает только relevant files.
Если state конфликтует с registry, сначала выполняется repair.

## QA

QA нужен, если без ответа пользователя нельзя безопасно определить требования, границы, приоритет или expected output.
QA можно пропустить только с записанным `qa_decision_reason`.

## Requirements

`requirements.md` фиксирует проверяемые требования.
Статусы: `missing`, `draft`, `approved`.

## Requalification

Issue переквалифицируется, если simple стал complex, service стал concept, появились child issues, зависимости или изменились требования.

## Plan

`plan.md` задаёт порядок действий и affected files.
Plan нельзя считать approved без approved requirements.

## Solution

`solution.md` фиксирует выбранное решение и существенные tradeoffs.

## Contract

`contract.md` задаёт проверяемый контракт результата: files, registry, state, links, orphan check и user output.

## Output report

`output/report.md` фиксирует фактический результат, commit sha, проверки и остатки.

## Closure

Закрытие разрешено только если requirements, plan, solution, contract и output report готовы, registry и state обновлены, persistence verified, blocking questions пусты, broken links и orphan files отсутствуют.

## Persistence gate

Перед ответом пользователю relevant state, registry и production files должны быть записаны через GitHub Connector.
Если запись не выполнена, issue нельзя считать закрытым.
