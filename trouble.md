# faq-asterisk

Частозадаваемые вопросы и частовстречаемые задачи, на которые можно найти ответы здесь.

---

####     -- Got SIP response 302 "Moved Temporarily" back from 192.168.11.162:5062

Настроена переадресация на самом аппарате.

Лечится отключением переадресации на аппарате.

---

#### Номер определяется совсем не так, как отправляется CallerID в вызов

Проверить параметры транка — возможно параметр fromuser задает другой номер

#### [Jan 26 17:20:22] WARNING[5184]: chan_sip.c:3714 \__sip_xmit: sip_xmit of 0x86be080 (len 381) to (null) returned -1: Invalid argument
#### [Jan 26 17:20:22] NOTICE[5184]: chan_sip.c:13734 sip_reg_timeout:    -- Registration for '348XY6@sipnet.ru' timed out, trying again (Attempt #168982)
#### [Jan 26 17:20:23] WARNING[5184]: chan_sip.c:3714 \__sip_xmit: sip_xmit of 0x86be080 (len 381) to (null) returned -1: Invalid argument

при этом пинг на доменное имя не идет

Ошибка DNS-сервера
лечится исправлением в /etc/resolv.conf
`nameserver 8.8.8.8`

если пинг на доменное имя проходит нормально — днс в порядке

---

#### -- Called SIP/line1/89XYZA78796     -- Got SIP response 603 "Declined" back from 192.168.1.193:5060     -- SIP/line1-00000348 is busy   == Everyone is busy/congested at this time (1:1/0/0)

Проблема в городской сети — нет сигнала на приходящей линии

---

#### [2019-03-29 12:41:55] WARNING[2960][C-00000146]: chan_sip.c:10262 process_sdp: Failed to initialize UDPTL, declining image stream
#### [2019-03-29 12:41:55] WARNING[2960][C-00000146]: pbx.c:7010 ast_pbx_start: PBX requires Asterisk to be fully booted
#### [2019-03-29 12:41:55] WARNING[2960][C-00000146]: chan_sip.c:26074 handle_request_invite: Failed to start PBX :(

отобрать у пользователя:группы `asterisk` права на доступ к файлам в /etc/unixODBC/

---
