# Конфиг dnsmasq для использования незаблокированных IP-адресов от популярных сервисов

_Russian-language only, as this is primarily intended to work-around
[Russian Censorship Agency](https://en.wikipedia.org/wiki/Roskomnadzor) blocking IPs used by popular
services, and is applicable only inside Russia._

Linux-only.

## Инструкция

1. Ставим из пакетов `dnsmasq`.

2. Не включаем его.
   
   На всякий случай через `sudo systemctl disable dnsmasq; sudo systemctl stop dnsmasq`
   проверяем, что он выключен. Конфиг днсмаска не трогаем — он использоваться не будет.

3. В `/etc/NetworkManager/NetworkManager.conf` дописываем:
 
   ```
   [main]
   dns=dnsmasq
   ```

4. Копируем [fuckrn.conf](fuckrkn.conf) в `/etc/NetworkManager/dnsmasq.d/fuckrkn.conf`.

5. `sudo systemctl restart NetworkManager`

И у вас будут работать слак, гмыло, гуглплей (и чего там ещё в конфиге) без протаскивания трафика
через VPN вне РФ.

Если эти айпишники заблочат — придётся менять, в телеге есть бот `@rkn_block_check_bot`, который
подскажет по домену, какие айпишники от него живы, какие заблокированы.

### Особенности

* Штатный конфиг `/etc/dnsmasq.conf` не используется — NetworkManager запускет свой экземпляр.

* `dnsmasq` таким образом уже будет поднят только на `127.0.0.1` и голой задницей в интернеты
  торчать не будет. Проверяется через `sudo netstat -apvtu --numeric-hosts | grep dnsmasq`.

* Если вы испольузете свой VPN для роутинга _только заблокированных адресов_ через него — такой
  сетап всё равно будет полезен, потому что тогда эти запросы пойдут в обход VPN.

### Лицензия

WTFPL или СС0, на выбор.
