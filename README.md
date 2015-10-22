# [Funstream.tv](https://funstream.tv) API и утилиты для помощи с интеграцией.

## Текущая версия
### 0.0.6
###### [История изменений](CHANGELOG.md)

  
Общее
-----

Запрос посылается методом `POST`, если не указано другое, параметры запроса в JSON формате, **протокол HTTPS**.
Авторизация происходит через токен в `header`. Например
```
POST /user/current HTTP/1.1
Token: Bearer <your-token-here>
```

Успешный ответ приходит со статусом `200`.  
При ошибке ответ приходит со статусом `500`. Формат ответа ошибки
```js
{
    message: <string> // error message
}
```

Параметры, значение которых должно быть установлено в `null`, могут не передаваться.  
Где написано ``объект из ответа ...``, если не указано иного, подразумевает ответ указанного запроса без необязательных параметров/опций.

Версия API передается через `Accept`. Например,
```
GET /user/current HTTP/1.1
Accept: application/json; version=1.0
```
*В данный момент передавать версию не обязательно*

Запросы передаются на сайт [`https://funstream.tv`](https://funstream.tv) для общего API и на [`wss://funstream.tv:443/socket.io/?EIO=3&transport=websocket`](wss://funstream.tv:443/socket.io/?EIO=3&transport=websocket) для чата.

Примеры запросов на `curl`
```sh
curl -H "Content-Type: application/json" -H "Accept: application/json; version 1.0" -X POST -d '{name: "..", password: ".."}' https://funstream.tv/api/user/login
curl -H "Content-Type: application/json" -H "Accept: application/json; version 1.0" -H "Token: Bearer .." -X POST -d '{content: "stream"}' https://funstream.tv/api/subscribe/subscribers
```


API
------
###### Уровень доступа
- **`P`** Публичный, авторизация не обязательна
- **`A`** Все авторизованные пользователи
- **`M`** Авторизованные модераторы
- **`S`** Авторизованные саппорты
- **`C`** Закрытый, для внутреннего использования

1. [**OAuth**](oauth.md)  
    - [`POST` `A` `/api/oauth/app` Получить данные приложения](oauth.md#Получить-данные-приложения)
    - [`POST` `A` `/api/oauth/app/set` Сохранить данные приложения](oauth.md#Сохранить-данные-приложения)
    - [`POST` `P` `/api/oauth/check` Проверка статуса кода](oauth.md#Проверка-статуса-кода)
    - [`POST` `P` `/api/oauth/exchange` Получить токен по коду](oauth.md#Получить-токен-по-коду)
    - [`POST` `A` `/api/oauth/grant` Предоставить доступ по коду](oauth.md#Предоставить-доступ-по-коду)
    - [`POST` `P` `/api/oauth/request` Запросить код](oauth.md#Запросить-код)
2. [**Чат**](chat.md)
    - [**Протокол взаимодействия**](chat.md#Протокол-взаимодействия)
        - [Примеры взаимодействия для разных языков](chat-client-examples.md)
    - [**Оповещение сервера**](chat.md#Оповещение-сервера)
        - [`WS` `P` `/chat/login` Подписаться на события пользователя](chat.md#Подписаться-на-события-пользователя)
        - [`WS` `A` `/chat/logout` Отписаться от событий пользователя](chat.md#Отписаться-от-событий-пользователя)
        - [`WS` `P` `/chat/join` Присоединится к каналу](chat.md#Присоединится-к-каналу)
        - [`WS` `P` `/chat/leave` Покинуть канал](chat.md#Покинуть-канал)
        - [`WS` `P` `/chat/history` История канала](chat.md#История-канала)
        - [`WS` `A` `/chat/publish` Отправить сообщение](chat.md#Отправить-сообщение)
    - [**Оповещение клиента**](chat.md#Оповещение-клиента)
        - [`WS` `P` `/chat/message` Новое сообщение](chat.md#Новое-сообщение)
        - [`WS` `P` `/chat/message/remove` Удаление сообщения](chat.md#Удаление-сообщения)
        - [`WS` `P` `/chat/user/join` Присоединение к каналу](chat.md#Присоединение-к-каналу)
        - [`WS` `P` `/chat/user/leave` Отсоединение от канала](chat.md#Отсоединение-от-канала)
    - [**Каналы чата, текущие и запланированные**](chat.md#Каналы-чата-текущие-и-запланированные)
    - [**Типы сообщений**](chat.md#Типы-сообщений)
3. [**Общее**](common.md)
    - [**Пользователь**](common.md#Пользователь)
        - [`POST` `P` `/api/user` Найти пользователя](common.md#Найти-пользователя)
        - [`POST` `P` `/api/user/current` Данные текущего пользователя](common.md#Данные-текущего-пользователя)
        - [`POST` `P` `/api/user/login` Логин](common.md#Логин)
        - [`POST` `P` `/api/user/logout` Логаут](common.md#Логаут)
        - [`POST` `C` `/api/user/register` Регистрация](common.md#Регистрация)
        - [`POST` `A` `/api/user/settings` Получить или установить настройки текущего пользователя](common.md#Получить-или-установить-настройки-текущего-пользователя)
        - [`POST` `C` `/api/oauth/thirdparty/register` Регистрация через сторонние сервисы](common.md#Регистрация-через-сторонние-сервисы)
    - [**Категория**](common.md#Категория)
        - [`POST` `P` `/api/category` Категория контента](common.md#Категория-контента)
    - [**Стрим**](common.md#Стрим)
        - [`POST` `P` `/api/stream` Данные стрима](common.md#Данные-стрима)
    - [**Чат**](common.md#Чат)
        - [`POST` `P` `/api/channel/data` Данные каналов](common.md#Данные-каналов)
        - [`POST` `P` `/api/smile` Доступные смайлы канала](common.md#Доступные-смайлы-канала)
    - [**Фильтр**](common.md#Фильтр)
        - [`POST` `A/P` `/api/content` Список элементов контента](common.md#Список-элементов-контента)
        - [`POST` `P` `/api/content/top` Топ N элементов контента](common.md#Топ-n-элементов-контента)
    - [**Подписки**](common.md#Подписки)
        - [`POST` `A` `/api/subscribe/add` Подписаться](common.md#Подписаться)
        - [`POST` `A` `/api/subscribe/amount` Количество активных подписок онлайн](common.md#Количество-активных-подписок)
        - [`POST` `A` `/api/subscribe/check` Проверить подписку](common.md#Проверить-подписку)
        - [`POST` `A` `/api/subscribe/list` Список подписок](common.md#Список-подписок)
        - [`POST` `A` `/api/subscribe/remove` Отписаться](common.md#Отписаться)
        - [`POST` `A` `/api/subscribe/subscribers` Список подписчиков пользователя](common.md#Список-подписчиков-пользователя)
    - [**Игноры**](common.md#Игноры)
        - [`POST` `A` `/api/ignore/add` Добавить в список игнорируемых](common.md#Добавить-в-список-игнорируемых)
        - [`POST` `A` `/api/ignore/check` Проверить на игнор](common.md#Проверить-на-игнор)
        - [`POST` `A` `/api/ignore/list` Список игнорируемого](common.md#Список-игнорируемого)
        - [`POST` `A` `/api/ignore/remove` Удалить из списка игнорируемого](common.md#Удалить-из-списка-игнорируемого)
    - [**Дополнительные вызовы**](common.md#Дополнительные-вызовы)
        - [`POST` `P` `/api/bulk` Пакетный запрос](common.md#Пакетный-запрос)
        - [`POST` `P` `/api/support?name=<streamer_name>` Получить список последних поддержавших для данного стримера за 5 минут](common.md#Получить-список-последних-поддержавших-для-данного-стримера-за-5-минут)
4. [**Админка**](admin.md)
    - [**Модерация**](admin.md#Модерация)
        - [`POST` `A` `/api/moderation/accuse` Забанить пользователя](admin.md#Забанить-пользователя)
        - [`POST` `P` `/api/moderation/check` Проверить забанен ли пользователь](admin.md#Проверить-забанен-ли-пользователь)
        - [`POST` `M` `/api/moderation/list` Получить список банов](admin.md#Получить-список-банов)
        - [`POST` `P` `/api/moderation/reasons` Получить список причин бана](admin.md#Получить-список-причин-бана)
        - [`POST` `M` `/api/moderation/undo` Отменить бан](admin.md#Отменить-бан)
    - [**Поддержка**](admin.md#Поддержка)
        - [`POST` `A` `/api/support/ask` Задать вопрос](admin.md#Задать-вопрос)
        - [`POST` `A` `/api/support/ban` Отключить возможность задавать вопросы](admin.md#Отключить-возможность-задавать-вопросы)
        - [`POST` `A` `/api/support/channels` Список последних вопросов](admin.md#Список-последних-вопросов)
        - [`POST` `S` `/api/support/list` Получить список вопросов](admin.md#Получить-список-вопросов)
5. [**Уведомления**](notifier.md)
    - [**Событие уведомления**](notifier.md#Событие-уведомления)
    - [**Типы уведомлений**](notifier.md#Типы-уведомлений)
        - [`thirdpartyLogin`](notifier.md#Логин-через-сторонние-сервисы)
        - [`thirdpartyRegister`](notifier.md#Регистрация-через-сторонние-сервисы)
6. [**Sc2tv**](sc2tv.md)
    - [**Авторизация**](sc2tv.md#Авторизация)
        - [`POST` `A` `/api/sc2tv/authUrl` Получить урл авторизации](sc2tv.md#Получить-урл-авторизации)
        - [`POST` `A` `/api/sc2tv/check` Проверить права](sc2tv.md#Проверить-права)
    - [**Донат**](sc2tv.md#Донат)
        - [`POST` `A` `/api/sc2tv/donate` Донат произвольной суммы](sc2tv.md#Донат-произвольной-суммы)
        - [`POST` `A` `/api/sc2tv/donate/fast` Микродонат](sc2tv.md#Микродонат)


Спасибо @JAremko за оформление документации