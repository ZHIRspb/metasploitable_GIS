# IIS DoS

Службы Internet Information Services (IIS) — это гибкий веб-сервер общего назначения от Microsoft, который работает в системах Windows и обслуживает запрошенные HTML-страницы или файлы.

Веб-сервер IIS принимает запросы от удаленных клиентских компьютеров и возвращает соответствующий ответ. Эта базовая функциональность позволяет веб-серверам обмениваться информацией и доставлять ее через локальные сети (LAN), например корпоративные интрасети, и глобальные сети (WAN).

Веб-сервер может доставлять информацию пользователям в нескольких формах, например, в виде статических веб-страниц, закодированных в HTML; посредством обмена файлами в виде загрузок и выгрузок; и текстовые документы, файлы изображений и многое другое.

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

#### [CVE-2015-1635](https://nvd.nist.gov/vuln/detail/CVE-2015-1635)

В стеке протоколов HTTP (HTTP.sys) существует уязвимость удаленного выполнения кода, которая возникает, когда HTTP.sys неправильно анализирует специально созданные HTTP-запросы. Злоумышленник, который успешно воспользовался этой уязвимостью, может выполнить произвольный код в контексте системной учетной записи.

#### Настройка IIS

Изначально веб-сервер на машине не развернут, но его легко можно установить самому.

Для этого нажмите Start → Administrative Tools → Server Manager

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

Затем во вкладке Roles Summary нажмите на Add Roles

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

Затем в появившемся окне выберите Web Server (IIS) и нажмите Next

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

Далее установите необходимые сервисы в соответствии со скриншотом

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

Дождитесь установки всех компонентов, после чего по адресу [http://localhost](http://localhost/) будет доступна веб-страница IIS7

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

Для реализации DoS-атаки можно использовать готовый эксплойт в Metasploit

Используйте модуль `auxiliary/dos/http/ms15_034_ulonglongadd`

Затем нужно выставить параметру `rhosts` значение IP-адреса Metasploitable-машины и запустить скрипт

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

В ходе выполнения скрипта атакуемая машина “крашнется”, что свидетельствует об успешной эксплуатации атаки типа “Отказ в обслуживании”

<figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>
