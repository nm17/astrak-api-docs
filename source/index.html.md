---
title: Astrak API v1.0
language_tabs:
  - "'js": Javascript'
  - "'python": Python'
language_clients:
  - "'js": ""
  - "'python": ""
toc_footers: []
includes: []
search: true
highlight_theme: darkula
headingLevel: 2

---

<!-- Generator: Widdershins v4.0.1 -->

<h1 id="astrak-api">Astrak API v1.0</h1>

> Scroll down for code samples, example requests and responses. Select a language for code samples from the tabs above or the mobile navigation menu.

Обычный мессенджер. 

Base URLs:

* <a href="http://afternoon-dusk-97603.herokuapp.com/">http://afternoon-dusk-97603.herokuapp.com/</a>

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

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<password>string</password>
<username>string</username>
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

|Status|Meaning|Description|Schema|
|---|---|---|---|

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

|Status|Meaning|Description|Schema|
|---|---|---|---|

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

|Status|Meaning|Description|Schema|
|---|---|---|---|

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
