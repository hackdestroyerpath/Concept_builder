# Инструкция проекта: Concept Builder Service Mode

[Назад к README](../README.md)

## Назначение

Краткая инструкция для `Service Mode` (сервисного режима). Полные правила находятся в рабочих файлах GitHub.

## Связанные файлы

- [README](../README.md)
- [Состояние service](../State/service_state.json)
- [Протокол запуска](../Protocols/common/startup.md)
- [Service Mode](../Protocols/service/service_mode.md)

## Запуск

1. GitHub Connector используется для чтения и записи.
2. Открываются `README.md`, `State/service_state.json`, `Repository/file_index.jsonl` и `Repository/link_graph.md`.
3. Выполняется `Protocols/common/startup.md`.
4. Если `pending_user_action` не `null`, сначала показывается ожидаемое действие.
5. Загружаются только `active_protocols` из состояния и файлы из пакета фокуса.

## Рабочий режим

Для обслуживания системы применяется `Protocols/service/service_mode.md`. Для входных материалов и registry применяется `Protocols/service/input_registry.md`. Для служебных задач применяются `Protocols/issue/issue_lifecycle.md` и при необходимости `Protocols/issue/complex_linked.md`.

## Проверка изменения системных файлов

Изменение системных файлов требует утверждённой служебной задачи, утверждённых требований, решения и договора или записанного аварийного исключения. Список затронутых файлов фиксируется до записи.

## Восстановление

Если хэш состояния неверен, фокус потерян, реестр конфликтует с состоянием или уверенность контекста низкая, выполняется восстановление по `Protocols/common/focus_packet.md`.

## Сохранение

Перед ответом сохраняются рабочие файлы, реестр, индекс, карта и нужное состояние. `persisted=yes` допустимо только после фактической записи через GitHub Connector.

## Маркер здоровья

```text
mode=service; focus=<focus>; phase=<phase>; persisted=<yes|no>; next=<next>; context_confidence=<high|medium|low>
```
