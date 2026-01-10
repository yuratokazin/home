## DD-WRT для чего нужен Ethernet and IP Tunneling

http://192.168.1.1/eop-tunnel.asp

**Setup** >>> **Tunnels**

Функции 

**Ethernet and IP Tunneling** в DD-WRT предназначены для создания виртуальных каналов связи между двумя удаленными точками через интернет. 

Вот основные сценарии их использования:

- **Объединение сетей (L2 Bridge):** Технология **EoIP (Ethernet over IP)** позволяет соединить два роутера так, будто они подключены друг к другу обычным кабелем. Это создает единую локальную сеть между двумя квартирами или офисами: устройства в разных городах будут видеть друг друга по локальным IP, работать в одной рабочей группе и поддерживать функции вроде Steam Remote Play или общих сетевых дисков.
- **Создание IP-туннелей (L3):** Протоколы вроде **IPIP** или **GRE** используются для передачи IP-пакетов внутри другого IP-пакета. Это нужно для построения корпоративных сетей (Site-to-Site VPN) или проброса трафика к специфическому оборудованию, например, для связи с маршрутизаторами Cisco.
- **Получение IPv6:** С помощью туннелирования (например, **6in4**) можно получить доступ к современному протоколу IPv6, если ваш провайдер его еще не поддерживает, подключившись к бесплатному сервису (Tunnel Broker).
- **Обход ограничений:** Туннели скрывают тип трафика от провайдера, позволяя обходить блокировки или безопасно подключаться к домашней сети из публичных Wi-Fi зон. 

Хотите объединить **две конкретные точки** в одну сеть или просто ищете способ **безопасного доступа** к домашним файлам извне?


Ссылки:


[View topic - ipip, ip-gre tunnel - DD-WRT](https://forum.dd-wrt.com/phpBB2/viewtopic.php?t=69774&sid=6f6e7d81836a7e60b280b8de9ea5fccd)
3 апр. 2010 г. — Posted: Sat Apr 03, 2010 8:45 Post subject: ipip, ip-gre tunnel, Reply with quote. Интересует возможность создания i...

DD-WRT
[EOIP между DD-WRT и Keenetic Giga 2 для объединения ...](https://habr.com/ru/articles/424807/)
27 сент. 2018 г. — EOIP между DD-WRT и Keenetic Giga 2 для объединения двух квартир в одну сеть или один HDD на две квартиры / Хабр

Хабр

[View topic - Site to Site Tunnel - DD-WRT](https://forum.dd-wrt.com/phpBB2/viewtopic.php?p=989344#:~:text=DD%2DWRT%20has%20capabilities%20built,router%20at%20the%20same%20time.)
21 окт. 2015 г. — DD-WRT has capabilities built-in for using PPTP VPNs. The PPTP SERVER end of the connection will handle multiple co...

DD-WRT
IPv6 6in4 Tunnel Configuration on DD-WRT - Medium
14 окт. 2016 г. — Dynamic DNS. The solution to this problem is to use a dynamic DNS updater which DD-WRT has good support for. Dynami...

Medium

[Easy SSH tunnels/ru - DD-WRT Wiki](https://wiki.dd-wrt.com/wiki/index.php/Easy_SSH_tunnels/ru#:~:text=Easy%20SSH%20tunnels/ru%20%2D%20DD%2DWRT%20Wiki)
1 июн. 2020 г. — Easy SSH tunnels/ru - DD-WRT Wiki.

DD-WRT

[Easy SSH tunnels - DD-WRT Wiki](https://wiki.dd-wrt.com/wiki/index.php/Easy_SSH_tunnels#:~:text=%5Bedit%5D%20Introduction,restrictions%20at%20the%20remote%20location.)
6 нояб. 2010 г. — [edit] Introduction. SSH tunneling allows you to forward traffic from one location to another using encryption betw...

DD-WRT

[IPv6, 6in4 tunnel - GUI only - DD-WRT Wiki](https://wiki.dd-wrt.com/wiki/index.php/IPv6%2C_6in4_tunnel_-_GUI_only#:~:text=First%20thing%20you%20need%20to%20do%2C%20is,to%20main%20page%20to%20create%20a%20tunnel.)
First thing you need to do, is to create your account at HE.net IPv6 Tunnel Broker After creating an account, return back to main ...

DD-WRT

[EOIP & DD-WRT для объединения двух квартир в одну сеть или ...](https://habr.com/ru/articles/271605/)
23 нояб. 2015 г. — EOIP & DD-WRT для объединения двух квартир в одну сеть или все для Remote Play / Хабр

Хабр

[IP tunnel - Wikipedia](https://en.wikipedia.org/wiki/IP_tunnel#:~:text=An%20IP%20tunnel%20is%20an,by%20encapsulation%20of%20its%20packets.)
An IP tunnel is an Internet Protocol (IP) network communications channel between two networks. It is used to transport another net...

Wikipedia

[EoIP - RouterOS - MikroTik Documentation](https://help.mikrotik.com/docs/spaces/ROS/pages/24805521/EoIP#:~:text=Ethernet%20over%20IP%20(EoIP)%20Tunneling,connection%20capable%20of%20transporting%20IP.)
21 янв. 2024 г. — Ethernet over IP (EoIP) Tunneling is a MikroTik RouterOS protocol based on GRE RFC 1701 that creates an Ethernet tu...

MikroTik

[What is tunneling? | Tunneling in networking - Cloudflare](https://www.cloudflare.com/ru-ru/learning/network-layer/what-is-tunneling/)
Similarly, in networking, tunnels are a method for transporting data across a network using protocols that are not supported by th...

Cloudflare

[IP Tunnels - HPE Aruba Networking](https://arubanetworking.hpe.com/techdocs/AOS-CX/10.10/HTML/ip_services_83xx-10000/Content/Chp_IP_tun/ip-tun-10.htm#:~:text=An%20IP%20tunnel%20provides%20a,networks%20that%20the%20tunnel%20spans.)
An IP tunnel provides a virtual link between endpoints on two different networks enabling data to be exchanged as if the endpoints...

Hewlett Packard Enterprise
























