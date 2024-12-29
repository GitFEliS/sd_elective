# Задача
DDD. Для некого выдуманного или реального домена с проблемой описать
1. Область проблемы (subdomains)
    а. Объяснить почему тот или иной поддомен относится к тому или иному типу
1. Область решения (bounded context) и их связь с областью проблемы
1. Concept map (Bounded Contexts + Integration Patterns)
    а. Для каждой интеграции объяснить почему выбран именно этот паттерн

Сделаем DDD для сервиса - аналога Discord.

# subdomains

core:
- Voice chatting
    - Core потому что, непосредственно относится к главной фиче продукта
- Text messaging
    - Core потому что, непосредственно относится к главной фиче продукта
- Server and channel management
    - Core потому что, юзерам нужно находить способы коммуникации между собой, и то насколько
    пользователям легко найти друг друга и организовать коммуникацию относится к главной фичи продукта

supporting:
- Notifications
    - supporting потому что, не приносят ключевой ценности, весь кор функционал в полной мере работает и без уведомлений,
    но достаточно важно для quality of life пользователя, мало кому захочется пользоваться текстовым чатом, если они не будут
    знать о новых сообщениях

generic:
- User Management
    - generic, потому что не уникально для домена

# bounded context

1. Voice chatting context
    - Инфра и по обеспечивающие голосовую коммуникацию
1. Text chatting context
    - Инфра и по обеспечивающие текстовую коммуникацию
1. User servers meta context
    - Инфра и по обеспечивающие CRUD операции по пользовательски серверам
1. Notifications context
    - Инфра и по обеспечивающие уведомления на стороне пользователя
1. IAM service
    - Инфра и по обеспечивающие авторизацию и аутентификацию
1. User meta context
    - "Профиль" пользователя

## Связь bounded context и subdomains

Voice chatting -> Подключиться к "комнате" голосового общения ->  Voice chatting context
Voice chatting -> Загрузить мета информацию о "комнате", если она относится к серверу -> User servers meta context
Text messaging -> Подключиться к "комнате" текстового общения ->  Text chatting context
Text chatting -> Загрузить мета информацию о "комнате", если она относится к серверу -> User servers meta context
Server and channel management -> Найти сервер по запросу -> User servers meta context
Notifications -> Отправить уведомление пользователю -> Notifications context
User Management -> Аутенитифицировать и авторизовать пользователя -> IAM service
Text chatting -> Получить мета информацию о отправителе и получателе ->  User meta context

# Concept map

Voice chatting context <- Shared Kernel <- User servers meta context
Text chatting context <- Shared Kernel <- User servers meta context

Потому что Voice и Text chatting плотно интегрированы в User servers, и работают с общей информацией
но выполняют глобально разные задачи

Text chatting context <- Customer-Supplier <- User meta context 

Потому что, User meta context никак не зависит от Text chatting context, но text chatting зависит, и при добавление
фич в профиль пользователя Text chatting о них уведомляется и подстраивается
