# Создание точного триггера времени на каждый день

## Код

```js
/**
 * Создает триггер для вызвающей функции
 *
 * @param {{
 *   h: number;
 *   m: number;
 *   s: number;
 *   ms: number;
 * }} param0
 */
function createAccurateTimeBasedTrigger_({ h, m, s, ms }) {
  const fnName = createAccurateTimeBasedTrigger_.caller.name;
  const date = new Date();
  date.setHours(h || 0, m || 0, s || 0, ms || 0);
  date.setTime(date.getTime() + 24 * 60 * 60 * 1000);

  ScriptApp.newTrigger(fnName).timeBased().at(date).create();
}
```

## Вызов

### Пример вызова

Каждый раз, когда будет вызываться функция `myFunction` будет создаваться триггер для ее выполнения через сутки в 3:32 по времени скрипта

```js
function myFunction() {
  // Необходимо поместить вначала функции
  createAccurateTimeBasedTrigger_({ h: 3, m: 32 });
}
```

Пример, когда `myFunction` будет вызываться каждые сутки в 0:00

```js
function myFunction() {
  // Необходимо поместить вначала функции
  createAccurateTimeBasedTrigger_();
}
```

### Реальное тестирование

В [Таблице для тестирования] подобный триггер создает запись каждый день. Посмотрим, надолго ли его хватит

[таблице для тестирования]: https://docs.google.com/spreadsheets/d/1IMN6nOkxSJLXv2Oy0Ug0X3hjTK3nQ4zWDF2QsJHppGA/edit?usp=sharing
