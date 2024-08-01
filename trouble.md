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

#### Уведомления о пропущенных вызовах

* [в Telegram](https://wiki.merionet.ru/ip-telephoniya/63/modul-integracii-s-telegram-v-freepbx/)
  - [ссылка на файл](files/missedcallnotify-0.1.1-fix.tar.gz)

* [на e-mail](https://habr.com/ru/post/463829/)
  - [на e-mail аналогично Telegram (устаревший модуль)](https://github.com/FreePBX-ContributedModules/missedcallnotify)

#### не запускается FOP2
#### # /usr/local/fop2/fop2_server --debuglevel 5
#### eth0: error fetching interface information: Device not found
#### Can't get info from ifconfig:  at script/fop2_server.pl line 6878.

не найдено устройство eth0
в `/etc/init.d/fop2` есть строчка
```
[ -r /etc/sysconfig/fop2 ] && . /etc/sysconfig/fop2
```
лезем туда - это файл опций для запуска fop
```
# cat /etc/sysconfig/fop2
OPTIONS="-d"
```
У fop2_server можно просмотреть опции запуска `/usr/local/fop2/fop2_server --help` и интерфейс можно добавить опцией --iface, в итоге файл должен получиться
```
# cat /etc/sysconfig/fop2
OPTIONS="--daemon --iface eth2"
```

#### [2020-12-01 10:33:05] WARNING[24993] res_pjsip_registrar.c: Endpoint 'anonymous' has no configured AORs

Пофиксилось изменением порядка **Match Inbound Authentication** с **Auth Username** на **Username**

Прим: дело было с GSM-шлюзом KTS, который регистрировался на FreePBX.

#### Посоветуйте USB-модем для связи

Huawei E1550 и, возможно, e170 е171; модуль `chan_dongle`, при большом количестве лучше подключать через хороший usb-hub с внешним питанием.
те, что в 2009 под маркой МТС с оленем продавались - без проблем работают.

#### При вызове From: Anonymous <sip:anonymous@localhost> и с него вызов не проходит

В настройках линии включено `Block CID Setting:` (Скрытие номера) (Linksys SPA8000)

#### Как конвертировать аудиофайлы для asterisk

* [Ссылка 1](https://www.voip-info.org/convert-wav-audio-files-for-use-in-asterisk/)

* [Ссылка 2](https://alexeyka.zantsev.com/?p=839)

#### Как вызывать все телефоны, зарегистрированные на один номер в случае применения PJSIP

Использовать функцию `PJSIP_DIAL_CONTACTS()`, например:

```
exten => _6XXX,1,Dial(${PJSIP_DIAL_CONTACTS(${EXTEN})})
```

#### Где взять коды регионов и сотовый операторов?

Раньше они были на сайте Россвязи, а сейчас в [Реестре открытых данных Минцифры России](http://opendata.digital.gov.ru/)

#### Есть ли аналог callbackexten для PJSIP?

Есть

```
support_path=yes
line=yes
endpoint=<номер>
```

#### sngrep криво отображает графику

Если после запуска утилиты вы наблюдаете вместо нормальных линий линии из текста, то выполните в консоли линукса следующие команды:
```
alias sngrep=’NCURSES_NO_UTF8_ACS=1 sngrep’
echo export NCURSES_NO_UTF8_ACS=1 >> /etc/environment
```

#### WARNING[*]: db.c:288 db_execute_sql: Error executing SQL (COMMIT): database is locked

Ошибка доступа к файлу /var/lib/asterisk/astdb.sqlite3 должно быть `-rw-r--r--`, например:
```
-rw-r--r-- 1 asterisk asterisk 815K июн 17 14:38 /var/lib/asterisk/astdb.sqlite3
```

#### Некорректно отображается CallerID при вызовах между Panasonic KX-NS500 и Asterisk

Проблему удалось решить следующим образом. На стороне Panasonic в свойствах транкового порта необходимо включить параметры CNIP, как отображено на скриншоте. В Asterisk, для корректного отображения CallerID Name при входящих звонках со стороны Panasonic, необходимо выполнить конвертацию кодировки:
```
same => n,Set(CALLERID(name)=${ICONV(WINDOWS-1251,UTF-8,${CALLERID(name)})})
```
