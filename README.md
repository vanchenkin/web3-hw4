# Отчет HW4

Команды для запуска:

```
export CERTORAKEY=<personal_access_key>
certoraRun ./certora/conf/default.conf --optimistic_loop
```

** Предварительно поставил все для certora - https://docs.certora.com/en/latest/docs/user-guide/install.html 

Отчет до изменений(успешный) - https://prover.certora.com/output/9299541/3153f1a9d7e34a91801449c5cec3ba4c?anonymousKey=b75c4027d09a622c82183cae3b6962bb55f250ed

Отчет после изменений - https://prover.certora.com/output/9299541/a9c93b5668f0411f89857764ab64f43a?anonymousKey=1fc156973b02fc746395de85e445693bce43f5af

Изменения:

- при transferFrom убрана функция, которая тратит allowance
- при transfer переводится количество монет минус 10
- при вызове approve мы дополнительно начисляем пользователю токены

Нарушения правил:

- Rule: transfer behavior and side effects
    - изменение: при transfer переводится количество монет минус 10
    - transfer вызвал revert, и сработал assert на единственные возможные условия реверта, которые не выполнились
- Rule: transferFrom behavior and side effects
    - изменение: при transferFrom убрана функция, которая тратит allowance
    - так как убрана функция, то не срабатывает assert, который проверяет что allowanceBefore больше, чем мы перевели
- Rule: only the token holder or an approved third party can reduce an account's balance 
    - изменение: при transferFrom убрана функция, которая тратит allowance
    - убрали проверку allowance, соответственно можем делать неавторизованные переводы
- Rule: only the token holder or an approved third party can reduce an account's balance 
    - изменение: при вызове approve мы дополнительно начисляем пользователю токены
    - добавляем токены пользователю - нарушаем правило
- Rule: only mint and burn can change total supply
    - изменение: при вызове approve мы дополнительно начисляем пользователю токены
    - нарушен инвариант totalSupply, так как начисляем токены