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

#### Настройка DAHDI для карты TDM800 и TDM410

Хотя TDM410 и TDM800 больше не «поддерживаются», их можно заставить работать.
В файл `dahdi-linux-complete-3.1.0+3.1.0/linux/drivers/dahdi/wctdm24xxp/base.c` добавьте обратно устройства PCI для TDM410 и TDM800

```
static DEFINE_PCI_DEVICE_TABLE(wctdm_pci_tbl) = {
    { 0xd161, 0x2400, PCI_ANY_ID, PCI_ANY_ID, 0, 0, (unsigned long) &wctdm2400 },
    **{ 0xd161, 0x0800, PCI_ANY_ID, PCI_ANY_ID, 0, 0, (unsigned long) &wctdm800 },**
    { 0xd161, 0x8002, PCI_ANY_ID, PCI_ANY_ID, 0, 0, (unsigned long) &wcaex800 },
    { 0xd161, 0x8003, PCI_ANY_ID, PCI_ANY_ID, 0, 0, (unsigned long) &wcaex2400 },
    **{ 0xd161, 0x8005, PCI_ANY_ID, PCI_ANY_ID, 0, 0, (unsigned long) &wctdm410 },**
    { 0xd161, 0x8006, PCI_ANY_ID, PCI_ANY_ID, 0, 0, (unsigned long) &wcaex410 },
    { 0xd161, 0x8007, PCI_ANY_ID, PCI_ANY_ID, 0, 0, (unsigned long) &wcha80000 },
    { 0xd161, 0x8008, PCI_ANY_ID, PCI_ANY_ID, 0, 0, (unsigned long) &wchb80000 },
    { 0 }
};
```

ну и конечно пересобрать dahdi

https://serverfault.com/questions/978738/dahdi-3-0-0-not-assign-spans-and-cannot-generate-configuration
