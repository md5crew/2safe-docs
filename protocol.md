## Описание протокола взаимодействия демона и клиента

Взаимодействие происходит через Unix-сокет `/tmp/2safe.sock`

Настоящий протокол содержит спецификацию сообщений для:
* Управление настройками;
* Совершения вызовов API;
* Оповещения об событиях.

#### Настройки
##### *settings* # набор значений для определённых полей
##### *set_settings* # установка значений для определённых полей
```json
запрос: {
  "type": "set_settings",
  args: {
    "login": "FooBaz",
    "email": "foo@baz.com"
  }
}
```
##### *get_settings* # получение значений для определённых полей
```json
запрос: {
  "type": "get_settings",
  "fields": ["login", "email"]
}

ответ: {
  "type": "settings",
  values: {
    "login": "FooBaz",
    "email": "foo@baz.com"
  }
}
```
#### Вызовы API
##### *call_api* # вызов метода API (см. [safecalls.h](https://github.com/Xlab/lib2safe/blob/master/safecalls.h))
```json
запрос: {
  "type": "call_api",
  "call": "chk_mail"
  args: { "email": "foo@baz.com" }
}
```

#### События изменения состояни##### *event*dfdf
