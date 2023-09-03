# Настройка Openwrt без GUI

Есть роутеры без GUI. Это TL 841v14 и некоторые другие старички. Для них маршрут интереснее.

    ssh root@192.168.1.1

При ошибке unable to negotiate... sha-rsa делаем так

    ssh -o HostKeyAlgorithms=+ssh-rsa -o PubkeyAcceptedKeyTypes=+ssh-rsa root@192.168.1.1

При первом подключении прописываем yes для сохранения ssh ключа. 

## Дата

    date -s 2023.12.31-14:30:00 для 31 декабря 2023 года 14:30

Выведет строку, соответствующую ISO формату записи введенной даты.
Если все норм, то значит дата записалась нормально.

## Как юзать vi

для переключения между режимами редактирования и просмотра жмём __i__ для редактирования и __Esc__ для просмотра.

стрелочки работают для перемещения между символами.

__Выход из редактора:__

[Esc] [:] [w] [q] [Enter] последовательность для выхода из режима редактирования, начала ввода команды, записи файла с сохранением изменений и выхода из редактора.

[Esc] [:] [q] [!] [Enter] последовательность для выхода из режима редактирования, начала ввода команды, выхода файла с подтверждением о том, что изменения не будут записаны.

## MAC адрес

    vi /etc/config/network

Оглядываем взглядом файл. Если есть секция wan_eth0.2_dev (wan_eth1_dev) - с суффиксом dev - то редактируем ее.

Допустим, нужно подменить мак адрес на AC-15-A2-1C-F1-5D.

    config device 'wan_eth0_2_dev'
        option name 'eth0.2'
        option macaddr 'ac:15:a2:1c:f1:5d'

Находим требуемую область в конфиге, спускаемся к ней стрелочками, входим в режим редактирования [i], удаляем символы старого mac адреса и прописываем новый (обязательно lowercase через двоеточие). Будет как в приложенной области. Выходим через [Esc] [:wq] [Enter].

Если секции wan_eth0.2_dev (wan_eth1_dev) нет, то ищем просто секцию wan_eth0.2 (wan_eth1). Там аналогично строчка с позицией macaddr.

## Беспроводная сеть

    vi /etc/config/wireless

В верхней секции (config wifi-device) меняем только disabled секцию

    config wifi-device 'radio0'
        option type 'mac80211'
        option channel '11'
        option hwmode '11g'
        option path 'platform/10300000.wmac'
        option htmode 'HT20'
        option disabled '0'

(чтобы после обновления включился wifi)

В секции config wifi-iface меняем ssid, encryption и дописываем пароль.

config wifi-iface 'default_radio0'
        option device 'radio0'
        option network 'lan'
        option mode 'ap'
        option ssid 'WIFI SSID'
        option encryption 'psk2'
        option key 'WIFI PWD'



## Файлы 802.1x

По идее все файлы без GUI зашиты одинаковым набором файлов от меня, поэтому должно совпадать. Если нет - welcome в поиск файлов через ls. _:evil:_

    chmod +x /etc/init.d/wpa
    /etc/init.d/wpa enable
    /etc/init.d/wpa start

Также редактируем конфиг.

    vi /etc/config/wpa.conf
--

    ctrl_interface=/var/run/wpa_supplicant
    network {
        <...>
        identity="8230001"
        password="orioks_pwd"
    }

__заметка__

В новейшей ревизии переделал на /etc/config/mschapv2 и на /etc/init.d/wpad

## Главный этап сие мероприятия

    reboot

Волшебная перезагрузка применяет все изменения и делает так, чтобы заработало.

## Проверка состояния

    ifconfig

Смотрим на eth0.2 (eth1). Нас интересует наличие следующих строк:

    eth0.2  Link encap:Ethernet <...>
            inet addr: 172.18.54.199
            RX packets: 14071
            TX packets: 2884

Наличие метки inet addr позволяет понять, что выдан ipv4 адрес от провайдера.

RX packets показывает наличие принятых пакетов от коммутатора на этаже, то есть работоспособность провода. Здесь это смотреть чуть проще, чем через /sys/class/net так что вот.

Если есть ip адрес (ipv6 не в счет), все работает хорошо.