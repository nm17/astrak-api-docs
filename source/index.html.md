---
title: Astrak API v1.0
language_tabs:
  - "python": Python
  - "js": Javascript
toc_footers: 
  - <a href="https://teleg.run/astrakme">(!) Мы в телеграме</a>
includes: []
search: true
highlight_theme: darkula
headingLevel: 2

---

<h1 id="astrak-api">Astrak API v1.0</h1>

> Scroll down for code samples, example requests and responses. Select a language for code samples from the tabs above or the mobile navigation menu.

Обычный мессенджер. 

Base URLs:

* <a href="http://afternoon-dusk-97603.herokuapp.com/">http://afternoon-dusk-97603.herokuapp.com/</a>

Релизы астрак: https://github.com/catofsof/astrak

## Указание Callback для получения событий

<a id="opIdpost-events-callback-start"></a>

`POST /events/callback/start`

> Пример ответа

```json
{
  "type": "new_message",
  "event": {
    "message_id": 12,
    "to_id": 1,
    "from_id": 2,
    "created_at": 1581110027,
    "text": "this is a new message!"
  }
}
```

Используем URL в формате `https://ip:port/` для вашего же удобства. Вам, конечно, никто не запрещает просто написать туда произвольный текст. Но вы сами прекрасно понимаете, что это совсем не имеет смысла.

Если все успешно, то мы получаем JSON с полем code: 10.

Поднимаем веб-сервер.

На ваш ранее указанный URL будут приходить события в POST формате. Вот пример одного из событий:



При смене URL Callback сервера приходит событие 'callback_deprecated', в котором указаны старый и новый серверы.

> Body parameter

```json
{
  "token": "string",
  "callback": "string"
}
```

<h3 id="указание-callback-для-получения-событий-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|token|body|string|true|Токен пользователя|
|callback|body|string|true|URL, на который будут приходить события|

## Получение событий с помощью Long Polling

<a id="opIdpost-events-polling"></a>

`POST /events/polling`

> Пример бота

```python
from astrak import Astrak, Dispatcher, TaskManager
from astrak.types import Message
import logging

logging.basicConfig(level=logging.DEBUG)

astrak = Astrak(username="username", password="password")
dp = Dispatcher(astrak)


@dp.message_handler(text="привет")
async def text_handler(message: Message):
    await message.answer("И тебе привет!")


@dp.message_handler(text_contains="шуе")
async def text_handler(message: Message):
    await message.answer("ППШ")


task_manager = TaskManager(astrak.loop)
task_manager.add_task(dp.dispatch_forever())
task_manager.run()
```

Ждем ответ сервера. Как только произойдет какое-либо событие, сервер сразу вернет вам ответ.
События приходят одинаковыми как для Callback, так и для Long Polling.

> Body parameter

```json
{
  "token": "string"
}
```

<h3 id="получение-событий-с-помощью-long-polling-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|token|body|string|false|Токен пользователя|

## Зарегистрироваться

<a id="opIdpost-users-register"></a>

`POST /users/register`

Запрос выполняет регистрацию пользователя и возращает актуальный токен.

Пароль не может быть короче 8 символов.

Имя пользователя не может быть длиннее 15 символов.

Если в одном из запросов был введён некорректный номер телефона, то необходимо сделать повторый запрос с тем же логином и паролем, но с другим номером телефона. Придёт сообщение с новым кодом подтвержения. Дальше, как обычно, необходимо подтвердить аккаунт методом /confirm.

> Body parameter

```json
{
  "password": "string",
  "username": "string",
  "phone": "string"
}
```

<h3 id="зарегистрироваться-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|password|body|string|true|Пароль пользователя|
|username|body|string|true|Имя пользователя|
|phone|body|string|true|Номер телефона|

## Зайти в систему

<a id="opIdpost-users-login"></a>

`POST /users/login`

Возращает токен пользователя по логину и паролю.

> Body parameter

```json
{
  "password": "string",
  "username": "string"
}
```

<h3 id="зайти-в-систему-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|password|body|string|true|none|
|username|body|string|true|none|

## Подтвердить аккаунт по смс

<a id="opIdpost-users-confirm"></a>

`POST /users/confirm`

Подтверждает аккаунт по смс сообщению.

> Body parameter

```json
{
  "code": "string",
  "phone": "string"
}
```

<h3 id="подтвердить-аккаунт-по-смс-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|code|body|string|true|none|
|phone|body|string|true|none|

## Проверить токен на валидность

<a id="opIdget-users-check"></a>

`GET /users/check`

Запрос проверяет токен на валидность.

## Отправить сообщение пользователю

<a id="opIdpost-messages-send"></a>

`POST /messages/send`

Отправляет сообщение пользователю.

> Body parameter

```json
{
  "token": "string",
  "text": "string",
  "to": "string"
}
```

<h3 id="отправить-сообщение-пользователю-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|token|body|string|true|none|
|text|body|string|true|none|
|to|body|string|true|none|

## Получить список диалогов

<a id="opIdget-messages-dialogs"></a>

`GET /messages/dialogs`

> Пример кода

```python
from astrak import Astrak
import asyncio

astrak_api = Astrak(username="username", password="password")


async def main():
    me = await astrak_api.get_me()
    print(me.id)

    dialogs = await astrak_api.get_all_dialogs()
    print(dialogs)

    await astrak_api.session.close()


loop = asyncio.get_event_loop()
loop.run_until_complete(main())
```

Возращает массив со всеми диалогами пользователя.

> Body parameter

```json
{
  "token": "string"
}
```

<h3 id="получить-список-диалогов-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|token|body|string|true|none|

## Получить сообщения из диалога

<a id="opIdget-messages-dialog"></a>

`GET /messages/dialog`

Возращает массив сообщений с конкретным пользователем.

> Body parameter

```json
{
  "token": "string",
  "id": "string"
}
```

<h3 id="получить-сообщения-из-диалога-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|token|body|string|true|none|
|id|body|string|true|none|

## Удалить сообщение

<a id="opIdpost-messages-delete"></a>

`POST /messages/delete`

Удаляет сообщение по его ID.

> Body parameter

```json
{
  "token": "string",
  "message_id": "string"
}
```

<h3 id="удалить-сообщение-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|token|body|string|true|none|
|message_id|body|string|true|ID сообщения. Обычно приходит в событиях|
