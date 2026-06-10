# Complex and linked issue

[Назад к README](../../README.md)

## Назначение

Протокол описывает complex issue, child issue, linked issue, зависимости, блокировки и propagation результата.

## Complex issue

Complex issue нужен, если задача распадается на самостоятельные work units, требует child issue или не может честно закрыться одним contract.
Parent issue хранит decomposition rationale, child candidates, parent acceptance logic и integration check.

## Child approval

Child issue сначала создаётся как proposed. После команды пользователя `утверждаю children` proposed children становятся approved.
Отклонённые children получают tombstone или registry note.

## Relationships

Связи хранятся в registry и local issue state:

- `parent_id`
- `child_ids`
- `blocks`
- `depends_on`
- `uses_output_of`
- `related_to`

## Readiness

Issue можно выполнять только если dependencies закрыты, required outputs существуют, parent не блокирует выполнение и нет unresolved blockers.

## Propagation

После закрытия issue агент обновляет parent, downstream issue, dependency status, output links и registry.
Если propagation не выполнена, issue не считается полностью закрытым.
