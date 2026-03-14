## 1. Определение заражённого хоста (ARP)

На основании ARP-трафика видно, что инициатором сетевой активности является хост **10.1.17.215** (MAC: Intel_26:4a:74). 
Этот хост первым обращается к контроллеру домена и выполняет ARP Probe.

![ARP Screenshot](screenshots/arp_infected_host.png)

## 2. DNS-анализ (определение контроллера домена)

Хост **10.1.17.215** выполняет стандартные доменные DNS-запросы к контроллеру домена:

- SRV-запрос: `_ldap._tcp.Default-First-Site-Name._sites.dc._msdcs.bluemoontuesday.com`
- Ответ указывает на контроллер домена: **win-gsh54qlw48d.bluemoontuesday.com** (IP **10.1.17.2**)

Эта активность является штатной для доменного клиента и подтверждает:
- роль **10.1.17.2** как контроллера домена и DNS-сервера;
- роль **10.1.17.215** как доменного клиента.

![DNS Query](screenshots/dns_query_dc.png)
![DNS Response](screenshots/dns_response_dc.png)

## 2. DNS-анализ (системный трафик Windows)

После ARP-активности хост 10.1.17.215 выполняет стандартные DNS-запросы, характерные для Windows-клиента

### Запросы к Microsoft Defender  телеметрии
Хост обращается к домену

- `kv801.prod.do.dsp.mp.microsoft.com`

Ответ содержит цепочку CNAME через CDN Akamai

- `kv801.prod.do.dsp.mp.microsoft.com.edgekey.net`
- `e12437.d.akamaiedge.net`
- A‑запись 23.40.146.4

Это штатный трафик Microsoft Defender и не является вредоносным.

![DNS Query Microsoft](screenshotsdns_query_dc.png)
![DNS Response Microsoft](screenshotsdns_response_dc.png)

### Проверка подключения к интернету (NCSI)
Хост также выполняет запрос

- `www.msftconnecttest.com`

Ответ проходит через

- `ncsi-geo.trafficmanager.net`
- `www.msftncsi.com.edgesuite.net`
- `a1961.g2.akamai.net`

И резолвится в

- 23.220.102.9
- 23.220.102.18

Это стандартная проверка интернет‑доступа Windows (NCSI).

---

Данный трафик является нормальным и важен для контекста он показывает, что хост функционирует как обычный доменный клиент. Вредоносной активности в этих запросах нет.
![DNS Screenshot](screenshots/313131.png)
