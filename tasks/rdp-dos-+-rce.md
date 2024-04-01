# RDP DoS + RCE

Протокол удаленного рабочего стола (Remote Desktop Protocol) - протокол, использующийся для удаленного подключения пользователя к компьютеру

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

По умолчанию протокол удаленного рабочего стола (RDP) не включен ни в одной операционной системе Windows. Metasploitable 3 имеет несколько уязвимостей RDP.

Используя утилиту rdesktop вы можете получить доступ к удаленному компьютеру, если у вас есть действительные учетные данные.

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

На машине присутствуют две уязвимости, которые можно проэксплуатировать

### RDP DoS Exploitation

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

Уязвимость MS12-020 может быть использована для того, чтобы реализовать атаку типа “Отказ в обслуживании” на удаленном компьютере.

Для реализации атаки используйте модуль Metasploit `auxiliary/dos/windows/rdp/ms12_020_maxchannelids`

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

После выполнения скрипта на уязвимой машине высветится “синий экран” и она аварийно выключится

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

### Bluekeep Exploit

Эта служба также уязвима для атаки Bluekeep (CVE-2019-0708), и вы можете использовать соответствующий модуль Metasploit `auxiliary/scanner/rdp/cve_2019_0708_bluekeep` , чтобы проверить наличие этой уязвимости на целевой машине.

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

Для эксплуатации используйте модуль `exploit/windows/rdp/cve_2019_0708_bluekeep_rce`

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

В выводе мы можем видеть, что экплоит отработал, но сессия не была создана

Причину сбоя и ее решение можно найти в информации о модуле на сайте [rapid7](https://www.rapid7.com/db/modules/exploit/windows/rdp/cve\_2019\_0708\_bluekeep\_rce/)

<figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

Чтобы изменить реестр, нажмите Win+R на виртуальной клавиатуре и введите regedit

<figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

Затем перейдите в папку Computer\HKEY\_LOCAL\_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp

<figure><img src="../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

Замените значение параметра fDisableCam на 0

<figure><img src="../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

После этого при повторном использовании эксплоита создастся сессия meterpreter с системными правами

<figure><img src="../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>
