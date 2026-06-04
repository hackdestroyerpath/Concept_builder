# Execution Mode

[← Назад к README](../../README.md)

Связанные файлы:
- [Протокол запуска](../common/startup.md)
- [Загрузка контекста](../common/context_loading.md)
- [Жизненный цикл issue](../common/issue_lifecycle.md)
- [Экспорт концепции](concept_export.md)
- [Concepts](../../Concepts/README.md)

## Назначение

`Execution Mode` создаёт, уточняет, проверяет и экспортирует пользовательские концепции внутри `Concepts/`. Он использует общее ядро системы, но не обслуживает системные протоколы.

## Объект работы

Одна концепция хранится так:

```text
Concepts/<concept_slug>/
├── README.md
├── about.md
├── operating_model.md
├── requirements.md
├── process.md
├── pages/
├── Issues/
├── manifest.jsonl
├── structure.md
└── state.json
```

## Запуск режима

1. Открыть root `README.md`.
2. Прочитать `State/execution_index_state.json`.
3. Загрузить только `active_protocols`.
4. Если активная концепция выбрана, открыть её `README.md`, `manifest.jsonl`, `structure.md` и `state.json`.
5. Не читать все концепции без причины.
6. Ответить с `Health marker`.

## Создание концепции

Агент отделяет точку входа от вложений, сохраняет входные данные в `Inbox/`, предлагает `concept_slug`, создаёт каркас концепции, обновляет manifest, structure и state, затем предлагает первый concept issue.

## Продолжение концепции

Минимальная загрузка: concept `README.md`, `manifest.jsonl`, `structure.md`, `state.json`, concept registry и файлы текущего фокуса.

## Concept issue

Любое изменение концепции идёт через concept issue: `reason.md`, `requirements.md`, `plan.md`, `solution.md`, `contract.md`, `output/report.md`.

Разрешено менять только файлы выбранной концепции. Изменение `State/`, `Protocols/`, `Instructions/`, root README или service issue architecture требует перехода в `Service Mode`.

## Лифт фокуса

Агент движется так:

```text
repository → concept → page group → page → issue → requirement → solution → output
```

При подъёме агент сохраняет output, обновляет parent state и оставляет summary вместо лишних деталей.
