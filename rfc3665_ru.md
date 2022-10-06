# Основные примеры потока вызовов Session Initiation Protocol (SIP)

>
> Network Working Group                                        A. Johnston
> Request for Comments: 3665                                           MCI
> BCP: 75                                                       S. Donovan
> Category: Best Current Practice                                R. Sparks
>                                                            C. Cunningham
>                                                              dynamicsoft
>                                                               K. Summers
>                                                                    Sonus
>                                                            December 2003
>

#### Статус документа

Этот документ определяет лучшие текущие практики интернета для интернет-сообщества и запрашивает обсуждение и предложения по улучшению. Распространение данной памятки не ограничено.

#### Уведомление об авторских правах

Copyright (C) The Internet Society (2003). Все права защищены.

#### Абстрактно

В этом документе приведены примеры потоков вызовов протокола установления сеанса (SIP). Элементы в этих потоках вызовов включают SIP-агенты пользователей и клиентов, SIP-прокси и серверы перенаправления. Сценарии включают регистрацию SIP и создание сеанса SIP. Показаны схемы потоков вызовов и сведения о сообщениях.

## Содержание

- [Основные примеры потока вызовов Session Initiation Protocol (SIP)](#основные-примеры-потока-вызовов-session-initiation-protocol-sip)
	- [1. Обзор](#1-обзор)
		- [1.1 Главный постулат](#11-главный-постулат)
		- [1.2. Условные обозначения для потоков сообщений](#12-условные-обозначения-для-потоков-сообщений)
		- [1.3 Постулат протокола SIP](#13-постулат-протокола-sip)
	- [2. SIP-регистрация](#2-sip-регистрация)
		- [2.1 Успешная новая регистрация](#21-успешная-новая-регистрация)
		- [2.2. Обновление списка контактов](#22-обновление-списка-контактов)
		- [2.3. Запрос текущего списка контактов](#23-запрос-текущего-списка-контактов)
		- [2.4. Отмена регистрации](#24-отмена-регистрации)
		- [2.5. Неудачная регистрация](#25-неудачная-регистрация)
	- [3. Установка сеанса SIP](#3-установка-сеанса-sip)
		- [3.1. Успешная установка сеанса](#31-успешная-установка-сеанса)
		- [3.2. Установка сеанса через два прокси](#32-установка-сеанса-через-два-прокси)
		- [3.3. Сеанс с множественной прокси-аутентификацией](#33-сеанс-с-множественной-прокси-аутентификацией)
		- [3.4. Успешный сеанс с отказом прокси-сервера](#34-успешный-сеанс-с-отказом-прокси-сервера)
		- [3.5. Сеанс через SIP ALG](#35-сеанс-через-sip-alg)
		- [3.6. Сеанс через редирект и прокси-серверы с SDP в ACK](#36-сеанс-через-редирект-и-прокси-серверы-с-sdp-в-ack)
		- [3.7. Сеанс с re-INVITE (Смена IP-адреса)](#37-сеанс-с-re-invite-смена-ip-адреса)
		- [3.8. Неудачно - нет ответа](#38-неудачно---нет-ответа)
		- [3.9. Неудачно - занято](#39-неудачно---занято)
		- [3.10. Неудачно - нет ответа от User Agent](#310-неудачно---нет-ответа-от-user-agent)
		- [3.11. Неудачно - временно недоступно](#311-неудачно---временно-недоступно)
	- [4. Соображения безопасности](#4-соображения-безопасности)
	- [5. Ссылки](#5-ссылки)
		- [5.1. Нормативные ссылки](#51-нормативные-ссылки)
		- [5.2. Информативные ссылки](#52-информативные-ссылки)
	- [6. Заявление об интеллектуальной собственности](#6-заявление-об-интеллектуальной-собственности)
	- [7. Благодарности](#7-благодарности)
	- [8. Адреса авторов](#8-адреса-авторов)
	- [9. Полное заявление об авторских правах](#9-полное-заявление-об-авторских-правах)

---

## 1. Обзор

Потоки вызовов, показанные в этом документе, были созданы при проектировании сети связи SIP IP. Они представляют собой пример минимального набора функциональных возможностей.

Авторы надеются, что этот документ будет полезен как разработчикам SIP, так и разработчикам протоколов и поможет в дальнейшем достижении цели стандартной реализации RFC 3261[1]. Эти потоки представляют собой тщательно проверенные и рассмотренные рабочей группой сценарии наиболее основных примеров в качестве дополнения к спецификациям.

Эти потоки вызовов основаны на текущей версии 2.0 SIP в RFC 3261[1] с использованием SDP, описанным в RFC 3264[2]. Другие RFC также включают стандарт SIP, но не используются в этом наборе основных потоков вызовов.

Показаны примеры взаимодействия потока SIP с ТфОП через шлюзы, содержащиеся в сопровождающем документе, документе RFC 3666[5].

Ключевые слова "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY" и "OPTIONAL" в настоящем документе следует толковать так, как описано в BCP 14, RFC 2119[4].

### 1.1 Главный постулат

В основе потоков вызовов в этом документе лежит ряд гипотез об архитектуре, сети и протоколе. Обратите внимание, что эти гипотезы не являются требованиями. Они изложены в этом разделе чтобы их можно было принять во внимание и помочь в понимании примеров потока вызовов.

Аутентификация пользовательских SIP-агентов в этих примерах потоков вызовов выполняется с использованием HTTP Digest, как определено в [1] и [3].

Некоторые прокси-серверы в этих потоках вызовов вставляют заголовки маршрута записи в запросы, чтобы гарантировать, что они находятся в сигнальном пути для будущих обменов сообщениями.

Эти потоки показывают TCP, TLS и UDP для транспорта. См. обсуждение в RFC 3261 для получения подробной информации о транспортных проблемах для SIP.

### 1.2. Условные обозначения для потоков сообщений

Пунктирные линии (`---`) представляют собой сигнальные сообщения, обязательные для сценария вызова. Эти сообщения могут быть сигнализацией SIP или PSTN. Стрелка указывает направление потока сообщений.

Двойные пунктирные линии (`===`) представляют собой медиа-пути между элементами сети.

Сообщения с круглыми скобками вокруг их имени представляют собой необязательные сообщения.

Сообщения, обозначенные на рисунках как F1, F2 и т.д. относятся к деталям сообщения в списке, следующем за рисунком.

Комментарии в деталях сообщения отображаются в следующем виде:

/* Комментарии. \*/

### 1.3 Постулат протокола SIP

Этот документ не подписывает потоки точно так, как они показаны, а скорее иллюстрирует принципы наилучшей практики. Это лучшие практики использования (упорядочение, синтаксис, выбор функций для этой цели, обработка ошибок) методов SIP, заголовков и параметров. Важно: точные потоки здесь не должны быть скопированы как есть разработчиком из-за конкретных неправильных характеристик, которые были введены в документ для удобства и перечислены ниже. Подводя итог, можно сказать, что основные потоки представляют собой хорошо изученные примеры использования SIP, которые являются наилучшей общепринятой практикой в соответствии с консенсусом IETF.

Для простоты чтения и редактирования документа существует ряд различий между некоторыми примерами и фактическими сообщениями SIP. Например, ответы HTTP Digest не являются фактическими кодировками MD5. Идентификаторы вызовов часто повторяются, и количество CSeq часто начинается с `1`. Поля заголовка обычно отображаются в том же порядке. Обычно отображается только минимально необходимый набор полей заголовка, другие, которые обычно присутствуют, такие как `Accept`, `Supported`, `Allow` и т.д. не отображаются.

Действующие лица:
```
   Element       Display Name   URI                         IP Address
   -------       ------------   ---                         ----------

   User Agent    Alice          alice@atlanta.example.com   192.0.2.101
   User Agent    Bob            bob@biloxi.example.com      192.0.2.201
   User Agent                   bob@chicago.example.com     192.0.2.100
   Proxy Server                 ss1.atlanta.example.com     192.0.2.111
   Proxy/Registrar              ss2.biloxi.example.com      192.0.2.222
   Proxy Server                 ss3.chicago.example.com     192.0.2.233
   ALG                          alg1.atlanta.example.com    192.0.2.128
```

## 2. SIP-регистрация

Регистрация связывает URI контакта конкретного устройства с адресом записи пользователя SIP (Address of Record - AOR).

### 2.1 Успешная новая регистрация

```
    Bob                        SIP Server
     |                               |
     |          REGISTER F1          |
     |------------------------------>|
     |      401 Unauthorized F2      |
     |<------------------------------|
     |          REGISTER F3          |
     |------------------------------>|
     |            200 OK F4          |
     |<------------------------------|
     |                               |
```

Bob отправляет запрос SIP `REGISTER` на SIP-сервер. Запрос включает в себя список контактов пользователя. Этот поток показывает использование HTTP дайджест для аутентификации с использованием TLS транспорта. Транспорт TLS используется из-за отсутствия защиты целостности в дайджесте HTTP и опасности захвата регистрации без него, как описано в RFC 3261[1].
Сервер SIP предоставляет Бобу вызов. Боб вводит свой действительный идентификатор пользователя и пароль. SIP-клиент Боба шифрует информацию о пользователе в соответствии с вызовом, выданным SIP-сервером, и отправляет ответ на SIP-сервер. SIP-сервер проверяет учетные данные пользователя. Он регистрирует пользователя в своей базе данных контактов и возвращает ответ (200 OK) SIP-клиенту Боба. Ответ включает текущий список контактов пользователя в заголовках контактов. Показанный формат аутентификации - дайджест HTTP. Предполагается, что Боб ранее не регистрировался на этом сервере.

Детали сообщения:

```
   F1 REGISTER Bob -> SIP Server

   REGISTER sips:ss2.biloxi.example.com SIP/2.0
   Via: SIP/2.0/TLS client.biloxi.example.com:5061;branch=z9hG4bKnashds7
   Max-Forwards: 70
   From: Bob <sips:bob@biloxi.example.com>;tag=a73kszlfl
   To: Bob <sips:bob@biloxi.example.com>
   Call-ID: 1j9FpLxk3uxtm8tn@biloxi.example.com
   CSeq: 1 REGISTER
   Contact: <sips:bob@client.biloxi.example.com>
   Content-Length: 0

   F2 401 Unauthorized SIP Server -> Bob

   SIP/2.0 401 Unauthorized
   Via: SIP/2.0/TLS client.biloxi.example.com:5061;branch=z9hG4bKnashds7
    ;received=192.0.2.201
   From: Bob <sips:bob@biloxi.example.com>;tag=a73kszlfl
   To: Bob <sips:bob@biloxi.example.com>;tag=1410948204
   Call-ID: 1j9FpLxk3uxtm8tn@biloxi.example.com
   CSeq: 1 REGISTER
   WWW-Authenticate: Digest realm="atlanta.example.com", qop="auth",
    nonce="ea9c8e88df84f1cec4341ae6cbe5a359",
    opaque="", stale=FALSE, algorithm=MD5
   Content-Length: 0

   F3 REGISTER Bob -> SIP Server

   REGISTER sips:ss2.biloxi.example.com SIP/2.0
   Via: SIP/2.0/TLS client.biloxi.example.com:5061;branch=z9hG4bKnashd92
   Max-Forwards: 70
   From: Bob <sips:bob@biloxi.example.com>;tag=ja743ks76zlflH
   To: Bob <sips:bob@biloxi.example.com>
   Call-ID: 1j9FpLxk3uxtm8tn@biloxi.example.com
   CSeq: 2 REGISTER
   Contact: <sips:bob@client.biloxi.example.com>
   Authorization: Digest username="bob", realm="atlanta.example.com"
    nonce="ea9c8e88df84f1cec4341ae6cbe5a359", opaque="",
    uri="sips:ss2.biloxi.example.com",
    response="dfe56131d1958046689d83306477ecc"
   Content-Length: 0

   F4 200 OK SIP Server -> Bob

   SIP/2.0 200 OK
   Via: SIP/2.0/TLS client.biloxi.example.com:5061;branch=z9hG4bKnashd92
    ;received=192.0.2.201
   From: Bob <sips:bob@biloxi.example.com>;tag=ja743ks76zlflH
   To: Bob <sips:bob@biloxi.example.com>;tag=37GkEhwl6
   Call-ID: 1j9FpLxk3uxtm8tn@biloxi.example.com
   CSeq: 2 REGISTER
   Contact: <sips:bob@client.biloxi.example.com>;expires=3600
   Content-Length: 0
```

### 2.2. Обновление списка контактов

```
   Bob                        SIP Server
     |                               |
     |          REGISTER F1          |
     |------------------------------>|
     |            200 OK F2          |
     |<------------------------------|
     |                               |
```

Боб хочет обновить список адресов, по которым SIP-сервер будет перенаправлять или пересылать запросы `INVITE`.

Боб отправляет запрос SIP `REGISTER` на SIP-сервер. Запрос Боба включает в себя обновленный список контактов. Поскольку пользователь уже прошел проверку подлинности на сервере - он предоставляет учетные данные проверки подлинности вместе с запросом и не оспаривается сервером. SIP-сервер проверяет учетные данные пользователя. Он регистрирует пользователя в своей базе данных контактов, обновляет список контактов пользователя и возвращает ответ (`200 OK`) SIP-клиенту Боба. Ответ включает текущий список контактов пользователя в заголовках контактов.

Детали сообщения:

```
   F1 REGISTER Bob -> SIP Server

   REGISTER sips:ss2.biloxi.example.com SIP/2.0
   Via: SIP/2.0/TLS client.biloxi.example.com:5061;branch=z9hG4bKnashds7
   Max-Forwards: 70
   From: Bob <sips:bob@biloxi.example.com>;tag=a73kszlfl
   To: Bob <sips:bob@biloxi.example.com>
   Call-ID: 1j9FpLxk3uxtm8tn@biloxi.example.com
   CSeq: 1 REGISTER
   Contact: mailto:bob@biloxi.example.com
   Authorization: Digest username="bob", realm="atlanta.example.com",
    qop="auth", nonce="1cec4341ae6cbe5a359ea9c8e88df84f", opaque="",
    uri="sips:ss2.biloxi.example.com",
    response="71ba27c64bd01de719686aa4590d5824"
   Content-Length: 0

   F2 200 OK SIP Server -> Bob

   SIP/2.0 200 OK
   Via: SIP/2.0/TLS client.biloxi.example.com:5061;branch=z9hG4bKnashds7
    ;received=192.0.2.201
   From: Bob <sips:bob@biloxi.example.com>;tag=a73kszlfl
   To: Bob <sips:bob@biloxi.example.com>;tag=34095828jh

   Call-ID: 1j9FpLxk3uxtm8tn@biloxi.example.com
   CSeq: 1 REGISTER
   Contact: <sips:bob@client.biloxi.example.com>;expires=3600
   Contact: <mailto:bob@biloxi.example.com>;expires=4294967295
   Content-Length: 0
```

### 2.3. Запрос текущего списка контактов

```
   Bob                        SIP Server
     |                               |
     |          REGISTER F1          |
     |------------------------------>|
     |            200 OK F2          |
     |<------------------------------|
     |                               |
```

Боб отправляет запрос регистрации, не содержащий заголовков контактов, на прокси-сервер, указывая, что пользователь хочет запросить у сервера текущий список контактов пользователя. Поскольку пользователь уже прошел проверку подлинности на сервере - он предоставляет учетные данные аутентификации вместе с запросом и не оспаривается сервером. SIP-сервер проверяет учетные данные пользователя. Сервер возвращает ответ (`200 OK`), который включает текущий список регистрации пользователя в заголовках контактов.

Детали сообщения:

```
   F1 REGISTER Bob -> SIP Server

   REGISTER sips:ss2.biloxi.example.com SIP/2.0
   Via: SIP/2.0/TLS client.biloxi.example.com:5061;branch=z9hG4bKnashds7
   Max-Forwards: 70
   From: Bob <sips:bob@biloxi.example.com>;tag=a73kszlfl
   To: Bob <sips:bob@biloxi.example.com>
   Call-ID: 1j9FpLxk3uxtm8tn@biloxi.example.com
   CSeq: 1 REGISTER
   Authorization: Digest username="bob", realm="atlanta.example.com",
    nonce="df84f1cec4341ae6cbe5ap359a9c8e88", opaque="",
    uri="sips:ss2.biloxi.example.com",
    response="aa7ab4678258377c6f7d4be6087e2f60"
   Content-Length: 0

   F2 200 OK SIP Server -> Bob

   SIP/2.0 200 OK
   Via: SIP/2.0/TLS client.biloxi.example.com:5061;branch=z9hG4bKnashds7
    ;received=192.0.2.201

   From: Bob <sips:bob@biloxi.example.com>;tag=a73kszlfl
   To: Bob <sips:bob@biloxi.example.com>;tag=jqoiweu75
   Call-ID: 1j9FpLxk3uxtm8tn@biloxi.example.com
   CSeq: 1 REGISTER
   Contact: <sips:bob@client.biloxi.example.com>;expires=3600
   Contact: <mailto:bob@biloxi.example.com>;expires=4294967295
   Content-Length: 0
```

### 2.4. Отмена регистрации

```
   Bob                         SIP Server
     |                               |
     |          REGISTER F1          |
     |------------------------------>|
     |            200 OK F2          |
     |<------------------------------|
     |                               |
```

Боб хочет отменить его регистрацию на SIP-сервере. Боб отправляет SIP-запрос `REGISTER` на SIP-сервер. Срок действия запроса равен `0` и применяется ко всем существующим контактам. Поскольку пользователь уже прошел аутентификацию на сервере - он предоставляет учетные данные аутентификации вместе с запросом и не оспаривается сервером. SIP-сервер проверяет учетные данные пользователя. Он очищает список контактов пользователя и возвращает ответ (`200 OK`) SIP-клиенту Боба.

Детали сообщения:

```
   F1 REGISTER Bob -> SIP Server

   REGISTER sips:ss2.biloxi.example.com SIP/2.0
   Via: SIP/2.0/TLS client.biloxi.example.com:5061;branch=z9hG4bKnashds7
   Max-Forwards: 70
   From: Bob <sips:bob@biloxi.example.com>;tag=a73kszlfl
   To: Bob <sips:bob@biloxi.example.com>
   Call-ID: 1j9FpLxk3uxtm8tn@biloxi.example.com
   CSeq: 1 REGISTER
   Expires: 0
   Contact: *
   Authorization: Digest username="bob", realm="atlanta.example.com",
    nonce="88df84f1cac4341aea9c8ee6cbe5a359", opaque="",
    uri="sips:ss2.biloxi.example.com",
    response="ff0437c51696f9a76244f0cf1dbabbea"
   Content-Length: 0

   F2 200 OK SIP Server -> Bob

   SIP/2.0 200 OK
   Via: SIP/2.0/TLS client.biloxi.example.com:5061;branch=z9hG4bKnashds7
    ;received=192.0.2.201
   From: Bob <sips:bob@biloxi.example.com>;tag=a73kszlfl
   To: Bob <sips:bob@biloxi.example.com>;tag=1418nmdsrf
   Call-ID: 1j9FpLxk3uxtm8tn@biloxi.example.com
   CSeq: 1 REGISTER
   Content-Length: 0
```

### 2.5. Неудачная регистрация

```
   Bob                        SIP Server
     |                               |
     |          REGISTER F1          |
     |------------------------------>|
     |      401 Unauthorized F2      |
     |<------------------------------|
     |          REGISTER F3          |
     |------------------------------>|
     |      401 Unauthorized F4      |
     |<------------------------------|
     |                               |
```

Боб отправляет SIP-запрос `REGISTER` на SIP-сервер. SIP-сервер предоставляет Бобу вызов. Боб вводит свой идентификатор пользователя и пароль. SIP-клиент Боба шифрует информацию о пользователе в соответствии с вызовом, выданным SIP-сервером, и отправляет ответ на SIP-сервер. SIP-сервер пытается проверить учетные данные пользователя, но они недействительны (пароль пользователя не совпадает с паролем, установленным для учетной записи пользователя). Сервер возвращает ответ (`401 Unauthorized`) SIP-клиенту Боба.

Детали сообщения:

```
   F1 REGISTER Bob -> SIP Server

   REGISTER sips:ss2.biloxi.example.com SIP/2.0
   Via: SIP/2.0/TLS client.biloxi.example.com:5061;branch=z9hG4bKnashds7
    ;received=192.0.2.201
   From: Bob <sips:bob@biloxi.example.com>;tag=a73kszlfl
   To: Bob <sips:bob@biloxi.example.com>
   Call-ID: 1j9FpLxk3uxtm8tn@biloxi.example.com
   CSeq: 1 REGISTER
   Contact: <sips:bob@client.biloxi.example.com>
   Content-Length: 0

   F2 Unauthorized SIP Server -> Bob

   SIP/2.0 401 Unauthorized
   Via: SIP/2.0/TLS client.biloxi.example.com:5061;branch=z9hG4bKnashds7
    ;received=192.0.2.201
   From: Bob <sips:bob@biloxi.example.com>;tag=a73kszlfl
   To: Bob <sips:bob@biloxi.example.com>;tag=1410948204
   Call-ID: 1j9FpLxk3uxtm8tn@biloxi.example.com
   CSeq: 1 REGISTER
   WWW-Authenticate: Digest realm="atlanta.example.com", qop="auth",
    nonce="f1cec4341ae6ca9c8e88df84be55a359",
    opaque="", stale=FALSE, algorithm=MD5
   Content-Length: 0

   F3 REGISTER Bob -> SIP Server

   REGISTER sips:ss2.biloxi.example.com SIP/2.0
   Via: SIP/2.0/TLS client.biloxi.example.com:5061;branch=z9hG4bKnashd92
   Max-Forwards: 70
   From: Bob <sips:bob@biloxi.example.com>;tag=JueHGuidj28dfga
   To: Bob <sips:bob@biloxi.example.com>
   Call-ID: 1j9FpLxk3uxtm8tn@biloxi.example.com
   CSeq: 2 REGISTER
   Contact: <sips:bob@client.biloxi.example.com>
   Authorization: Digest username="bob", realm="atlanta.example.com",
    nonce="f1cec4341ae6ca9c8e88df84be55a359", opaque="",
    uri="sips:ss2.biloxi.example.com",
    response="61f8470ceb87d7ebf508220214ed438b"
   Content-Length: 0

   /*  В приведенном выше ответе кодируется неверный пароль */

   F4 401 Unauthorized SIP Server -> Bob

   SIP/2.0 401 Unauthorized
   Via: SIP/2.0/TLS client.biloxi.example.com:5061;branch=z9hG4bKnashd92
    ;received=192.0.2.201
   From: Bob <sips:bob@biloxi.example.com>;tag=JueHGuidj28dfga
   To: Bob <sips:bob@biloxi.example.com>;tag=1410948204
   Call-ID: 1j9FpLxk3uxtm8tn@biloxi.example.com
   CSeq: 2 REGISTER
   WWW-Authenticate: Digest realm="atlanta.example.com", qop="auth",
    nonce="84f1c1ae6cbe5ua9c8e88dfa3ecm3459",
    opaque="", stale=FALSE, algorithm=MD5
   Content-Length: 0
```

## 3. Установка сеанса SIP

В этом разделе подробно описывается установление сеанса между двумя агентами SIP-пользователей (UAs): Alice и Bob. Alice (sip:alice@atlanta.example.com) и Bob (sip:bob@biloxi.example.com) считаются SIP-телефонами или устройствами с поддержкой SIP. Успешные вызовы показывают начальную сигнализацию, обмен медиа-информацией в виде полезных нагрузок SDP, установление медиа - сеанса, а затем, наконец, завершение вызова.

Дайджест-проверки подлинности http используется прокси-сервером для аутентификации абонента Alice. Предполагается, что Боб зарегистрировался на прокси-сервере `Proxy 2` в соответствии с разделом 2, чтобы иметь возможность принимать вызовы через прокси-сервер.

### 3.1. Успешная установка сеанса

```
   Alice                     Bob
     |                        |
     |       INVITE F1        |
     |----------------------->|
     |    180 Ringing F2      |
     |<-----------------------|
     |                        |
     |       200 OK F3        |
     |<-----------------------|
     |         ACK F4         |
     |----------------------->|
     |   Both Way RTP Media   |
     |<======================>|
     |                        |
     |         BYE F5         |
     |<-----------------------|
     |       200 OK F6        |
     |----------------------->|
     |                        |
```

В этом сценарии Алиса выполняет вызов Боба напрямую.

Детали сообщения:

```
   F1 INVITE Alice -> Bob

   INVITE sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>

   Call-ID: 3848276298220188511@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:alice@client.atlanta.example.com;transport=tcp>
   Content-Type: application/sdp
   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000

   F2 180 Ringing Bob -> Alice

   SIP/2.0 180 Ringing
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=8321234356
   Call-ID: 3848276298220188511@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:bob@client.biloxi.example.com;transport=tcp>
   Content-Length: 0

   F3 200 OK Bob -> Alice

   SIP/2.0 200 OK
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=8321234356
   Call-ID: 3848276298220188511@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:bob@client.biloxi.example.com;transport=tcp>
   Content-Type: application/sdp
   Content-Length: 147

   v=0
   o=bob 2890844527 2890844527 IN IP4 client.biloxi.example.com
   s=-
   c=IN IP4 192.0.2.201
   t=0 0
   m=audio 3456 RTP/AVP 0
   a=rtpmap:0 PCMU/8000

   F4 ACK Alice -> Bob

   ACK sip:bob@client.biloxi.example.com SIP/2.0
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bd5
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=8321234356
   Call-ID: 3848276298220188511@atlanta.example.com
   CSeq: 1 ACK
   Content-Length: 0

   /* Потоки RTP устанавливаются между Alice и Bob. */

   /* Bob вешает трубку с Alice. Обратите внимание, что CSeq НЕ равен 2,
	    поскольку Alice и Bob поддерживают свои собственные независимые
			счетчики CSeq. (INVITE - это запрос 1, сгенерированный Алисой,
			а BYE - это запрос 1, сгенерированный Бобом) */

   F5 BYE Bob -> Alice

   BYE sip:alice@client.atlanta.example.com SIP/2.0
   Via: SIP/2.0/TCP client.biloxi.example.com:5060;branch=z9hG4bKnashds7
   Max-Forwards: 70
   From: Bob <sip:bob@biloxi.example.com>;tag=8321234356
   To: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   Call-ID: 3848276298220188511@atlanta.example.com
   CSeq: 1 BYE
   Content-Length: 0

   F6 200 OK Alice -> Bob

   SIP/2.0 200 OK
   Via: SIP/2.0/TCP client.biloxi.example.com:5060;branch=z9hG4bKnashds7
    ;received=192.0.2.201
   From: Bob <sip:bob@biloxi.example.com>;tag=8321234356
   To: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   Call-ID: 3848276298220188511@atlanta.example.com
   CSeq: 1 BYE
   Content-Length: 0
```

### 3.2. Установка сеанса через два прокси

```
   Alice           Proxy 1          Proxy 2            Bob
     |                |                |                |
     |   INVITE F1    |                |                |
     |--------------->|                |                |
     |     407 F2     |                |                |
     |<---------------|                |                |
     |     ACK F3     |                |                |
     |--------------->|                |                |
     |   INVITE F4    |                |                |
     |--------------->|   INVITE F5    |                |
     |     100  F6    |--------------->|   INVITE F7    |
     |<---------------|     100  F8    |--------------->|
     |                |<---------------|                |
     |                |                |     180 F9     |
     |                |    180 F10     |<---------------|
     |     180 F11    |<---------------|                |
     |<---------------|                |     200 F12    |
     |                |    200 F13     |<---------------|
     |     200 F14    |<---------------|                |
     |<---------------|                |                |
     |     ACK F15    |                |                |
     |--------------->|    ACK F16     |                |
     |                |--------------->|     ACK F17    |
     |                |                |--------------->|
     |                Both Way RTP Media                |
     |<================================================>|
     |                |                |     BYE F18    |
     |                |    BYE F19     |<---------------|
     |     BYE F20    |<---------------|                |
     |<---------------|                |                |
     |     200 F21    |                |                |
     |--------------->|     200 F22    |                |
     |                |--------------->|     200 F23    |
     |                |                |--------------->|
     |                |                |                |
```

В этом сценарии Alice выполняет вызов Bob, используя два прокси-сервера `Proxy 1` и `Proxy 2`. Первоначальное сообщение `INVITE` (F1) содержит предварительно загруженный заголовок `Route` с адресом `Proxy 1` (Proxy 1 настроен как исходящий прокси по умолчанию для Alice.). Запрос не содержит учетных данных авторизации, которые требуются `Proxy 1`, поэтому отправляется ответ `407 Proxy Authorization`, содержащий информацию о запросе. Затем отправляется новое сообщение `INVITE` (F4) с правильными учетными данными и вызов продолжается. Вызов завершается, когда Боб отключается, отправляя сообщение `BYE`.

Proxy 1 вставляет заголовок `Record-Route` в сообщение `INVITE`, чтобы гарантировать его присутствие во всех последующих обменах сообщениями. Proxy 2 также вставляется в заголовок `Record-Route`. `ACK` (F15) и `BYE` (F18) имеют заголовок `Route`.

Детали сообщения:

```
   F1 INVITE Alice -> Proxy 1

   INVITE sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74b43
   Max-Forwards: 70
   Route: <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 3848276298220188511@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:alice@client.atlanta.example.com;transport=tcp>
   Content-Type: application/sdp
   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000

   /* Proxy 1 запрашивает у Alice аутентификацию */

   F2 407 Proxy Authorization Required Proxy 1 -> Alice

   SIP/2.0 407 Proxy Authorization Required
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74b43
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=3flal12sf
   Call-ID: 3848276298220188511@atlanta.example.com
   CSeq: 1 INVITE
   Proxy-Authenticate: Digest realm="atlanta.example.com", qop="auth",
    nonce="f84f1cec41e6cbe5aea9c8e88d359",
    opaque="", stale=FALSE, algorithm=MD5
   Content-Length: 0

   F3 ACK Alice -> Proxy 1

   ACK sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74b43
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=3flal12sf
   Call-ID: 3848276298220188511@atlanta.example.com
   CSeq: 1 ACK
   Content-Length: 0

   /* Alice в ответ повторно отправляет INVITE с учетными
	    данными для аутентификации. */

   F4 INVITE Alice -> Proxy 1

   INVITE sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
   Max-Forwards: 70
   Route: <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 3848276298220188511@atlanta.example.com
   CSeq: 2 INVITE
   Contact: <sip:alice@client.atlanta.example.com;transport=tcp>
   Proxy-Authorization: Digest username="alice",
    realm="atlanta.example.com",
    nonce="wf84f1ceczx41ae6cbe5aea9c8e88d359", opaque="",
    uri="sip:bob@biloxi.example.com",
    response="42ce3cef44b22f50c6a6071bc8"
   Content-Type: application/sdp
   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000

   /* Proxy 1 принимает учетные данные и пересылает INVITE на Proxy 2.
	    Клиент для Алисы готовится к приему данных на порт 49172 из сети. */

   F5 INVITE Proxy 1 -> Proxy 2

   INVITE sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/TCP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Max-Forwards: 69
   Record-Route: <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 3848276298220188511@atlanta.example.com
   CSeq: 2 INVITE
   Contact: <sip:alice@client.atlanta.example.com;transport=tcp>
   Content-Type: application/sdp
   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000

   F6 100 Trying Proxy 1 -> Alice

   SIP/2.0 100 Trying
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 3848276298220188511@atlanta.example.com
   CSeq: 2 INVITE
   Content-Length: 0

   F7 INVITE Proxy 2 -> Bob

   INVITE sip:bob@client.biloxi.example.com SIP/2.0
   Via: SIP/2.0/TCP ss2.biloxi.example.com:5060;branch=z9hG4bK721e4.1
   Via: SIP/2.0/TCP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.111
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Max-Forwards: 68
   Record-Route: <sip:ss2.biloxi.example.com;lr>,
    <sip:ss1.atlanta.example.com;lr>

   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 3848276298220188511@atlanta.example.com
   CSeq: 2 INVITE
   Contact: <sip:alice@client.atlanta.example.com;transport=tcp>
   Content-Type: application/sdp
   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000

   F8 100 Trying Proxy 2 -> Proxy 1

   SIP/2.0 100 Trying
   Via: SIP/2.0/TCP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.111
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 3848276298220188511@atlanta.example.com
   CSeq: 2 INVITE
   Content-Length: 0

   F9 180 Ringing Bob -> Proxy 2

   SIP/2.0 180 Ringing
   Via: SIP/2.0/TCP ss2.biloxi.example.com:5060;branch=z9hG4bK721e4.1
    ;received=192.0.2.222
   Via: SIP/2.0/TCP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.111
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Record-Route: <sip:ss2.biloxi.example.com;lr>,
    <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 3848276298220188511@atlanta.example.com
   Contact: <sip:bob@client.biloxi.example.com;transport=tcp>
   CSeq: 2 INVITE
   Content-Length: 0

   F10 180 Ringing Proxy 2 -> Proxy 1

   SIP/2.0 180 Ringing
   Via: SIP/2.0/TCP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.111
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Record-Route: <sip:ss2.biloxi.example.com;lr>,
    <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 3848276298220188511@atlanta.example.com
   Contact: <sip:bob@client.biloxi.example.com;transport=tcp>
   CSeq: 2 INVITE
   Content-Length: 0

   F11 180 Ringing Proxy 1 -> Alice

   SIP/2.0 180 Ringing
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Record-Route: <sip:ss2.biloxi.example.com;lr>,
    <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 3848276298220188511@atlanta.example.com
   Contact: <sip:bob@client.biloxi.example.com;transport=tcp>
   CSeq: 2 INVITE
   Content-Length: 0

   F12 200 OK Bob -> Proxy 2

   SIP/2.0 200 OK
   Via: SIP/2.0/TCP ss2.biloxi.example.com:5060;branch=z9hG4bK721e4.1
    ;received=192.0.2.222
   Via: SIP/2.0/TCP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.111
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Record-Route: <sip:ss2.biloxi.example.com;lr>,
    <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 3848276298220188511@atlanta.example.com
   CSeq: 2 INVITE

   Contact: <sip:bob@client.biloxi.example.com;transport=tcp>
   Content-Type: application/sdp
   Content-Length: 147

   v=0
   o=bob 2890844527 2890844527 IN IP4 client.biloxi.example.com
   s=-
   c=IN IP4 192.0.2.201
   t=0 0
   m=audio 3456 RTP/AVP 0
   a=rtpmap:0 PCMU/8000

   F13 200 OK Proxy 2 -> Proxy 1

   SIP/2.0 200 OK
   Via: SIP/2.0/TCP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.111
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Record-Route: <sip:ss2.biloxi.example.com;lr>,
    <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 3848276298220188511@atlanta.example.com
   CSeq: 2 INVITE
   Contact: <sip:bob@client.biloxi.example.com;transport=tcp>
   Content-Type: application/sdp
   Content-Length: 147

   v=0
   o=bob 2890844527 2890844527 IN IP4 client.biloxi.example.com
   s=-
   c=IN IP4 192.0.2.201
   t=0 0
   m=audio 3456 RTP/AVP 0
   a=rtpmap:0 PCMU/8000


   F14 200 OK Proxy 1 -> Alice

   SIP/2.0 200 OK
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Record-Route: <sip:ss2.biloxi.example.com;lr>,
    <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159

   Call-ID: 3848276298220188511@atlanta.example.com
   CSeq: 2 INVITE
   Contact: <sip:bob@client.biloxi.example.com;transport=tcp>
   Content-Type: application/sdp
   Content-Length: 147

   v=0
   o=bob 2890844527 2890844527 IN IP4 client.biloxi.example.com
   s=-
   c=IN IP4 192.0.2.201
   t=0 0
   m=audio 3456 RTP/AVP 0
   a=rtpmap:0 PCMU/8000

   F15 ACK Alice -> Proxy 1

   ACK sip:bob@client.biloxi.example.com SIP/2.0
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74b76
   Max-Forwards: 70
   Route: <sip:ss1.atlanta.example.com;lr>,
    <sip:ss2.biloxi.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 3848276298220188511@atlanta.example.com
   CSeq: 2 ACK
   Content-Length: 0

   F16 ACK Proxy 1 -> Proxy 2

   ACK sip:bob@client.biloxi.example.com SIP/2.0
   Via: SIP/2.0/TCP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74b76
    ;received=192.0.2.101
   Max-Forwards: 69
   Route: <sip:ss2.biloxi.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 3848276298220188511@atlanta.example.com
   CSeq: 2 ACK
   Content-Length: 0

   F17 ACK Proxy 2 -> Bob

   ACK sip:bob@client.biloxi.example.com SIP/2.0
   Via: SIP/2.0/TCP ss2.biloxi.example.com:5060;branch=z9hG4bK721e4.1

   Via: SIP/2.0/TCP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.111
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74b76
    ;received=192.0.2.101
   Max-Forwards: 68
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 3848276298220188511@atlanta.example.com
   CSeq: 2 ACK
   Content-Length: 0

	 /* RTP-потоки RTP устанавливаются между Алисой и Бобом */

	 /* Боб вешает трубку вместе с Алисой. */

	 /* Опять же, обратите внимание, что CSeq не равен 3. Алиса и Боб сохраняют
	 их собственные отдельные счетчики CSeq */

   F18 BYE Bob -> Proxy 2

   BYE sip:alice@client.atlanta.example.com SIP/2.0
   Via: SIP/2.0/TCP client.biloxi.example.com:5060;branch=z9hG4bKnashds7
   Max-Forwards: 70
   Route: <sip:ss2.biloxi.example.com;lr>,
    <sip:ss1.atlanta.example.com;lr>
   From: Bob <sip:bob@biloxi.example.com>;tag=314159
   To: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   Call-ID: 3848276298220188511@atlanta.example.com
   CSeq: 1 BYE
   Content-Length: 0

   F19 BYE Proxy 2 -> Proxy 1

   BYE sip:alice@client.atlanta.example.com SIP/2.0
   Via: SIP/2.0/TCP ss2.biloxi.example.com:5060;branch=z9hG4bK721e4.1
   Via: SIP/2.0/TCP client.biloxi.example.com:5060;branch=z9hG4bKnashds7
    ;received=192.0.2.201
   Max-Forwards: 69
   Route: <sip:ss1.atlanta.example.com;lr>
   From: Bob <sip:bob@biloxi.example.com>;tag=314159
   To: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   Call-ID: 3848276298220188511@atlanta.example.com
   CSeq: 1 BYE
   Content-Length: 0

   F20 BYE Proxy 1 -> Alice

   BYE sip:alice@client.atlanta.example.com SIP/2.0
   Via: SIP/2.0/TCP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
   Via: SIP/2.0/TCP ss2.biloxi.example.com:5060;branch=z9hG4bK721e4.1
    ;received=192.0.2.222
   Via: SIP/2.0/TCP client.biloxi.example.com:5060;branch=z9hG4bKnashds7
    ;received=192.0.2.201
   Max-Forwards: 68
   From: Bob <sip:bob@biloxi.example.com>;tag=314159
   To: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   Call-ID: 3848276298220188511@atlanta.example.com
   CSeq: 1 BYE
   Content-Length: 0


   F21 200 OK Alice -> Proxy 1

   SIP/2.0 200 OK
   Via: SIP/2.0/TCP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.111
   Via: SIP/2.0/TCP ss2.biloxi.example.com:5060;branch=z9hG4bK721e4.1
    ;received=192.0.2.222
   Via: SIP/2.0/TCP client.biloxi.example.com:5060;branch=z9hG4bKnashds7
    ;received=192.0.2.201
   From: Bob <sip:bob@biloxi.example.com>;tag=314159
   To: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   Call-ID: 3848276298220188511@atlanta.example.com
   CSeq: 1 BYE
   Content-Length: 0


   F22 200 OK Proxy 1 -> Proxy 2

   SIP/2.0 200 OK
   Via: SIP/2.0/TCP ss2.biloxi.example.com:5060;branch=z9hG4bK721e4.1
    ;received=192.0.2.222
   Via: SIP/2.0/TCP client.biloxi.example.com:5060;branch=z9hG4bKnashds7
    ;received=192.0.2.101
   From: Bob <sip:bob@biloxi.example.com>;tag=314159
   To: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   Call-ID: 3848276298220188511@atlanta.example.com
   CSeq: 1 BYE
   Content-Length: 0

   F23 200 OK Proxy 2 -> Bob

   SIP/2.0 200 OK
   Via: SIP/2.0/TCP client.biloxi.example.com:5060;branch=z9hG4bKnashds7
    ;received=192.0.2.201
   From: Bob <sip:bob@biloxi.example.com>;tag=314159
   To: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   Call-ID: 3848276298220188511@atlanta.example.com
   CSeq: 1 BYE
   Content-Length: 0
```

### 3.3. Сеанс с множественной прокси-аутентификацией

```
     Alice        Proxy 1     Proxy 2         Bob
       |            |           |             |
       |  INVITE F1 |           |             |
       |----------->|           |             |
       |  407 Proxy Authorization Required F2 |
       |<-----------|           |             |
       |   ACK F3   |           |             |
       |----------->|           |             |
       |  INVITE F4 |           |             |
       |----------->|           |             |
       |   100 F5   |           |             |
       |<-----------| INVITE F6 |             |
       |            |---------->|             |
       |            |  407 Proxy Authorization Required F7
       |            |<----------|             |
       |            |   ACK F8  |             |
       |            |---------->|             |
       |  407 Proxy Authorization Required F9 |
       |<-----------|           |             |
       |   ACK F10  |           |             |
       |----------->|           |             |
       |  INVITE F11|           |             |
       |----------->|           |             |
       |   100 F12  |           |             |
       |<-----------| INVITE F13|             |
       |            |---------->|             |
       |            |  100 F14  |             |
       |            |<----------|  INVITE F15 |
       |            |           |------------>|
       |            |           | 200 OK F16  |
       |            | 200 OK F17|<------------|
       | 200 OK F18 |<----------|             |
       |<-----------|           |             |
       |   ACK F19  |           |             |
       |----------->|  ACK F20  |             |
       |            |---------->|   ACK F21   |
       |            |           |------------>|
       |           RTP Media Path             |
       |<====================================>|
```

В этом сценарии Alice выполняет вызов Боба, используя два прокси-сервера `Proxy 1` и `Proxy 2`. Alice имеет действительные учетные данные в обоих доменах. Поскольку начальный `INVITE (F1)` не содержит учетных данных авторизации, необходимых `Proxy 1`, поэтому отправляется ответ `407 Proxy Authorization`, содержащий информацию о вызове. Затем отправляется новый запрос `INVITE (F4)`, содержащий действительные учетные данные, и вызов продолжается после запроса `Proxy 2` и получения действительных учетных данных. Вызов завершается, когда Боб отключается, инициируя сообщение `BYE`.

`Proxy 1` вставляет заголовок Record-Route в сообщение `INVITE`, чтобы убедиться, что он присутствует во всех последующих обменах сообщениями. `Proxy 2` также вставляет себя в заголовок Record-Route.

Детали сообщения:

```
   F1 INVITE Alice -> Proxy 1

   INVITE sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74b03
   Max-Forwards: 70
   Route: <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:alice@client.atlanta.example.com>
   Content-Type: application/sdp
   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000

   /* Proxy 1 запрашивает у Alice аутентификацию */


   F2 407 Proxy Authorization Required Proxy 1 -> Alice

   SIP/2.0 407 Proxy Authorization Required
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74b03
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=876321
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Proxy-Authenticate: Digest realm="atlanta.example.com", qop="auth",
    nonce="wf84f1cczx41ae6cbeaea9ce88d359",
    opaque="", stale=FALSE, algorithm=MD5
   Content-Length: 0

   F3 ACK Alice -> Proxy 1

   ACK sip:bob@biloxi.example.com SIP/2.0
   Max-Forwards: 70
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74b03
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=876321
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 ACK
   Content-Length: 0

   /* Alice отвечает, повторно отправляя INVITE с учетными данными
	 аутентификации в нем. Используется тот же Call-ID, поэтому
	 CSeq увеличивается. */

   F4 INVITE Alice -> Proxy 1

   INVITE sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74b21
   Max-Forwards: 70
   Route: <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 2 INVITE
   Contact: <sip:alice@client.atlanta.example.com>
   Proxy-Authorization: Digest username="alice",
    realm="atlanta.example.com",
    nonce="wf84f1ceczx41ae6cbe5aea9c8e88d359", opaque="",
    uri="sip:bob@biloxi.example.com",
    response="42ce3cef44b22f50c6a6071bc8"
   Content-Type: application/sdp
   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000

   / * Proxy 1 принимает учетные данные и пересылает INVITE на Proxy 2.
	 Клиент Alice готовится к приему данных на порт 49172 из сети. */

   F5 100 Trying Proxy 1 -> Alice

   SIP/2.0 100 Trying
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74b21
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 2 INVITE
   Content-Length: 0


   F6 INVITE Proxy 1 -> Proxy 2

   INVITE sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK230f2.1
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74b21
    ;received=192.0.2.101
   Max-Forwards: 69
   Record-Route: <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 2 INVITE
   Contact: <sip:alice@client.atlanta.example.com>
   Content-Type: application/sdp
   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000

   /* Proxy 2 запрашивает у Alice аутентификацию */

   F7 407 Proxy Authorization Required Proxy 2 -> Proxy 1

   SIP/2.0 407 Proxy Authorization Required
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK230f2.1
    ;received=192.0.2.111
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74b21
    ;received=192.0.2.101

   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=838209
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 2 INVITE
   Proxy-Authenticate: Digest realm="biloxi.example.com", qop="auth",
    nonce="c1e22c41ae6cbe5ae983a9c8e88d359",
    opaque="", stale=FALSE, algorithm=MD5
   Content-Length: 0


   F8 ACK Proxy 1 -> Proxy 2

   ACK sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74b21
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=838209
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 2 ACK
   Content-Length: 0

   /* Proxy 1 пересылает вызов Алисе для аутентификации от Proxy 2 */


   F9 407 Proxy Authorization Required Proxy 1 -> Alice

   SIP/2.0 407 Proxy Authorization Required
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74b21
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=838209
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 2 INVITE
   Proxy-Authenticate: Digest realm="biloxi.example.com", qop="auth",
    nonce="c1e22c41ae6cbe5ae983a9c8e88d359",
    opaque="", stale=FALSE, algorithm=MD5
   Content-Length: 0


   F10 ACK Alice -> Proxy 1

   ACK sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74b21
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=838209
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com

   CSeq: 2 ACK
   Proxy-Authorization: Digest username="alice",
    realm="atlanta.example.com",
    nonce="wf84f1ceczx41ae6cbe5aea9c8e88d359", opaque="",
    uri="sip:bob@biloxi.example.com",
    response="42ce3cef44b22f50c6a6071bc8"
   Content-Length: 0

	 /* Алиса отвечает повторной отправкой INVITE с данными аутентификации
   для Proxy 1 и Proxy 2. */


   F11 INVITE Alice -> Proxy 1

   INVITE sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 3 INVITE
   Contact: <sip:alice@client.atlanta.example.com>
   Proxy-Authorization: Digest username="alice",
    realm="atlanta.example.com",
    nonce="wf84f1ceczx41ae6cbe5aea9c8e88d359", opaque="",
    uri="sip:bob@biloxi.example.com",
    response="42ce3cef44b22f50c6a6071bc8"
   Proxy-Authorization: Digest username="alice",
    realm="biloxi.example.com",
    nonce="c1e22c41ae6cbe5ae983a9c8e88d359", opaque="",
    uri="sip:bob@biloxi.example.com", response="f44ab22f150c6a56071bce8"
   Content-Type: application/sdp
   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000

   /* Proxy 1 находит свои учетные данные и авторизует Алису,
	 перенаправляя INVITE на прокси. */

   F12 100 Trying Proxy 1 -> Alice

   SIP/2.0 100 Trying
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 3 INVITE
   Content-Length: 0


   F13 INVITE Proxy 1 -> Proxy 2

   INVITE sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK230f2.1
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Max-Forwards: 69
   Record-Route: <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 3 INVITE
   Contact: <sip:alice@client.atlanta.example.com>
   Proxy-Authorization: Digest username="alice",
    realm="biloxi.example.com",
    nonce="c1e22c41ae6cbe5ae983a9c8e88d359", opaque="",
    uri="sip:bob@biloxi.example.com", response="f44ab22f150c6a56071bce8"
   Content-Type: application/sdp
   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000

   /* Proxy 2 находит свои учетные данные и авторизует Алису,
	 пересылая INVITE Бобу. */

   F14 100 Trying Proxy 2 -> Proxy 1

   SIP/2.0 100 Trying
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK230f2.1
    ;received=192.0.2.111
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 3 INVITE
   Content-Length: 0


   F15 INVITE Proxy 2 -> Bob

   INVITE sip:bob@client.biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP ss2.biloxi.example.com:5060;branch=z9hG4bK31972.1
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK230f2.1
    ;received=192.0.2.111
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Max-Forwards: 68
   Record-Route: <sip:ss2.biloxi.example.com;lr>,
    <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 3 INVITE
   Contact: <sip:alice@client.atlanta.example.com>
   Content-Type: application/sdp
   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000

	 /* Боб немедленно отвечает на звонок */

   F16 200 OK Bob -> Proxy 2

   SIP/2.0 200 OK
   Via: SIP/2.0/UDP ss2.biloxi.example.com:5060;branch=z9hG4bK31972.1
    ;received=192.0.2.222
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK230f2.1
    ;received=192.0.2.111
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Record-Route: <sip:ss2.biloxi.example.com;lr>,
    <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=9103874
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 3 INVITE
   Contact: <sip:bob@client.biloxi.example.com>
   Content-Type: application/sdp
   Content-Length: 147

   v=0
   o=bob 2890844527 2890844527 IN IP4 client.biloxi.example.com
   s=-
   c=IN IP4 192.0.2.201
   t=0 0
   m=audio 3456 RTP/AVP 0
   a=rtpmap:0 PCMU/8000


   F17 200 OK Proxy 2 -> Proxy 1

   SIP/2.0 200 OK
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK230f2.1
    ;received=192.0.2.111
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Record-Route: <sip:ss2.biloxi.example.com;lr>,
    <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=9103874
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 3 INVITE
   Contact: <sip:bob@client.biloxi.example.com>
   Content-Type: application/sdp
   Content-Length: 147

   v=0
   o=bob 2890844527 2890844527 IN IP4 client.biloxi.example.com
   s=-

   c=IN IP4 192.0.2.201
   t=0 0
   m=audio 3456 RTP/AVP 0
   a=rtpmap:0 PCMU/8000


   F18 200 OK Proxy 1 -> Alice

   SIP/2.0 200 OK
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Record-Route: <sip:ss2.biloxi.example.com;lr>,
    <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=9103874
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 3 INVITE
   Contact: <sip:bob@client.biloxi.example.com>
   Content-Type: application/sdp
   Content-Length: 147

   v=0
   o=bob 2890844527 2890844527 IN IP4 client.biloxi.example.com
   s=-
   c=IN IP4 192.0.2.201
   t=0 0
   m=audio 3456 RTP/AVP 0
   a=rtpmap:0 PCMU/8000


   F19 ACK Alice -> Proxy 1

   ACK sip:bob@client.biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74b44
   Max-Forwards: 70
   Route: <sip:ss1.atlanta.example.com;lr>,
    <sip:ss2.biloxi.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=9103874
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 3 ACK
   Proxy-Authorization: Digest username="alice",
    realm="atlanta.example.com",
    nonce="wf84f1ceczx41ae6cbe5aea9c8e88d359", opaque="",
    uri="sip:bob@biloxi.example.com",
    response="42ce3cef44b22f50c6a6071bc8"
   Proxy-Authorization: Digest username="alice",
    realm="biloxi.example.com",

    nonce="c1e22c41ae6cbe5ae983a9c8e88d359", opaque="",
    uri="sip:bob@biloxi.example.com", response="f44ab22f150c6a56071bce8"
   Content-Length: 0


   F20 ACK Proxy 1 -> Proxy 2

   ACK sip:bob@client.biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK230f2.1
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74b44
    ;received=192.0.2.101
   Max-Forwards: 69
   Route: <sip:ss2.biloxi.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=9103874
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 3 ACK
   Contact: <sip:bob@client.biloxi.example.com>
   Proxy-Authorization: Digest username="alice",
    realm="biloxi.example.com",
    nonce="c1e22c41ae6cbe5ae983a9c8e88d359", opaque="",
    uri="sip:bob@biloxi.example.com", response="f44ab22f150c6a56071bce8"
   Content-Length: 0


   F21 ACK Proxy 2 -> Bob

   ACK sip:bob@client.biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP ss2.biloxi.example.com:5060;branch=z9hG4bK31972.1
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK230f2.1
    ;received=192.0.2.111
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74b44
    ;received=192.0.2.101
   Max-Forwards: 68
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=9103874
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 3 ACK
   Contact: <sip:bob@client.biloxi.example.com>
   Content-Length: 0
```

### 3.4. Успешный сеанс с отказом прокси-сервера

```
    Alice           Proxy 1          Proxy 2            Bob
      |                |                |                |
      |   INVITE F1    |                |                |
      |--------------->|                |                |
      |   INVITE F2    |                |                |
      |--------------->|                |                |
      |   INVITE F3    |                |                |
      |--------------->|                |                |
      |   INVITE F4    |                |                |
      |--------------->|                |                |
      |   INVITE F5    |                |                |
      |--------------->|                |                |
      |   INVITE F6    |                |                |
      |--------------->|                |                |
      |   INVITE F7    |                |                |
      |--------------->|                |                |
      |     INVITE F8                   |                |
      |-------------------------------->|                |
      |            407 F9               |                |
      |<--------------------------------|                |
      |             ACK F10             |                |
      |-------------------------------->|                |
      |           INVITE F11            |                |
      |-------------------------------->|   INVITE F12   |
      |             100  F13            |--------------->|
      |<--------------------------------|                |
      |                                 |     180 F14    |
      |             180 F15             |<---------------|
      |<--------------------------------|                |
      |                                 |     200 F16    |
      |             200 F17             |<---------------|
      |<--------------------------------|                |
      |             ACK F18             |                |
      |-------------------------------->|     ACK F19    |
      |                                 |--------------->|
      |                Both Way RTP Media                |
      |<================================================>|
      |                                 |     BYE F20    |
      |             BYE F21             |<---------------|
      |<--------------------------------|                |
      |             200 F22             |                |
      |-------------------------------->|     200 F23    |
      |                                 |--------------->|
      |                                 |                |

```

В этом сценарии Alice выполняет вызов Боба через прокси-сервер. Alice настроена для первичного прокси-сервера SIP `Proxy 1` и вторичного прокси-сервера SIP `Proxy 2` (или может использовать записи DNS SRV для определения местоположения `Proxy 1` и `Proxy 2`). У Алисы есть действительные учетные данные для обоих доменов. `Proxy 1` не обслуживается и не отвечает на сообщения `INVITE` (доступен, но не отвечает). Затем Alice завершает вызов Боба, используя `Proxy 2`.

Детали сообщения:

```
   F1 INVITE Alice -> Proxy 1

   INVITE sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK465b6d
   Max-Forwards: 70
   Route: <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:alice@client.atlanta.example.com>
   Content-Type: application/sdp
   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000


   F2 INVITE Alice -> Proxy 1

   Same as Message F1


   F3 INVITE Alice -> Proxy 1

   Same as Message F1


   F4 INVITE Alice -> Proxy 1

   Same as Message F1

   F5 INVITE Alice -> Proxy 1

   Same as Message F1


   F6 INVITE Alice -> Proxy 1

   Same as Message F1


   F7 INVITE Alice -> Proxy 1

   Same as Message F1

   /* Алиса отказывается от не отвечающего прокси-сервера */

   F8 INVITE Alice -> Proxy 2

   INVITE sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74b8a
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 4Fde34wkd11wsGFDs3@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:alice@client.atlanta.example.com>
   Content-Type: application/sdp
   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000

	 /* Proxy 2 вызывает Алису для аутентификации */


   F9 407 Proxy Authorization Required Proxy 2 -> Alice

   SIP/2.0 407 Proxy Authorization Required
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74b8a
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=2421452

   Call-ID: 4Fde34wkd11wsGFDs3@atlanta.example.com
   CSeq: 1 INVITE
   Proxy-Authenticate: Digest realm="biloxi.example.com", qop="auth",
    nonce="1ae6cbe5ea9c8e8df84fqnlec434a359",
    opaque="", stale=FALSE, algorithm=MD5
   Content-Length: 0


   F10 ACK Alice -> Proxy 2

   ACK sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74b8a
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=2421452
   Call-ID: 4Fde34wkd11wsGFDs3@atlanta.example.com
   CSeq: 1 ACK
   Content-Length: 0

   /* Алиса отвечает повторной отправкой INVITE с данными
	 аутентификации.  */


   F11 INVITE Alice -> Proxy 2

   INVITE sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 4Fde34wkd11wsGFDs3@atlanta.example.com
   CSeq: 2 INVITE
   Contact: <sip:alice@client.atlanta.example.com>
   Proxy-Authorization: Digest username="alice",
    realm="biloxi.example.com",
    nonce="1ae6cbe5ea9c8e8df84fqnlec434a359", opaque="",
    uri="sip:bob@biloxi.example.com",
    response="8a880c919d1a52f20a1593e228adf599"
   Content-Type: application/sdp
   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000

	 /* Прокси 2 принимает учетные данные и пересылает INVITE Бобу.
   Клиент Алисы готовится к приему данных на порт 49172 из сети.
   */


   F12 INVITE Proxy 2 -> Bob

   INVITE sip:bob@client.biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP ss2.biloxi.example.com:5060;branch=z9hG4bK721e4.1
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Max-Forwards: 69
   Record-Route: <sip:ss2.biloxi.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 4Fde34wkd11wsGFDs3@atlanta.example.com
   CSeq: 2 INVITE
   Contact: <sip:alice@client.atlanta.example.com>
   Content-Type: application/sdp
   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000


   F13 100 Trying Proxy 2 -> Alice

   SIP/2.0 100 Trying
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 4Fde34wkd11wsGFDs3@atlanta.example.com
   CSeq: 2 INVITE
   Content-Length: 0


   F14 180 Ringing Bob -> Proxy 2

   SIP/2.0 180 Ringing
   Via: SIP/2.0/UDP ss2.biloxi.example.com:5060;branch=z9hG4bK721e4.1
    ;received=192.0.2.222

   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Record-Route: <sip:ss2.biloxi.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 4Fde34wkd11wsGFDs3@atlanta.example.com
   CSeq: 2 INVITE
   Contact: <sip:bob@client.biloxi.example.com>
   Content-Length: 0


   F15 180 Ringing Proxy 2 -> Alice

   SIP/2.0 180 Ringing
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Record-Route: <sip:ss2.biloxi.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 4Fde34wkd11wsGFDs3@atlanta.example.com
   CSeq: 2 INVITE
   Contact: <sip:bob@client.biloxi.example.com>
   Content-Length: 0


   F16 200 OK Bob -> Proxy 2

   SIP/2.0 200 OK
   Via: SIP/2.0/UDP ss2.biloxi.example.com:5060;branch=z9hG4bK721e4.1
    ;received=192.0.2.222
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Record-Route: <sip:ss2.biloxi.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 4Fde34wkd11wsGFDs3@atlanta.example.com
   CSeq: 2 INVITE
   Contact: <sip:bob@client.biloxi.example.com>
   Content-Type: application/sdp
   Content-Length: 147

   v=0
   o=bob 2890844527 2890844527 IN IP4 client.biloxi.example.com
   s=-
   c=IN IP4 192.0.2.201
   t=0 0
   m=audio 3456 RTP/AVP 0
   a=rtpmap:0 PCMU/8000

   F17 200 OK Proxy 2 -> Alice

   SIP/2.0 200 OK
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Record-Route: <sip:ss2.biloxi.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 4Fde34wkd11wsGFDs3@atlanta.example.com
   CSeq: 2 INVITE
   Contact: <sip:bob@client.biloxi.example.com>
   Content-Type: application/sdp
   Content-Length: 147

   v=0
   o=bob 2890844527 2890844527 IN IP4 client.biloxi.example.com
   s=-
   c=IN IP4 192.0.2.201
   t=0 0
   m=audio 3456 RTP/AVP 0
   a=rtpmap:0 PCMU/8000


   F18 ACK Alice -> Proxy 2

   ACK sip:bob@client.biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74b8g
   Max-Forwards: 70
   Route: <sip:ss2.biloxi.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 4Fde34wkd11wsGFDs3@atlanta.example.com
   CSeq: 2 ACK
   Content-Length: 0


   F19 ACK Proxy 2 -> Bob

   ACK sip:bob@client.biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP ss2.biloxi.example.com:5060;branch=z9hG4bK721e4.1
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74b8g
    ;received=192.0.2.101
   Max-Forwards: 69
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 4Fde34wkd11wsGFDs3@atlanta.example.com

   CSeq: 2 ACK
   Content-Length: 0

	 /* Между Алисой и Бобом устанавливаются потоки RTP */

   /* Боб завершает вызов с Алисой. */


   F20 BYE Bob -> Proxy 2

   BYE sip:alice@client.atlanta.example.com SIP/2.0
   Via: SIP/2.0/UDP client.biloxi.example.com:5060;branch=z9hG4bKnashds7
   Max-Forwards: 70
   Route: <sip:ss2.biloxi.example.com;lr>
   From: Bob <sip:bob@biloxi.example.com>;tag=314159
   To: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   Call-ID: 4Fde34wkd11wsGFDs3@atlanta.example.com
   CSeq: 1 BYE
   Content-Length: 0


   F21 BYE Proxy 2 -> Alice

   BYE sip:alice@client.atlanta.example.com SIP/2.0
   Via: SIP/2.0/UDP ss2.biloxi.example.com:5060;branch=z9hG4bK721e4.1
   Via: SIP/2.0/UDP client.biloxi.example.com:5060;branch=z9hG4bKnashds7
    ;received=192.0.2.201
   Max-Forwards: 69
   From: Bob <sip:bob@biloxi.example.com>;tag=314159
   To: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   Call-ID: 4Fde34wkd11wsGFDs3@atlanta.example.com
   CSeq: 1 BYE
   Content-Length: 0


   F22 200 OK Alice -> Proxy 2

   SIP/2.0 200 OK
   Via: SIP/2.0/UDP ss2.biloxi.example.com:5060;branch=z9hG4bK721e4.1
    ;received=192.0.2.222
   Via: SIP/2.0/UDP client.biloxi.example.com:5060;branch=z9hG4bKnashds7
    ;received=192.0.2.201
   From: Bob <sip:bob@biloxi.example.com>;tag=314159
   To: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   Call-ID: 4Fde34wkd11wsGFDs3@atlanta.example.com
   CSeq: 1 BYE
   Content-Length: 0

   F23 200 OK Proxy 2 -> Bob

   SIP/2.0 200 OK
   Via: SIP/2.0/UDP client.biloxi.example.com:5060;branch=z9hG4bKnashds7
    ;received=192.0.2.201
   From: Bob <sip:bob@biloxi.example.com>;tag=314159
   To: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   Call-ID: 4Fde34wkd11wsGFDs3@atlanta.example.com
   CSeq: 1 BYE
   Content-Length: 0
```

### 3.5. Сеанс через SIP ALG

```
   Alice             ALG           Proxy 2            Bob
     |                |                |                |
     |   INVITE F1    |                |                |
     |--------------->|   INVITE F2    |                |
     |     100 F3     |--------------->|   INVITE F4    |
     |<---------------|     100 F5     |--------------->|
     |                |<---------------|      180 F6    |
     |                |     180 F7     |<---------------|
     |     180 F8     |<---------------|                |
     |<---------------|                |      200 F9    |
     |                |    200 F10     |<---------------|
     |     200 F11    |<---------------|                |
     |<---------------|                                 |
     |     ACK F12    |                                 |
     |--------------->|             ACK F13             |
     |                |-------------------------------->|
     |    RTP Media   |        Both Way RTP Media       |
     |<==============>|<===============================>|
     |     BYE F14    |                                 |
     |--------------->|             BYE F15             |
     |                |-------------------------------->|
     |                |             200 F16             |
     |     200 F17    |<--------------------------------|
     |<---------------|                                 |
     |                |                                 |
```

Alice совершает вызов Bob через ALG (шлюз прикладного уровня) и SIP-прокси. Маршрутизация через ALG выполняется с помощью предварительно загруженного заголовка `Route` в `INVITE` F1. Обратите внимание, что настройка медиапотока не является сквозной - ALG терминирует оба медиапотока и соединяет их. Это выполняется ALG изменением SDP в `INVITE` (F1) и сообщением `200 ОК` (F10), и, возможно, любым 18-кратным сообщением `ACK`, содержащих SDP.

В дополнение к обходу брандмауэра этот агент пользователя back-to-Back (B2BUA) может использоваться как часть службы анонимайзера (в которой вся идентифицирующая информация об Alice будет удалена) или для выполнения преобразования кодека мультимедиа, такого как преобразование mu-law в A-law PCM при международном вызове.

Также обратите внимание, что `Proxy 2` не записывает Record-Route в этом потоке вызовов.

Детали сообщения:

```
   F1 INVITE Alice -> SIP ALG

   INVITE sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:alice@client.atlanta.example.com>
   Route: <sip:alg1.atlanta.example.com;lr>
   Proxy-Authorization: Digest username="alice",
    realm="biloxi.example.com",
    nonce="85b4f1cen4341ae6cbe5a3a9c8e88df9", opaque="",
    uri="sip:bob@biloxi.example.com",
    response="b3f392f9218a328b9294076d708e6815"
   Content-Type: application/sdp
   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000

   /* Клиент Алисы готовится к приему данных на порт 49172 из сети. */


   F2 INVITE SIP ALG -> Proxy 2

   INVITE sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP alg1.atlanta.example.com:5060;branch=z9hG4bK739578.1
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Max-Forwards: 69
   Record-Route: <sip:alg1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:alice@client.atlanta.example.com>
   Proxy-Authorization: Digest username="alice",
    realm="biloxi.example.com",
    nonce="85b4f1cen4341ae6cbe5a3a9c8e88df9", opaque="",
    uri="sip:bob@biloxi.example.com",
    response="b3f392f9218a328b9294076d708e6815"
   Content-Type: application/sdp
   Content-Length: 150

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.128
   t=0 0
   m=audio 2000 RTP/AVP 0
   a=rtpmap:0 PCMU/8000


   F3 100 Trying SIP ALG -> Alice

   SIP/2.0 100 Trying
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Content-Length: 0

   /* SIP ALG prepares to proxy data from port 192.0.2.128/2000 to
   192.0.2.101/49172.   Proxy 2 uses a Location Service function to
   determine where Bob is located. Based upon location analysis the call
   is forwarded to Bob */


   F4 INVITE Proxy 2 -> Bob

   INVITE sip:bob@client.biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP ss2.biloxi.example.com:5060;branch=z9hG4bK2d4790.1
   Via: SIP/2.0/UDP alg1.atlanta.example.com:5060;branch=z9hG4bK739578.1
    ;received=192.0.2.128
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Max-Forwards: 68
   Record-Route: <sip:alg1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:alice@client.atlanta.example.com>
   Content-Type: application/sdp

   Content-Length: 150

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.128
   t=0 0
   m=audio 2000 RTP/AVP 0
   a=rtpmap:0 PCMU/8000


   F5 100 Trying Proxy 2 -> SIP ALG

   SIP/2.0 100 Trying
   Via: SIP/2.0/UDP alg1.atlanta.example.com:5060;branch=z9hG4bK739578.1
    ;received=192.0.2.128
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Content-Length: 0


   F6 180 Ringing Bob -> Proxy 2

   SIP/2.0 180 Ringing
   Via: SIP/2.0/UDP ss2.biloxi.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.222
   Via: SIP/2.0/UDP alg1.atlanta.example.com:5060;branch=z9hG4bK739578.1
    ;received=192.0.2.128
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Record-Route: <sip:alg1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:bob@client.biloxi.example.com>
   Content-Length: 0


   F7 180 Ringing Proxy 2 -> SIP ALG

   SIP/2.0 180 Ringing
   Via: SIP/2.0/UDP alg1.atlanta.example.com:5060;branch=z9hG4bK739578.1
    ;received=192.0.2.128
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Record-Route: <sip:alg1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:bob@client.biloxi.example.com>
   Content-Length: 0


   F8 180 Ringing SIP ALG -> Alice

   SIP/2.0 180 Ringing
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Record-Route: <sip:alg1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:bob@client.biloxi.example.com>
   Content-Length: 0


   F9 200 OK Bob -> Proxy 2

   SIP/2.0 200 OK
   Via: SIP/2.0/UDP ss2.biloxi.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.222
   Via: SIP/2.0/UDP alg1.atlanta.example.com:5060;branch=z9hG4bK739578.1
    ;received=192.0.2.128
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Record-Route: <sip:alg1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:bob@client.biloxi.example.com>
   Content-Type: application/sdp
   Content-Length: 147

   v=0
   o=bob 2890844527 2890844527 IN IP4 client.biloxi.example.com
   s=-
   c=IN IP4 192.0.2.201
   t=0 0
   m=audio 3456 RTP/AVP 0
   a=rtpmap:0 PCMU/8000


   F10 200 OK Proxy 2 -> SIP ALG

   SIP/2.0 200 OK
   Via: SIP/2.0/UDP alg1.atlanta.example.com:5060;branch=z9hG4bK739578.1
    ;received=192.0.2.128
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Record-Route: <sip:alg1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:bob@client.biloxi.example.com>
   Content-Type: application/sdp
   Content-Length: 147

   v=0
   o=bob 2890844527 2890844527 IN IP4 client.biloxi.example.com
   s=-
   c=IN IP4 192.0.2.201
   t=0 0
   m=audio 3456 RTP/AVP 0
   a=rtpmap:0 PCMU/8000


   F11 200 OK SIP ALG -> Alice

   SIP/2.0 200 OK
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Record-Route: <sip:alg1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:bob@client.biloxi.example.com>
   Content-Type: application/sdp
   Content-Length: 147

   v=0
   o=bob 2890844527 2890844527 IN IP4 client.biloxi.example.com
   s=-
   c=IN IP4 192.0.2.128
   t=0 0
   m=audio 1734 RTP/AVP 0
   a=rtpmap:0 PCMU/8000

	 /* ALG готовится проксировать пакеты от 192.0.2.128/
      1734 до 192.0.2.201/3456 */


   F12 ACK Alice -> SIP ALG

   ACK sip:bob@client.biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bhh
   Max-Forwards: 70
   Route: <sip:alg1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 ACK
   Content-Length: 0


   F13 ACK SIP ALG -> Bob

   ACK sip:bob@client.biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP alg1.atlanta.example.com:5060;branch=z9hG4bK739578.1
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bhh
    ;received=192.0.2.101
   Max-Forwards: 69
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 ACK
   Content-Length: 0

	 /* Потоки RTP устанавливаются между Алисой и ALG и
   между ALG и Bob */

   /* Алиса завершает вызов с Бобом. */


   F14 BYE Alice -> SIP ALG

   BYE sip:bob@client.biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74be5
   Max-Forwards: 70
   Route: <sip:alg1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 2 BYE
   Content-Length: 0


   F15 BYE SIP ALG -> Bob

   BYE sip:bob@client.biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP alg1.atlanta.example.com:5060;branch=z9hG4bK739578.1
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74be5
    ;received=192.0.2.101
   Max-Forwards: 69
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 2 BYE
   Content-Length: 0


   F16 200 OK Bob -> SIP ALG

   SIP/2.0 200 OK
   Via: SIP/2.0/UDP alg1.atlanta.example.com:5060;branch=z9hG4bK739578.1
    ;received=192.0.2.128
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74be5
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 2 BYE
   Content-Length: 0


   F17 200 OK SIP ALG -> Alice

   SIP/2.0 200 OK
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74be5
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 2 BYE
   Content-Length: 0
```

### 3.6. Сеанс через редирект и прокси-серверы с SDP в ACK
```
   Alice        Redirect Server     Proxy 3             Bob
     |                |                |                |
     |   INVITE F1    |                |                |
     |--------------->|                |                |
     |     302 F2     |                |                |
     |<---------------|                |                |
     |     ACK F3     |                |                |
     |--------------->|                |                |
     |     INVITE F4                   |                |
     |-------------------------------->|    INVITE F5   |
     |             100  F6             |--------------->|
     |<--------------------------------|      180 F7    |
     |             180 F8              |<---------------|
     |<--------------------------------|                |
     |                                 |     200 F9     |
     |             200 F10             |<---------------|
     |<--------------------------------|                |
     |             ACK F11             |                |
     |-------------------------------->|     ACK F12    |
     |                                 |--------------->|
     |                Both Way RTP Media                |
     |<================================================>|
     |                                 |     BYE F13    |
     |             BYE F14             |<---------------|
     |<--------------------------------|                |
     |             200 F15             |                |
     |-------------------------------->|     200 F16    |
     |                                 |--------------->|
     |                                 |                |
```

В этом сценарии Алиса совершает звонок Бобу, используя сначала сервер перенаправления, а затем прокси-сервер. Сообщение `INVITE` сначала отправляется на сервер перенаправления. Сервер возвращает ответ `302 Moved Temporarily (F2)`, содержащий заголовок `Contact` с текущим SIP-адресом Боба. Затем Алиса генерирует новый `INVITE` и отправляет его Бобу через прокси-сервер, и звонок проходит нормально.  В этом примере в `INVITE` нет SDP, поэтому SDP передается в сообщении `ACK`.

Вызов завершается, когда Боб посылает сообщение `BYE`.

Детали сообщения:

```
   F1 INVITE Alice -> Redirect Server

   INVITE sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bKbf9f44
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:alice@client.atlanta.example.com>
   Content-Length: 0


   F2 302 Moved Temporarily Redirect Proxy -> Alice

   SIP/2.0 302 Moved Temporarily
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bKbf9f44
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=53fHlqlQ2
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:bob@chicago.example.com;transport=tcp>
   Content-Length: 0


   F3 ACK Alice -> Redirect Server

   ACK sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bKbf9f44
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=53fHlqlQ2
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 ACK
   Content-Length: 0


   F4 INVITE Alice -> Proxy 3

   INVITE sip:bob@chicago.example.com SIP/2.0
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com

   CSeq: 2 INVITE
   Contact: <sip:alice@client.atlanta.example.com;transport=tcp>
   Content-Length: 0


   F5 INVITE Proxy 3 -> Bob

   INVITE sip:bob@client.chicago.example.com SIP/2.0
   Via: SIP/2.0/TCP ss3.chicago.example.com:5060;branch=z9hG4bK721e.1
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Max-Forwards: 69
   Record-Route: <sip:ss3.chicago.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 2 INVITE
   Contact: <sip:alice@client.atlanta.example.com;transport=tcp>
   Content-Length: 0


   F6 100 Trying Proxy 3 -> Alice

   SIP/2.0 100 Trying
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 2 INVITE
   Content-Length: 0


   F7 180 Ringing Bob -> Proxy 3

   SIP/2.0 180 Ringing
   Via: SIP/2.0/TCP ss3.chicago.example.com:5060;branch=z9hG4bK721e.1
    ;received=192.0.2.233
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Record-Route: <sip:ss3.chicago.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 2 INVITE
   Contact: <sip:bob@client.chicago.example.com;transport=tcp>
   Content-Length: 0

   F8 180 Ringing Proxy 3 -> Alice

   SIP/2.0 180 Ringing
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Record-Route: <sip:ss3.chicago.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 2 INVITE
   Contact: <sip:bob@client.chicago.example.com;transport=tcp>
   Content-Length: 0


   F9 200 OK Bob -> Proxy 3

   SIP/2.0 200 OK
   Via: SIP/2.0/TCP ss3.chicago.example.com:5060;branch=z9hG4bK721e.1
    ;received=192.0.2.233
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Record-Route: <sip:ss3.chicago.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 2 INVITE
   Contact: <sip:bob@client.chicago.example.com;transport=tcp>
   Content-Type: application/sdp
   Content-Length: 148

   v=0
   o=bob 2890844527 2890844527 IN IP4 client.chicago.example.com
   s=-
   c=IN IP4 192.0.2.100
   t=0 0
   m=audio 3456 RTP/AVP 0
   a=rtpmap:0 PCMU/8000


   F10 200 OK Proxy -> Alice

   SIP/2.0 200 OK
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Record-Route: <sip:ss3.chicago.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 2 INVITE
   Contact: <sip:bob@client.chicago.example.com;transport=tcp>
   Content-Type: application/sdp
   Content-Length: 148

   v=0
   o=bob 2890844527 2890844527 IN IP4 client.chicago.example.com
   s=-
   c=IN IP4 192.0.2.100
   t=0 0
   m=audio 3456 RTP/AVP 0
   a=rtpmap:0 PCMU/8000

	 /* ACK содержит SDP Алисы, поскольку в INVITE его нет */


   F11 ACK Alice -> Proxy 3

   ACK sip:bob@client.chicago.example.com SIP/2.0
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bq9
   Max-Forwards: 70
   Route: <sip:ss3.chicago.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 2 ACK
   Content-Type: application/sdp
   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000


   F12 ACK Proxy 3 -> Bob

   ACK sip:bob@client.chicago.example.com SIP/2.0
   Via: SIP/2.0/TCP ss3.chicago.example.com:5060;branch=z9hG4bK721e.1
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bq9
    ;received=192.0.2.101
   Max-Forwards: 69
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 2 ACK
   Content-Type: application/sdp
   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000

	 /* Между Алисой и Бобом устанавливаются потоки RTP */

   /* Боб завершает вызов с Алисой. */


   F13 BYE Bob -> Proxy 3

   BYE sip:alice@client.atlanta.example.com SIP/2.0
   Via: SIP/2.0/TCP client.chicago.example.com:5060;branch=z9hG4bKfgaw2
   Max-Forwards: 70
   Route: <sip:ss3.chicago.example.com;lr>
   From: Bob <sip:bob@biloxi.example.com>;tag=314159
   To: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 BYE
   Content-Length: 0


   F14 BYE Proxy 3 -> Alice

   BYE sip:alice@client.atlanta.example.com SIP/2.0
   Via: SIP/2.0/TCP ss3.chicago.example.com:5060;branch=z9hG4bK721e.1
    ;received=192.0.2.100
   Via: SIP/2.0/TCP client.chicago.example.com:5060;branch=z9hG4bKfgaw2
   Max-Forwards: 69
   From: Bob <sip:bob@biloxi.example.com>;tag=314159
   To: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 BYE
   Content-Length: 0


   F15 200 OK Alice -> Proxy 3

   SIP/2.0 200 OK

   Via: SIP/2.0/TCP ss3.chicago.example.com:5060;branch=z9hG4bK721e.1
    ;received=192.0.2.233
   Via: SIP/2.0/TCP client.chicago.example.com:5060;branch=z9hG4bKfgaw2
    ;received=192.0.2.100
   From: Bob <sip:bob@biloxi.example.com>;tag=314159
   To: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 BYE
   Content-Length: 0


   F16 200 OK Proxy 3 -> Bob

   SIP/2.0 200 OK
   Via: SIP/2.0/TCP client.chicago.example.com:5060;branch=z9hG4bKfgaw2
    ;received=192.0.2.100
   From: Bob <sip:bob@biloxi.example.com>;tag=314159
   To: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 BYE
   Content-Length: 0
```

### 3.7. Сеанс с re-INVITE (Смена IP-адреса)

```
     Alice                Proxy 2                Bob
        |   F1 INVITE        |                    |
        |------------------->|      F2 INVITE     |
        |   F3 100 Trying    |------------------->|
        |<-------------------|   F4 180 Ringing   |
        |   F5 180 Ringing   |<-------------------|
        |<-------------------|                    |
        |                    |    F6 200 OK       |
        |    F7 200 OK       |<-------------------|
        |<-------------------|                    |
        |                 F8  ACK                 |
        |---------------------------------------->|
        |      Both Way RTP Media Established     |
        |<=======================================>|
        |                                         |
        |           Bob changes IP address        |
        |                                         |
        |                 F9 INVITE               |
        |<----------------------------------------|
        |                F10 200 OK               |
        |---------------------------------------->|
        |                 F11  ACK                |
        |<----------------------------------------|
        |         New RTP Media Stream            |
        |<=======================================>|
        |                 F12 BYE                 |
        |---------------------------------------->|
        |               F13 200 OK                |
        |<----------------------------------------|
        |                                         |
```

В этом примере показана сессия, в которой медианоситель меняется в середине сессии. Когда IP-адрес Боба меняется во время сессии, Боб посылает повторный `INVITE`, содержащий новый контакт и информацию SDP (номер версии увеличивается) на A. В этом потоке прокси не записывает маршрутизацию, поэтому не находится в пути обмена сообщениями SIP после первоначального обмена.

Детали сообщения:

```
   F1 INVITE Alice -> Proxy 2

   INVITE sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:alice@client.atlanta.example.com>
   Content-Type: application/sdp
   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000


   F2 INVITE Proxy 2 -> Bob

   INVITE sip:bob@client.biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP ss2.biloxi.example.com:5060;branch=z9hG4bK2d4790.1
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Max-Forwards: 69
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:alice@client.atlanta.example.com>
   Content-Type: application/sdp
   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000

   F3 100 Trying Proxy 2 -> Alice

   SIP/2.0 100 Trying
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Content-Length: 0


   F4 180 Ringing Bob -> Proxy 2

   SIP/2.0 180 Ringing
   Via: SIP/2.0/UDP ss2.biloxi.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.222
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:bob@client.biloxi.example.com>
   Content-Length: 0


   F5 180 Ringing Proxy 2 -> Alice

   SIP/2.0 180 Ringing
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:bob@client.biloxi.example.com>
   Content-Length: 0


   F6 200 OK Bob -> Proxy 2

   SIP/2.0 200 OK
   Via: SIP/2.0/UDP ss2.biloxi.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.222
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:bob@client.biloxi.example.com>
   Content-Type: application/sdp
   Content-Length: 147

   v=0
   o=bob 2890844527 2890844527 IN IP4 client.biloxi.example.com
   s=-
   c=IN IP4 192.0.2.201
   t=0 0
   m=audio 3456 RTP/AVP 0
   a=rtpmap:0 PCMU/8000


   F7 200 OK Proxy 2 -> Alice

   SIP/2.0 200 OK
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:bob@client.biloxi.example.com>
   Content-Type: application/sdp
   Content-Length: 147

   v=0
   o=bob 2890844527 2890844527 IN IP4 client.biloxi.example.com
   s=-
   c=IN IP4 192.0.2.201
   t=0 0
   m=audio 3456 RTP/AVP 0
   a=rtpmap:0 PCMU/8000


   F8 ACK Alice -> Bob

   ACK sip:bob@client.biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74b7b
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 ACK
   Content-Length: 0

	 /* Между Алисой и Бобом устанавливаются потоки RTP */

   /* Боб меняет IP-адрес и шлёт re-INVITE Алисе с новым контактом и SDP */


   F9 INVITE Bob -> Alice

   INVITE sip:alice@client.atlanta.example.com SIP/2.0
   Via: SIP/2.0/UDP client.chicago.example.com:5060;branch=z9hG4bKlkld5l
   Max-Forwards: 70
   From: Bob <sip:bob@biloxi.example.com>;tag=314159
   To: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 14 INVITE
   Contact: <sip:bob@client.chicago.example.com>
   Content-Type: application/sdp
   Content-Length: 149

   v=0
   o=bob 2890844527 2890844528 IN IP4 client.chicago.example.com
   s=-
   c=IN IP4 192.0.2.100
   t=0 0
   m=audio 47172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000


   F10 200 OK Alice -> Bob

   SIP/2.0 200 OK
   Via: SIP/2.0/UDP client.chicago.example.com:5060;branch=z9hG4bKlkld5l
    ;received=192.0.2.100
   Max-Forwards: 70
   From: Bob <sip:bob@biloxi.example.com>;tag=314159
   To: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 14 INVITE
   Contact: <sip:alice@client.atlanta.example.com>
   Content-Type: application/sdp
   Content-Length: 150

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 1000 RTP/AVP 0
   a=rtpmap:0 PCMU/8000


   F11 ACK Bob -> Alice

   ACK sip:alice@client.atlanta.example.com SIP/2.0
   Via: SIP/2.0/UDP client.chicago.example.com:5060;branch=z9hG4bKlkldcc
   Max-Forwards: 70
   From: Bob <sip:bob@biloxi.example.com>;tag=314159
   To: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 14 ACK
   Content-Length: 0

	 /* Новый поток RTP установлен между Алисой и Бобом */

   /* Алиса завершает вызов с Бобом */


   F12 BYE Alice -> Bob

   BYE sip:bob@client.chicago.example.com SIP/2.0
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bo4
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 2 BYE
   Content-Length: 0


   F13 200 OK Bob -> Alice

   SIP/2.0 200 OK
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bo4
    ;received=192.0.2.101
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 2 BYE
   Content-Length: 0
```

### 3.8. Неудачно - нет ответа

```
   Alice           Proxy 1          Proxy 2            Bob
     |                |                |                |
     |   INVITE F1    |                |                |
     |--------------->|   INVITE F2    |                |
     |     100  F3    |--------------->|   INVITE F4    |
     |<---------------|     100  F5    |--------------->|
     |                |<---------------|                |
     |                |                |      180 F6    |
     |                |     180 F7     |<---------------|
     |     180 F8     |<---------------|                |
     |<---------------|                |                |
     |   CANCEL F9    |                |                |
     |--------------->|                |                |
     |     200 F10    |                |                |
     |<---------------|   CANCEL F11   |                |
     |                |--------------->|                |
     |                |     200 F12    |                |
     |                |<---------------|                |
     |                |                |   CANCEL F13   |
     |                |                |--------------->|
     |                |                |     200 F14    |
     |                |                |<---------------|
     |                |                |     487 F15    |
     |                |                |<---------------|
     |                |                |     ACK F16    |
     |                |     487 F17    |--------------->|
     |                |<---------------|                |
     |                |     ACK F18    |                |
     |     487 F19    |--------------->|                |
     |<---------------|                |                |
     |     ACK F20    |                |                |
     |--------------->|                |                |
     |                |                |                |
```

В этом сценарии Алиса прекращает разговор до того, как Боб ответит (отправит ответ `200 OK`).  Алиса посылает `CANCEL (F9)`, поскольку от Боба не было получено окончательного ответа. Если бы `200 OK` на `INVITE` пересекся с `CANCEL`, Алиса отправила бы `ACK`, а затем `BYE` Бобу, чтобы правильно завершить вызов.

Обратите внимание, что сообщение `CANCEL` подтверждается сообщением `200 OK` по принципу hop-by-hop, а не end-to-end.

Детали сообщения:

```
   F1 INVITE Alice -> Proxy 1

   INVITE sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
   Max-Forwards: 70
   Route: <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:alice@client.atlanta.example.com>
   Proxy-Authorization: Digest username="alice",
    realm="atlanta.example.com",
    nonce="ze7k1ee88df84f1cec431ae6cbe5a359", opaque="",
    uri="sip:bob@biloxi.example.com",
    response="b00b416324679d7e243f55708d44be7b"
   Content-Type: application/sdp
   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000

   /* Клиент Алисы готовится к приему данных на порт 49172 из сети. */


   F2 INVITE Proxy 1 -> Proxy 2

   INVITE sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Max-Forwards: 69
   Record-Route: <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:alice@client.atlanta.example.com>
   Content-Type: application/sdp
   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000


   F3 100 Trying Proxy 1 -> Alice

   SIP/2.0 100 Trying
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Content-Length: 0


   F4 INVITE Proxy 2 -> Bob

   INVITE sip:bob@client.biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP ss2.biloxi.example.com:5060;branch=z9hG4bK721e4.1
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.111
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Record-Route: <sip:ss2.biloxi.example.com;lr>,
    <sip:ss1.atlanta.example.com;lr>
   Max-Forwards: 68
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:alice@client.atlanta.example.com>
   Content-Type: application/sdp
   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000

   F5 100 Trying Proxy 2 -> Proxy 1

   SIP/2.0 100 Trying
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.111
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Content-Length: 0


   F6 180 Ringing Bob -> Proxy 2

   SIP/2.0 180 Ringing
   Via: SIP/2.0/UDP ss2.biloxi.example.com:5060;branch=z9hG4bK721e4.1
    ;received=192.0.2.222
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.111
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Record-Route: <sip:ss2.biloxi.example.com;lr>,
    <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:bob@client.biloxi.example.com>
   Content-Length: 0


   F7 180 Ringing Proxy 2 -> Proxy 1

   SIP/2.0 180 Ringing
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.111
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Record-Route: <sip:ss2.biloxi.example.com;lr>,
    <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE

   Contact: <sip:bob@client.biloxi.example.com>
   Content-Length: 0


   F8 180 Ringing Proxy 1 -> Alice

   SIP/2.0 180 Ringing
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Record-Route: <sip:ss2.biloxi.example.com;lr>,
    <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:bob@client.biloxi.example.com>
   Content-Length: 0


   F9 CANCEL Alice -> Proxy 1

   CANCEL sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Route: <sip:ss1.atlanta.example.com;lr>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 CANCEL
   Content-Length: 0


   F10 200 OK Proxy 1 -> Alice

   SIP/2.0 200 OK
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 CANCEL
   Content-Length: 0


   F11 CANCEL Proxy 1 -> Proxy 2

   CANCEL sip:alice@atlanta.example.com SIP/2.0
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 CANCEL
   Content-Length: 0


   F12 200 OK Proxy 2 -> Proxy 1

   SIP/2.0 200 OK
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.111
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 CANCEL
   Content-Length: 0


   F13 CANCEL Proxy 2 -> Bob

   CANCEL sip:bob@client.biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP ss2.biloxi.example.com:5060;branch=z9hG4bK721e4.1
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 CANCEL
   Content-Length: 0


   F14 200 OK Bob -> Proxy 2

   SIP/2.0 200 OK
   Via: SIP/2.0/UDP ss2.biloxi.example.com:5060;branch=z9hG4bK721e4.1
    ;received=192.0.2.222
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 CANCEL
   Content-Length: 0


   F15 487 Request Terminated Bob -> Proxy 2

   SIP/2.0 487 Request Terminated
   Via: SIP/2.0/UDP ss2.biloxi.example.com:5060;branch=z9hG4bK721e4.1
    ;received=192.0.2.222
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.111
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Content-Length: 0


   F16 ACK Proxy 2 -> Bob

   ACK sip:bob@client.biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP ss2.biloxi.example.com:5060;branch=z9hG4bK721e4.1
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 ACK
   Content-Length: 0


   F17 487 Request Terminated Proxy 2 -> Proxy 1

   SIP/2.0 487 Request Terminated
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.111
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Content-Length: 0


   F18 ACK Proxy 1 -> Proxy 2

   ACK sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP ss2.biloxi.example.com:5060;branch=z9hG4bK721e4.1
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 ACK
   Content-Length: 0


   F19 487 Request Terminated Proxy 1 -> Alice

   SIP/2.0 487 Request Terminated
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE


   F20 ACK Alice -> Proxy 1

   ACK sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   Proxy-Authorization: Digest username="alice",
    realm="atlanta.example.com",
    nonce="ze7k1ee88df84f1cec431ae6cbe5a359", opaque="",
    uri="sip:bob@biloxi.example.com",
    response="b00b416324679d7e243f55708d44be7b"
   CSeq: 1 ACK
   Content-Length: 0
```

### 3.9.  Неудачно - занято

```
   Alice           Proxy 1          Proxy 2            Bob
     |                |                |                |
     |   INVITE F1    |                |                |
     |--------------->|   INVITE F2    |                |
     |     100  F3    |--------------->|   INVITE F4    |
     |<---------------|     100  F5    |--------------->|
     |                |<---------------|                |
     |                |                |      486 F6    |
     |                |                |<---------------|
     |                |                |     ACK F7     |
     |                |      486 F8    |--------------->|
     |                |<---------------|                |
     |                |      ACK F9    |                |
     |     486 F10    |--------------->|                |
     |<---------------|                |                |
     |     ACK F11    |                |                |
     |--------------->|                |                |
     |                |                |                |
```

В этом сценарии Боб занят и посылает ответ `486 Busy Here` на `INVITE` Алисы.  Обратите внимание, что ответ не-2xx подтверждается по принципу hop-by-hop, а не из конца в конец. Также обратите внимание, что многие SIP UA не будут возвращать ответ `486`, поскольку они имеют несколько линий и другие функции.

Детали сообщения:

```
   F1 INVITE Alice -> Proxy 1

   INVITE sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
   Max-Forwards: 70
   Route: <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:alice@client.atlanta.example.com;transport=tcp>
   Proxy-Authorization: Digest username="alice",
    realm="atlanta.example.com",
    nonce="dc3a5ab2530aa93112cf5904ba7d88fa", opaque="",
    uri="sip:bob@biloxi.example.com",
    response="702138b27d869ac8741e10ec643d55be"
   Content-Type: application/sdp
   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000

   /* Клиент Алисы готовится к приему данных на порт 49172 из сети. */


   F2 INVITE Proxy 1 -> Proxy 2

   INVITE sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/TCP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Max-Forwards: 69
   Record-Route: <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:alice@client.atlanta.example.com;transport=tcp>
   Content-Type: application/sdp
   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000


   F3 100 Trying Proxy 1 -> Alice

   SIP/2.0 100 Trying
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Content-Length: 0

   F4 INVITE Proxy 2 -> Bob

   INVITE sip:bob@client.biloxi.example.com SIP/2.0
   Via: SIP/2.0/TCP ss2.biloxi.example.com:5060;branch=z9hG4bK721e4.1
   Via: SIP/2.0/TCP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.111
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Max-Forwards: 68
   Record-Route: <sip:ss2.biloxi.example.com;lr>,
    <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:alice@client.atlanta.example.com;transport=tcp>
   Content-Type: application/sdp
   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000


   F5 100 Trying Proxy 2 -> Proxy 1

   SIP/2.0 100 Trying
   Via: SIP/2.0/TCP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.111
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Content-Length: 0


   F6 486 Busy Here Bob -> Proxy 2

   SIP/2.0  486 Busy Here
   Via: SIP/2.0/TCP ss2.biloxi.example.com:5060;branch=z9hG4bK721e4.1
    ;received=192.0.2.222
   Via: SIP/2.0/TCP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.111
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Content-Length: 0


   F7 ACK Proxy 2 -> Bob

   ACK sip:bob@client.biloxi.example.com SIP/2.0
   Via: SIP/2.0/TCP ss2.biloxi.example.com:5060;branch=z9hG4bK721e4.1
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 ACK
   Content-Length: 0


   F8 486 Busy Here Proxy 2 -> Proxy 1

   SIP/2.0  486 Busy Here
   Via: SIP/2.0/TCP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.111
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Content-Length: 0


   F9 ACK Proxy 1 -> Proxy 2

   ACK sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/TCP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 ACK
   Content-Length: 0

   F10 486 Busy Here Proxy 1 -> Alice

   SIP/2.0  486 Busy Here
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Content-Length: 0


   F11 ACK Alice -> Proxy 1

   ACK sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 ACK
   Proxy-Authorization: Digest username="alice",
    realm="atlanta.example.com",
    nonce="dc3a5ab2530aa93112cf5904ba7d88fa", opaque="",
    uri="sip:bob@biloxi.example.com",
    response="702138b27d869ac8741e10ec643d55be"
   Content-Length: 0
```

### 3.10. Неудачно - нет ответа от User Agent

```
   Alice           Proxy 1          Proxy 2            Bob
     |                |                |                |
     |   INVITE F1    |                |                |
     |--------------->|   INVITE F2    |                |
     |     100  F3    |--------------->|   INVITE F4    |
     |<---------------|     100  F5    |--------------->|
     |                |<---------------|   INVITE F6    |
     |                |                |--------------->|
     |                |                |   INVITE F7    |
     |                |                |--------------->|
     |                |                |   INVITE F8    |
     |                |                |--------------->|
     |                |                |   INVITE F9    |
     |                |                |--------------->|
     |                |                |   INVITE F10   |
     |                |                |--------------->|
     |                |                |   INVITE F11   |
     |                |     480 F12    |--------------->|
     |                |<---------------|                |
     |                |     ACK F13    |                |
     |     480 F14    |--------------->|                |
     |<---------------|                |                |
     |     ACK F15    |                |                |
     |--------------->|                |                |
     |                |                |                |
```

В этом примере Боб не отвечает на сообщения `INVITE` Алисы, повторно передаваемые `Proxy 2`. После шестой повторной передачи, `Proxy 2` отказывает и посылает Алисе сообщение `480 No Response`.

Детали сообщения:

```
   F1 INVITE Alice -> Proxy 1

   INVITE sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
   Max-Forwards: 70
   Route: <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:alice@client.atlanta.example.com>
   Proxy-Authorization: Digest username="alice",
    realm="atlanta.example.com",
    nonce="cf5904ba7d8dc3a5ab2530aa931128fa", opaque="",
    uri="sip:bob@biloxi.example.com",
    response="7afc04be7961f053c24f80e7dbaf888f"
   Content-Type: application/sdp
   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000

   /* Клиент Алисы готовится к приему данных на порт 49172 из сети. */


   F2 INVITE Proxy 1 -> Proxy 2

   INVITE sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Max-Forwards: 69
   Record-Route: <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:alice@client.atlanta.example.com>
   Content-Type: application/sdp
   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000


   F3 100 Trying Proxy 1 -> Alice

   SIP/2.0 100 Trying
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101

   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Content-Length: 0


   F4 INVITE Proxy 2 -> Bob

   INVITE sip:bob@client.biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP ss2.biloxi.example.com:5060;branch=z9hG4bK721e4.1
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.111
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Max-Forwards: 68
   Record-Route: <sip:ss2.biloxi.example.com;lr>,
   <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:alice@client.atlanta.example.com>
   Content-Type: application/sdp
   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000


   F5 100 Trying Proxy 2 -> Proxy 1

   SIP/2.0 100 Trying
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.111
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Content-Length: 0

   F6 INVITE Proxy 2 -> Bob

   Resend of Message F4


   F7 INVITE Proxy 2 -> Bob

   Resend of Message F4


   F8 INVITE Proxy 2 -> Bob

   Resend of Message F4


   F9 INVITE Proxy 2 -> Bob

   Resend of Message F4


   F10 INVITE Proxy 2 -> Bob

   Resend of Message F4


   F11 INVITE Proxy 2 -> Bob

   Resend of Message F4

	 /* Proxy 2 отказывает */


   F12 480 No Response Proxy 2 -> Proxy 1

   SIP/2.0 480 No Response
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.111
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Content-Length: 0

   F13 ACK Proxy 1 -> Proxy 2

   ACK sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 ACK
   Content-Length: 0


   F14 480 No Response Proxy 1 -> Alice

   SIP/2.0 480 No Response
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Content-Length: 0


   F15 ACK Alice -> Proxy 1

   ACK sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 ACK
   Proxy-Authorization: Digest username="alice",
    realm="atlanta.example.com",
    nonce="cf5904ba7d8dc3a5ab2530aa931128fa", opaque="",
    uri="sip:bob@biloxi.example.com",
    response="7afc04be7961f053c24f80e7dbaf888f"
   Content-Length: 0
```

### 3.11. Неудачно - временно недоступно

```
   Alice          Proxy 1          Proxy 2            Bob
     |                |                |                |
     |   INVITE F1    |                |                |
     |--------------->|   INVITE F2    |                |
     |     100  F3    |--------------->|   INVITE F4    |
     |<---------------|     100  F5    |--------------->|
     |                |<---------------|      180 F6    |
     |                |     180 F7     |<---------------|
     |     180 F8     |<---------------|                |
     |<---------------|                |     480 F9     |
     |                |                |<---------------|
     |                |                |     ACK F10    |
     |                |     480 F11    |--------------->|
     |                |<---------------|                |
     |                |     ACK F12    |                |
     |     480 F13    |--------------->|                |
     |<---------------|                |                |
     |     ACK F14    |                |                |
     |--------------->|                |                |
     |                |                |                |
```

В этом сценарии Боб сначала посылает Алисе ответ `180 Ringing`, указывая, что происходит оповещение. Однако затем Алисе отправляется ответ `480 Unavailable`. Этот ответ подтверждается и передается обратно Алисе.

Детали сообщения:

```
   F1 INVITE Alice -> Proxy 1

   INVITE sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
   Max-Forwards: 70
   Route: <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:alice@client.atlanta.example.com>
   Proxy-Authorization: Digest username="alice",
    realm="atlanta.example.com",
    nonce="aa9311cf5904ba7d8dc3a5ab253028fa", opaque="",
    uri="sip:bob@biloxi.example.com",
    response="59a46a91bf1646562a4d486c84b399db"
   Content-Type: application/sdp

   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000

   /* Клиент Алисы готовится к приему данных на порт 49172 из сети. */


   F2 INVITE Proxy 1 -> Proxy 2

   INVITE sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Max-Forwards: 69
   Record-Route: <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:alice@client.atlanta.example.com>
   Content-Type: application/sdp
   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000


   F3 100 Trying Proxy 1 -> Alice

   SIP/2.0 100 Trying
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE

   Content-Length: 0


   F4 INVITE Proxy 2 -> Bob

   INVITE sip:bob@client.biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP ss2.biloxi.example.com:5060;branch=z9hG4bK721e4.1
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.111
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Max-Forwards: 68
   Record-Route: <sip:ss2.biloxi.example.com;lr>,
    <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:alice@client.atlanta.example.com>
   Content-Type: application/sdp
   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000


   F5 100 Trying Proxy 2 -> Proxy 1

   SIP/2.0 100 Trying
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.111
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Content-Length: 0


   F6 180 Ringing Bob -> Proxy 2

   SIP/2.0 180 Ringing
   Via: SIP/2.0/UDP ss2.biloxi.example.com:5060;branch=z9hG4bK721e4.1
    ;received=192.0.2.222
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.111
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Record-Route: <sip:ss2.biloxi.example.com;lr>,
    <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:bob@client.biloxi.example.com>
   Content-Length: 0


   F7 180 Ringing Proxy 2 -> Proxy 1

   SIP/2.0 180 Ringing
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.111
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Record-Route: <sip:ss2.biloxi.example.com;lr>,
    <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:bob@client.biloxi.example.com>
   Content-Length: 0


   F8 180 Ringing Proxy 1 -> Alice

   SIP/2.0 180 Ringing
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Record-Route: <sip:ss2.biloxi.example.com;lr>,
    <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:bob@client.biloxi.example.com>

   Content-Length: 0


   F9 480 Temporarily Unavailable Bob -> Proxy 2

   SIP/2.0 480 Temporarily Unavailable
   Via: SIP/2.0/UDP ss2.biloxi.example.com:5060;branch=z9hG4bK721e4.1
    ;received=192.0.2.222
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.111
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Content-Length: 0


   F10 ACK Proxy 2 -> Bob

   ACK sip:bob@client.biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP ss2.biloxi.example.com:5060;branch=z9hG4bK721e4.1
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 ACK
   Content-Length: 0


   F11 480 Temporarily Unavailable Proxy 2 -> Proxy 1

   SIP/2.0 480 Temporarily Unavailable
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.111
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Content-Length: 0


   F12 ACK Proxy 1 -> Proxy 2

   ACK sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 ACK
   Content-Length: 0


   F13 480 Temporarily Unavailable Proxy 1 -> Alice

   SIP/2.0 480 Temporarily Unavailable
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Content-Length: 0


   F14 ACK Alice -> Proxy 1

   ACK sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   Proxy-Authorization: Digest username="alice",
    realm="atlanta.example.com",
    nonce="aa9311cf5904ba7d8dc3a5ab253028fa", opaque="",
    uri="sip:bob@biloxi.example.com",
    response="59a46a91bf1646562a4d486c84b399db"
   CSeq: 1 ACK
   Content-Length: 0
```

## 4. Соображения безопасности

Поскольку данный документ содержит примеры установления сеанса SIP, применяются соображения безопасности, изложенные в RFC 3261[1]. RFC 3261 описывает основные угрозы, включая перехват регистрации, выдачу себя за сервер, подделку тела сообщения, модификацию или разрыв сеанса, а также атаки типа "отказ в обслуживании" и "усиленные атаки".  Использование дайджеста HTTP, как показано в этом документе, обеспечивает одностороннюю аутентификацию и защиту от повторных атак. Транспорт TLS используется в сценариях регистрации из-за отсутствия защиты целостности в дайджесте HTTP и опасности перехвата регистрации без нее, как описано в RFC 3261[1].  Полное обсуждение недостатков дайджестов HTTP приведено в RFC 3261[1].  Использование TLS и схемы URI Secure SIP (sips) обеспечивает более высокий уровень безопасности, включая двустороннюю аутентификацию.  S/MIME может обеспечить сквозную конфиденциальность и защиту целостности тел сообщений, как описано в RFC 3261.

## 5. Ссылки

### 5.1. Нормативные ссылки

   [1] Rosenberg, J., Schulzrinne, H., Camarillo, G., Johnston, A.,
       Peterson, J., Sparks, R., Handley, M. and E. Schooler, "SIP:
       Session Initiation Protocol", RFC 3261, June 2002.

   [2] Rosenberg, J. and H. Schulzrinne, "An Offer/Answer Model with
       SDP", RFC 3264, April 2002.

   [3] Franks, J., Hallam-Baker, P., Hostetler, J., Lawrence, S., Leach,
       P., Luotonen, A. and L. Stewart, "HTTP authentication: Basic and
       Digest Access Authentication", RFC 2617, June 1999.

   [4] Bradner, S., "Key words for use in RFCs to Indicate Requirement
       Levels", BCP 14, RFC 2119, March 1997.

### 5.2. Информативные ссылки

   [5] Johnston, A., Donovan, S., Sparks, R., Cunningham, C. and K.
       Summers, "Session Initiation Protocol (SIP) Public Switched
       Telephone Network (PSTN) Call Flows", BCP 76, RFC 3666, December
       2003.

## 6. Заявление об интеллектуальной собственности

IETF не занимает никакой позиции в отношении действительности или объема любых прав интеллектуальной собственности или других прав, которые могут быть заявлены как относящиеся к реализации или использованию технологии, описанной в данном документе, или степени, в которой может быть или не быть доступна любая лицензия на основании таких прав; она также не утверждает, что предприняла какие-либо усилия для выявления таких прав.  Информация о процедурах IETF в отношении прав на документацию, относящуюся к треку стандартов и стандартам, содержится в документе BCP-11.  Копии заявлений о правах, предоставленных для публикации, и любые заверения о лицензиях, которые будут предоставлены, или результат попытки получить общую лицензию или разрешение на использование таких прав собственности исполнителями или пользователями данной спецификации могут быть получены в Секретариате IETF.

IETF приглашает любую заинтересованную сторону довести до ее сведения любые авторские права, патенты или патентные заявки, или другие права собственности, которые могут охватывать технологию, которая может потребоваться для применения настоящего стандарта.  Пожалуйста, адресуйте информацию Исполнительному директору IETF.

## 7. Благодарности

Этот документ был подготовлен совместными усилиями рабочих групп SIP и SIPPING. Авторы хотят поблагодарить всех, кто читал, просматривал, комментировал или вносил предложения по улучшению этого документа.

Спасибо Rohan Mahy, Adam Roach, Gonzalo Camarillo, Cullen Jennings и Tom Taylor за их подробные комментарии во время окончательного рассмотрения. Спасибо Dean Willis за его ранний вклад в разработку этого документа.

Авторы выражают благодарность Kundan Singh за выполнение проверки синтаксического анализа сообщений.

Авторы выражают благодарность следующим лицам за их участие в рассмотрении данного документа о потоках вызовов: Aseem Agarwal, Rafi Assadi, Ben Campbell, Sunitha Kumar, Jon Peterson, Marc Petit-Huguenin, Vidhi Rastogi и Bodgey Yin Shaohua.

Авторы также хотели бы поблагодарить за помощь следующих лиц: Jean-Francois Mule, Hemant Agrawal, Henry Sinnreich, David Devanatham, Joe Pizzimenti, Matt Cannon, John Hearty, the whole MCI WorldCom IPOP Design team, Scott Orton, Greg Osterhout, Pat Sollee, Doug Weisenberg, Danny Mistry, Steve McKinnon, and Denise Ingram, Denise Caballero, Tom Redman, Ilya Slain, Pat Sollee, John Truetken, и других представителей MCI WorldCom, 3Com, Cisco, Lucent и Nortel.

## 8. Адреса авторов

Все перечисленные авторы активно участвовали в написании большого объема текста данного документа.

   Alan Johnston
   MCI
   100 South 4th Street
   St. Louis, MO 63102
   USA

   EMail: alan.johnston@mci.com

   Steve Donovan
   dynamicsoft, Inc.
   5100 Tennyson Parkway
   Suite 1200
   Plano, Texas 75024
   USA

   EMail: sdonovan@dynamicsoft.com

   Robert Sparks
   dynamicsoft, Inc.
   5100 Tennyson Parkway
   Suite 1200
   Plano, Texas 75024
   USA

   EMail: rsparks@dynamicsoft.com

   Chris Cunningham
   dynamicsoft, Inc.
   5100 Tennyson Parkway
   Suite 1200
   Plano, Texas 75024
   USA

   EMail: ccunningham@dynamicsoft.com

   Kevin Summers
   Sonus
   1701 North Collins Blvd, Suite 3000
   Richardson, TX 75080
   USA

   EMail: kevin.summers@sonusnet.com

## 9. Полное заявление об авторских правах

Copyright (C) The Internet Society (2003).  All Rights Reserved.

This document and translations of it may be copied and furnished to others, and derivative works that comment on or otherwise explain it or assist in its implementation may be prepared, copied, published and distributed, in whole or in part, without restriction of any kind, provided that the above copyright notice and this paragraph are included on all such copies and derivative works.  However, this document itself may not be modified in any way, such as by removing the copyright notice or references to the Internet Society or other Internet organizations, except as needed for the purpose of developing Internet standards in which case the procedures for copyrights defined in the Internet Standards process must be followed, or as required to translate it into languages other than English.

The limited permissions granted above are perpetual and will not be revoked by the Internet Society or its successors or assignees.

This document and the information contained herein is provided on an "AS IS" basis and THE INTERNET SOCIETY AND THE INTERNET ENGINEERING TASK FORCE DISCLAIMS ALL WARRANTIES, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO ANY WARRANTY THAT THE USE OF THE INFORMATION HEREIN WILL NOT INFRINGE ANY RIGHTS OR ANY IMPLIED WARRANTIES OF MERCHANTABILITY OR FITNESS FOR A PARTICULAR PURPOSE.

### Благодарность

Финансирование функции редактора RFC в настоящее время осуществляется Интернет-сообществом.
