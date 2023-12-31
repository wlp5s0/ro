# Проверка подключения

Проверка подключения заключается в проверке корректности выданных параметров сети и наличия доступа к ресурсам ЛВС МИЭТ (они доступны без интернета).

__Выданный IP адрес__

Устроству, подключенному к ЛВС МИЭТ, выдается IP адрес в подсети 172.18.0.0/16, то есть любой адрес вида 172.18.55.20 является подтверждением успеха.

Посмотреть выданный IP можно [по гайду для мак адреса](./6-macaddr.md), там соседние строчки.

__Доступные ресурсы ЛВС__

При корректном подключении в сети будут доступны ресурсы [users.miet.ru](users.miet.ru), [miet.ru](miet.ru), [orioks.miet.ru](orioks.miet.ru). 

Также без оплаты можно пользоваться удаленным рабочим столом skylab и galaxy с предустановленным ПО для выполнения лабораторных работ.


Если этого не произошло, стоит проверить:
* Корректность используемых логина-пароля на account.miet.ru или orioks.miet.ru
* Правильность установленной даты на устройстве (в основном для роутеров)
* Попробовать настроить заново

Следующий шаг: [регистрация у провайдера](./5-reg.md).