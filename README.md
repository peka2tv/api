# [Funstream.tv](http://funstream.tv) API

## Текущая версия
### 0.0.16
### [История изменений](CHANGELOG.md)


## Общее

Запрос посылается методом `POST`, если не указано другое, параметры запроса в JSON формате, **протокол HTTP**.  
Авторизация происходит через токен в `Header`. Например
```
POST /user/current HTTP/1.1
Token: Bearer your-token-here
```

Успешный ответ приходит со статусом `200`.  
При ошибке ответ приходит со статусом `500`. Формат ответа ошибки
```ts
{
    message: string; // error message
}
```

Для описания параметров используется формат TypeScript интерфейсов и типов.  
Параметры отмеченные `?` являются необязательными.  
Где написано ``объект из ответа ...``, если не указано иного, подразумевает ответ указанного запроса без необязательных параметров/опций.

Версия API передается через `Accept`. Например,
```
POST /user/current HTTP/1.1
Accept: application/json; version=1.0
```
*В данный момент передавать версию не обязательно*

Запросы передаются на сайт [`http://funstream.tv`](http://funstream.tv) для общего API и на [`wss://chat.funstream.tv`](wss://chat.funstream.tv) для чата.

Примеры запросов на `curl`
```sh
curl -H "Content-Type: application/json" -H "Accept: application/json; version 1.0" -X POST http://funstream.tv/api/user/current
curl -H "Content-Type: application/json" -H "Accept: application/json; version 1.0" -H "Token: Bearer .." -X POST -d '{content: "stream"}' http://funstream.tv/api/subscribe/subscribers
```


##### В случае вопросов, ошибок или неточностей документации, пишите в Помощь на сайтах [funstream.tv](http://funstream.tv/stream/all/top) или [sc2tv.ru](http://sc2tv.ru) (необходимо залогиниться), категория 'Технические вопросы'.


# API

#### Права доступа
- **`P`** Публичный, авторизация не обязательна
- **`A`** Все авторизованные пользователи
- **`M`** Модераторы, роль `moderator`
- **`S`** Саппорты, роль `support`
- **`B`** Администратор блокировок, роль `blocker`
- **`Sm`** Администратор смайлов, роль `smiler`
- **`MS`** Мастерстримеры, роль `masterstreamer`
- **`RA`** Администратор ролей, роль `roleAdmin`
- **`Sc`** Изменение расписания, роль `schedule`
- **`Se`** Управление сезонами соревнования, роль `seasoner`
- **`C`** Закрытый, для внутреннего использования

---


- [**OAuth**](oauth.md)
    - [`POST` `A` `/api/oauth/app` Получить данные приложения](oauth.md#Получить-данные-приложения)
    - [`POST` `A` `/api/oauth/app/set` Сохранить данные приложения](oauth.md#Сохранить-данные-приложения)
    - [`POST` `P` `/api/oauth/check` Проверка статуса кода](oauth.md#Проверка-статуса-кода)
    - [`POST` `P` `/api/oauth/exchange` Получить токен по коду](oauth.md#Получить-токен-по-коду)
    - [`POST` `A` `/api/oauth/grant` Предоставить доступ по коду](oauth.md#Предоставить-доступ-по-коду)
    - [`POST` `P` `/api/oauth/request` Запросить код](oauth.md#Запросить-код)
- [**Чат**](chat.md)
    - [**Протокол взаимодействия**](chat.md#Протокол-взаимодействия)
        - [Примеры взаимодействия для разных языков](chat-client-examples.md)
    - [**Оповещение сервера**](chat.md#Оповещение-сервера)
        - [`WS` `P` `/chat/login` Подписаться на события пользователя](chat.md#Подписаться-на-события-пользователя)
        - [`WS` `A` `/chat/logout` Отписаться от событий пользователя](chat.md#Отписаться-от-событий-пользователя)
        - [`WS` `P` `/chat/join` Присоединится к каналу](chat.md#Присоединится-к-каналу)
        - [`WS` `P` `/chat/leave` Покинуть канал](chat.md#Покинуть-канал)
        - [`WS` `P` `/chat/history` История канала](chat.md#История-канала)
        - [`WS` `A` `/chat/publish` Отправить сообщение](chat.md#Отправить-сообщение)
        - [`WS` `P` `/chat/channel/list` Список пользователей в канале](chat.md#Список-пользователей-в-канале)
    - [**Оповещение клиента**](chat.md#Оповещение-клиента)
        - [`WS` `P` `/chat/message` Новое сообщение](chat.md#Новое-сообщение)
        - [`WS` `P` `/chat/message/remove` Удаление сообщения](chat.md#Удаление-сообщения)
        - [`WS` `P` `/chat/user/join` Присоединение к каналу](chat.md#Присоединение-к-каналу)
        - [`WS` `P` `/chat/user/leave` Отсоединение от канала](chat.md#Отсоединение-от-канала)
    - [**Каналы чата**](chat.md#Каналы-чата)
    - [**Типы сообщений**](chat.md#Типы-сообщений)
- [**Общее**](common.md)
    - [**Пользователь**](common.md#Пользователь)
        - [`POST` `P` `/api/user` Данные пользователя](common.md#Данные-пользователя)
        - [`POST` `P` `/api/user/list` Данные списка пользователей](common.md#Данные-списка-пользователей)
        - [`POST` `P` `/api/user/current` Данные текущего пользователя](common.md#Данные-текущего-пользователя)
        - [`POST` `RA` `/api/user/full` Полные данные пользователя](common.md#Полные-данные-пользователя)
        - [`POST` `C` `/api/user/login` Логин](common.md#Логин)
        - [`POST` `P` `/api/user/login/bytoken` Логин по токену](common.md#Логин-по-токену)
        - [`POST` `P` `/api/user/logout` Логаут](common.md#Логаут)
        - [`POST` `A` `/api/user/settings` Получить или установить настройки текущего пользователя](common.md#Получить-или-установить-настройки-текущего-пользователя)
        - [`POST` `P` `/api/user/forgot` Запрос на сброс пароля](common.md#Запрос-на-сброс-пароля)
        - [`POST` `P` `/api/user/restore` Установка пароля](common.md#Установка-пароля)
        - [`POST` `RA` `/api/user/roles/list` Список пользователей с ролью](common.md#Список-пользователей-с-ролью)
        - [`POST` `RA` `/api/user/roles/set` Изменить роль пользователя](common.md#Изменить-роль-пользователя)
    - [**Категория**](common.md#Категория)
        - [`POST` `P` `/api/category` Категория контента](common.md#Категория-контента)
    - [**Стрим**](common.md#Стрим)
        - [`POST` `P` `/api/stream` Данные стрима](common.md#Данные-стрима)
        - [`POST` `P` `/api/stream/preview` Ссылка на видео превью стрима](common.md#Ссылка-на-видео-превью-стрима)
    - [**Чат**](common.md#Чат)
        - [`POST` `P` `/api/channel/data` Данные каналов](common.md#Данные-каналов)
        - [`POST` `P` `/api/channel/private` Список приват каналов](common.md#Список-приват-каналов)
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
    - [**Мастерстримеры**](common.md#Мастерстримеры)
        - [`POST` `P` `/api/masterstreamer/list` Список иконок и смайлов мастерстримеров](common.md#Список-иконок-и-смайлов-мастерстримеров)
        - [`POST` `P` `/api/masterstreamer/icon/list` Список выбранных пользователями иконок](common.md#Список-выбранных-пользователями-иконок)
    - [**Расписание**](common.md#Расписание)
        - [`POST` `P` `/api/schedule/get` Список расписания](common.md#Список-расписания)
        - [`POST` `Sc` `/api/schedule/my` Расписание пользователя](common.md#Расписание-пользователя)
        - [`POST` `Sc` `/api/schedule/set` Изменить расписание пользователя](common.md#Изменить-расписание-пользователя)
    - [**Майлстоуны**](common.md#Майлстоуны)
        - [`POST` `P` `/api/milestone/get` Список майлстоунов](common.md#Список-майлстоунов)
        - [`POST` `A` `/api/milestone/my` Майлстоуны пользователя](common.md#Майлстоуны-пользователя)
        - [`POST` `A` `/api/milestone/set` Изменить майлстоуны пользователя](common.md#Изменить-майлстоуны-пользователя)
    - [**Сезоны соревнования**](common.md#Сезоны-соревнования)
        - [`POST` `P` `/api/season/get` Данные текущего сезона](common.md#Данные-текущего-сезона)
        - [`POST` `Se` `/api/season/start` Старт сезона](common.md#Старт-сезона)
        - [`POST` `Se` `/api/season/stop` Завершение сезона](common.md#Завершение-сезона)
        - [`POST` `P` `/api/season/top` Топ сезона](common.md#Топ-сезона)
    - [**Уровни**](common.md#Уровни)
        - [`POST` `A` `/api/level/my` Мои уровни](common.md#Мои-уровни)
        - [`POST` `A` `/api/level/premiumUsers` Уровни моих премиум юзеров](common.md#Уровни-моих-премиум-юзеров)
    - [**Аукцион окончания стрима**](common.md#Аукцион-окончания-стрима)
        - [`POST` `P` `/api/prolonger/active` Активные аукционы окончания стрима](common.md#Активные-аукционы-окончания-стрима)
        - [`POST` `P` `/api/prolonger/get` Данные аукциона окончания стрима](common.md#Данные-аукциона-окончания-стрима)
        - [`POST` `A` `/api/prolonger/start` Запустить аукцион окончания стрима](common.md#Запустить-аукцион-окончания-стрима)
    - [**Дополнительные вызовы**](common.md#Дополнительные-вызовы)
        - [`POST` `P` `/api/bulk` Пакетный запрос](common.md#Пакетный-запрос)
- [**Смайлы**](smile.md)
    - [**Смайлы**](smile.md#Смайлы)
        - [`POST` `P` `/api/smile` Доступные смайлы](smile.md#Доступные-смайлы)
        - [`POST` `MS/Sm` `/api/smile/add` Добавить смайл](smile.md#Добавить-смайл)
        - [`POST` `Sm` `/api/smile/approve` Утвердить изменения в смайлах мастерстримера](smile.md#Утвердить-изменения-в-смайлах-мастерстримера)
        - [`POST` `Sm` `/api/smile/pending` Список изменений в смайлах мастерстримеров на утверждение](smile.md#Список-изменений-в-смайлах-мастерстримеров-на-утверждение)
        - [`POST` `Sm` `/api/smile/reject` Отклонить изменения в смайлах мастерстримера](smile.md#Отклонить-изменения-в-смайлах-мастерстримера)
        - [`POST` `MS/Sm` `/api/smile/remove` Удалить смайлы](smile.md#Удалить-смайлы)
        - [`POST` `MS/Sm` `/api/smile/update` Обновить смайлы](smile.md#Обновить-смайлы)
        - [`POST` `MS` `/api/smile/ms/pending` Наличие смайлов в ожидании утверждения](smile.md#Наличие-смайлов-в-ожидании-утверждения)
        - [`POST` `MS` `/api/smile/ms/prepare` Отправить текущие изменения в смайлах на утверждение](smile.md#Отправить-текущие-изменения-в-смайлах-на-утверждение)
        - [`POST` `MS` `/api/smile/ms/revert` Откатить изменения в смайлах](smile.md#Откатить-изменения-в-смайлах)
    - [**Иконки**](smile.md#Иконки)
        - [`POST` `Ms` `/api/icon/add` Добавление иконки](smile.md#Добавление-иконки)
        - [`POST` `P` `/api/icon/list` Список иконок](smile.md#Список-иконок)
        - [`POST` `Ms` `/api/icon/remove` Удаление иконок](smile.md#Удаление-иконок)
- [**Комнаты**](room.md)
    - [**Основное**](room.md#Основное)
        - [`POST` `P` `/api/room` Данные комнаты](room.md#Данные-комнаты)
        - [`POST` `A` `/api/room/create` Создать комнату](room.md#Создать-комнату)
        - [`POST` `P` `/api/room/my` Список комнат пользователя](room.md#Список-комнат-пользователя)
        - [`POST` `A` `/api/room/preview` Изменить превью комнаты](room.md#Изменить-превью-комнаты)
        - [`POST` `A` `/api/room/settings` Настройки комнаты](room.md#Настройки-комнаты)
    - [**Видео**](room.md#Видео)
        - [`POST` `P/A` `/api/room/getCurrentVideo` Текущая трансляция комнаты](room.md#Текущая-трансляция-комнаты)
        - [`POST` `A` `/api/room/periscopeRandom` Ссылка на случайный стрим с перископа](room.md#Ссылка-на-случайный-стрим-с-перископа)
        - [`POST` `A` `/api/room/setCurrentVideo` Изменить текущую трансляцию комнаты](room.md#Изменить-текущую-трансляцию-комнаты)
    - [**Плейлист**](room.md#Плейлист)
        - [`POST` `P/A` `/api/room/playlist` Плейлист комнаты](room.md#Плейлист-комнаты)
        - [`POST` `P/A` `/api/room/playlist/history` История воспроизведения комнаты](room.md#История-воспроизведения-комнаты)
        - [`POST` `A` `/api/room/playlist/set` Изменить плейлист комнаты](room.md#Изменить-плейлист-комнаты)
    - [**Пользователи**](room.md#Пользователи)
        - [`POST` `P` `/api/room/user/admin` Статус администратора комнаты](room.md#Статус-администратора-комнаты)
        - [`POST` `A` `/api/room/user/list` Список имеющих доступ в комнату](room.md#Список-имеющих-доступ-в-комнату)
        - [`POST` `A` `/api/room/user/remove` Лишить пользователя доступа в комнату](room.md#Лишить-пользователя-доступа-в-комнату)
        - [`POST` `A` `/api/room/user/set` Изменить права доступа пользователя в комнату](room.md#Изменить-права-доступа-пользователя-в-комнату)
    - [**Инвайты**](room.md#Инвайты)
        - [`POST` `A` `/api/room/invite` Запросить доступ в комнату](room.md#Запросить-доступ-в-комнату)
        - [`POST` `A` `/api/room/invite/accept` Принять запрос](room.md#Принять-запрос)
        - [`POST` `A` `/api/room/invite/blacklist` Переместить запрос в чёрный список](room.md#Переместить-запрос-в-чёрный-список)
        - [`POST` `A` `/api/room/invite/list` Список запросов](room.md#Список-запросов)
        - [`POST` `A` `/api/room/invite/reject` Отклонить запрос](room.md#Отклонить-запрос)
        - [`POST` `A` `/api/room/invite/status` Статус запроса](room.md#Статус-запроса)
    - [**Уведомления**](room.md#Уведомления-1)
        - [`roomModify` Данные комнаты изменены](room.md#Данные-комнаты-изменены)
        - [`roomVideo` Новая трансляция в комнате](room.md#Новая-трансляция-в-комнате)
        - [`roomUserRemoved` Пользователь лишён доступа в комнату](room.md#Пользователь-лишён-доступа-в-комнату)
        - [`roomUserSet` Изменены права доступа пользователя в комнату](room.md#Изменены-права-доступа-пользователя-в-комнату)
- [**Голосование**](poll.md)
    - [`POST` `P` `/api/poll/info` Данные голосования](poll.md#Данные-голосования)
    - [`POST` `A` `/api/poll/start` Начать голосование](poll.md#Начать-голосование)
    - [`POST` `A` `/api/poll/vote` Проголосовать](poll.md#Проголосовать)
    - [**Уведомления**](poll.md#Уведомления-1)
        - [`poll/update` Обновлённые данные](poll.md#Обновлённые-данные)
- [**Плеер**](player.md)
    - [`GET` `P` `/api/player/live` Онлайн статус трансляций](player.md#Онлайн-статус-трансляций)
- [**Биллинг**](billing/README.md)
    - [**Донат**](billing/donate.md)
        - [**Информация о последних событиях**](billing/donate.md#Информация-о-последних-событиях)
            - [`GET` `P` `/billing/donate/challenge` Челенджи](billing/donate.md#Челенджи)
            - [`GET` `P` `/billing/donate/` Донат, антидонат, микродонат](billing/donate.md#Донат-антидонат-микродонат)
- [**Админка**](admin.md)
    - [**Модерация**](admin.md#Модерация)
        - [`POST` `A` `/api/moderation/accuse` Забанить пользователя](admin.md#Забанить-пользователя)
        - [`POST` `B` `/api/moderation/block` Блокировка пользователя](admin.md#Блокировка-пользователя)
        - [`POST` `P` `/api/moderation/check` Проверить забанен ли пользователь](admin.md#Проверить-забанен-ли-пользователь)
        - [`POST` `A` `/api/moderation/list` Получить список банов](admin.md#Получить-список-банов)
        - [`POST` `P` `/api/moderation/reasons` Получить список причин бана](admin.md#Получить-список-причин-бана)
        - [`POST` `M` `/api/moderation/undo` Отменить бан](admin.md#Отменить-бан)
    - [**Поддержка**](admin.md#Поддержка)
        - [`POST` `A` `/api/support/ask` Задать вопрос](admin.md#Задать-вопрос)
        - [`POST` `A` `/api/support/ban` Отключить возможность задавать вопросы](admin.md#Отключить-возможность-задавать-вопросы)
        - [`POST` `A` `/api/support/channels` Список последних вопросов](admin.md#Список-последних-вопросов)
        - [`POST` `S` `/api/support/list` Получить список вопросов](admin.md#Получить-список-вопросов)
    - [**Безопасность**](admin.md#Безопасность)
        - [`POST` `A` `/api/security/user` Список последних логинов](admin.md#Список-последних-логинов)
    - [**Уведомления**](admin.md#Уведомления-1)
        - [`chatBan` Бан в чате](admin.md#Бан-в-чате)
        - [`chatBanAttempt` Отметка о нарушении в чате](admin.md#Отметка-о-нарушении-в-чате)
        - [`chatBanUndo` Отмена бана в чате](admin.md#Отмена-бана-в-чате)
- [**Уведомления**](notification.md)
    - [**Событие уведомления**](notification.md#Событие-уведомления)
    - [**Прочие уведомления**](notification.md#Прочие-уведомления)
        - [**Донат в чате**](notification.md#Донат-в-чате)
            - [`chatChannelDonateLevel` Изменение донат уровня канала чата](notification.md#Изменение-донат-уровня-канала-чата)
        - [**Сторонние сервисы в чате**](notification.md#Сторонние-сервисы-в-чате)
            - [`chatThirdpartySendMessageError` Ошибка отправки сообщения в чат сервис](notification.md#Ошибка-отправки-сообщения-в-чат-сервис)
- [**Сторонние сервисы**](thirdparty.md)
    - [`POST` `A` `/api/oauth/thirdparty/goodgame` Сохранение данных авторизации для GoodGame](thirdparty.md#Сохранение-данных-авторизации-для-goodgame)
    - [`POST` `C` `/api/oauth/thirdparty/register` Регистрация через сторонние сервисы](thirdparty.md#Регистрация-через-сторонние-сервисы)
    - [**Уведомления**](thirdparty.md#Уведомления-1)
        - [`thirdpartyLogin` Логин через сторонние сервисы](thirdparty.md#Логин-через-сторонние-сервисы)
        - [`thirdpartyRegister` Регистрация через сторонние сервисы](thirdparty.md#Регистрация-через-сторонние-сервисы-1)

- [**Sc2tv**](sc2tv.md)
    - [**Авторизация**](sc2tv.md#Авторизация)
        - [`POST` `A` `/api/sc2tv/authUrl` Получить урл авторизации](sc2tv.md#Получить-урл-авторизации)
        - [`POST` `A` `/api/sc2tv/check` Проверить права](sc2tv.md#Проверить-права)
    - [**Донат**](sc2tv.md#Донат)
        - [`POST` `A` `/api/sc2tv/donate` Донат произвольной суммы](sc2tv.md#Донат-произвольной-суммы)
        - [`POST` `A` `/api/sc2tv/donate/fast` Микродонат](sc2tv.md#Микродонат)


Спасибо @JAremko за помощь в оформлении документации.
