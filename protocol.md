## Описание протокола взаимодействия демона и клиента

Взаимодействие происходит через Unix-сокет `/tmp/2safe.sock`

Настоящий протокол содержит спецификацию сообщений для:
* Управления настройками;
* Выполнения действий;
* Совершения вызовов API;
* Оповещения о событиях.

#### Настройки
##### *settings* # набор значений для определённых полей
##### *set_settings* # установка значений для определённых полей
```json
запрос
{
  "type": "set_settings",
  "args": {
    "login": "FooBaz",
    "email": "foo@baz.com"
  }
}
```
##### *get_settings* # получение значений для определённых полей
```json
запрос
{
  "type": "get_settings",
  "fields": ["login", "email"]
}

ответ
{
  "type": "settings",
  "values": {
    "login": "FooBaz",
    "email": "foo@baz.com"
  }
}
```
#### Мониторинг
##### *noop* # Пустой запрос (в надежде получить от демона очередь накопленных сообщений)
```json
{ "type": "noop" }
```
#### Действия
##### *action* # совершение какого-либо действия
```json
{
  "type": "action",
  "verb": "get_public_link",
  "args": { "file": "AK47/Верните сотку.mp3" }
}
```
#### Вызовы API
##### *api_call* # вызов метода [API](https://github.com/Xlab/lib2safe/blob/master/safecalls.h)
```json
{
  "type": "api_call",
  "call": "chk_mail",
  "args": { "email": "foo@baz.com" }
}
```

#### События изменений состояния
##### *event* # набор новых значений состояния
```json
пример1
{
  "type": "event",
  "category": "disk_quota",
  "values": {
    "used_bytes": "5400000",
    "total_bytes": "10240000"
  }
}

пример2
{
  "type": "event",
  "category": "file_progress",
  "values": {
    "id": "830",
    "bytes": "5120",
    "total_bytes": "102400"
  }
}

пример3
{
  "type": "event",
  "category": "action_result",
  "values": {
    "action": "get_public_link",
    "file": "AK47/Верните сотку.mp3",
    "result": "http://2safe.com/332fwfdss/sd3r3/sd3/md5.mp3"
  }
}

пример4
{
  "type": "event",
  "category": "api_result",
  "values": {
    "call": "chk_mail",
    "email": "foo@baz.com",
    "result": "true"
  }
}
```
