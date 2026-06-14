# Жизненный цикл issue

[Назад к README](../../README.md)

## Назначение

Основной протокол для служебных и концептных issue: продолжение работы, причина, вопросы, требования, план, решение, договор, выполнение, отчёт и закрытие. Этот файл задаёт рабочий процесс; [complex_linked.md](complex_linked.md) расширяет его для дочерних и зависимых задач.

## Связанные файлы

- [Service Mode](../service/service_mode.md)
- [Execution Mode](../execution/execution_mode.md)
- [Входные материалы и registry](../service/input_registry.md)
- [Сложные и связанные issue](complex_linked.md)
- [Шаблон issue](../../Templates/issue/README.md)

## Папка issue

```text
state.json
reason.md
qa.md                  # только если нужны реальные вопросы
requirements.md
plan.md
solution.md
contract.md
output/report.md
output/attachments/    # только если вложения действительно нужны
```

`output/report.md` является единственным допустимым именем отчёта. `output_report.md` считается неверным путём шаблона.

## Registry и продолжение

Продолжение начинается со строки registry, а не с памяти. Агент открывает:

1. нужную строку registry;
2. `state.json` задачи;
3. `reason.md`;
4. файл текущей фазы;
5. строки parent/child/linked, если они есть;
6. пакет фокуса.

Если registry row и state расходятся, issue получает статус `blocked: registry_state_conflict` до ремонта.

## Причина и зеркало

`reason.md` — каноническая сохранённая причина. Если issue создана из предложения, показанного пользователю, текст причины в ответе и `reason.md` должны совпадать byte-for-byte. При несовпадении выполнение и закрытие блокируются до ремонта зеркала.

## Проверка вопросов

`qa.md` создаётся только если нужны реальные вопросы. Если вопросы не нужны, state и requirements фиксируют:

```yaml
qa_required: false
qa_decision_reason: "..."
qa_file_created: false
```

Если вопросы обязательны, requirements нельзя approve до ответа на вопросы или явного отказа пользователя от вопросов.

## Проверка requirements

`requirements.md` содержит:

```yaml
status: missing|draft|approved
source_reason: path
acceptance_criteria: []
out_of_scope: []
unknowns: []
user_approval: required|received|waived_with_reason
```

План может стартовать только после `requirements_status=approved`, кроме явной исследовательской issue, где результатом являются вопросы, а не изменение рабочих файлов.

## Проверки плана, решения и договора

| Файл | Минимальное содержание | Условие допуска |
|---|---|---|
| `plan.md` | steps, affected files, validation plan, rollback/repair note | утверждён до решения |
| `solution.md` | selected approach, alternatives rejected, exact file operations | утверждён до договора |
| `contract.md` | allowed files, blocked files, persistence order, success/failure evidence | утверждён до выполнения; английские элементы здесь являются именами полей договора |

Выполнение без known affected files и persistence plan запрещено.

## Проверка выполнения

Перед изменением файлов:

```yaml
requirements_approved: true
plan_approved: true
solution_approved: true
contract_approved: true
affected_files_known: true
blocked_files_respected: true
persistence_order_known: true
validation_plan_known: true
```

Atomic repair exception фиксирует reason, files, checks и state/output evidence. После завершения ремонта активная service exception должна быть сброшена, если она была использована.

## Схема output/report

`output/report.md` содержит:

```yaml
issue_id: string
scope: service|concept
reason_path: path
commit_sha: string|null
changed_files: []
registry_updated: true|false
state_updated: true|false
checks_run: []
link_orphan_result: pass|fail|not_applicable
language_gate: pass|fail|not_applicable
residual_risks: []
closure_allowed: true|false
next_expected_step: string
```

Вложения размещаются в `output/attachments/` только если нужны и должны быть указаны в report.

## Переходы закрытия

```text
validating -> closed       only if output verified and propagation done
validating -> blocked      if persistence/check failed
closed -> open             only via separate repair issue
open|approved -> tombstoned if user rejects or supersedes before execution
```

Закрытие требует строки registry, state задачи, output/report, распространения к parent/child, проверки ссылок и фактического сохранения.

## Переквалификация

Issue переквалифицируется, когда простая задача становится сложной, service становится concept, появляется child issue или dependency, меняется scope requirements, output влияет на другую issue или affected files переходят границы режимов. Переквалификация обновляет registry row, state, пакет фокуса и при необходимости передаёт работу в [complex_linked.md](complex_linked.md).

## Вариант concept issue

Concept issue использует те же проверки, но allowed mutations остаются внутри `Concepts/<slug>/`, кроме `State/execution_index_state.json`, если меняется summary активной концепции. Manifest, structure, local registry и обновления состояния concept являются жёсткими условиями для создания, удаления или переименования страниц.
