API взаимодействия со сторонними сервисами
----------------------------------
- [`POST` `A` `/api/oauth/thirdparty/goodgame` Сохранение данных авторизации для GoodGame](#Сохранение-данных-авторизации-для-goodgame)
- [`POST` `C` `/api/oauth/thirdparty/register` Регистрация через сторонние сервисы](#Регистрация-через-сторонние-сервисы)


### Описание
Методы взаимодействия со сторонними сервисами - Twitch, GoodGame, VK, Google


#### Сохранение данных авторизации для GoodGame
##### [`POST` `A` `/api/oauth/thirdparty/goodgame`](https://funstream.tv/api/oauth/thirdparty/goodgame)
**запрос**
```js
{
    name: <string>, // Имя пользователя на GoodGame
    password: <string> // Пароль пользователя на GoodGame
}
```
**ответ**
```js
{}
```
*Сохраняет логин/пароль GoodGame'а пользователя для возможности писать в чат GG.*  
*Доступ через логин/пароль единственный способ авторизации сторонних сервисов на GoodGame. Пароль хранится в бд Funstream в зашифрованном виде.*  
*Вернёт ошибку если любой из параметров не передан или пустой, пользователь не залогинен или передан неверный токен.*



#### Регистрация через сторонние сервисы
##### [`POST` `C` `/api/oauth/thirdparty/register`](https://funstream.tv/api/oauth/thirdparty/register)
**запрос**
```js
{
    name: <string>, // Имя пользователя
    token: <string> // Токен авторизации, полученный соответствующим уведомлением после авторизации пользователя на стороннем сервисе
}
```
**ответ**
```js
{
    token: <string>, // Токен пользователя
    current: <obj> // Данные пользователя, объект из ответа /api/user/current
}
```
*Работает только на funstream.tv.*  
*Позволяет регистрироваться через Twitch/VK/Google.*  
*Вернёт ошибку если токен авторизации не верен, пользователь с таким именем уже существует или имя пользователя недопустимо.*