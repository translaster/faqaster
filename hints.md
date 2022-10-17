# Подсказки

#### Отправка сообщения MESSAGE всем контактам одного endpoint
```
exten => _XX,1,Set(TIMEOUT(absolute)=30)
 same => n,Set(array_contacts=${PJSIP_DIAL_CONTACTS(${EXTEN})})
 same => n,While($["${SET(contact=${SHIFT(array_contacts,&):6})}" != ""])
 same => n,MessageSend(pjsip:${contact})
 same => n,EndWhile
 same => n,Hangup()
```
