# SSH Bruteforce

SSH, также известный как Secure Shell или Secure Socket Shell, - это сетевой протокол, который предоставляет пользователям безопасный доступ к компьютеру через незащищенную сеть.

SSH также относится к набору утилит, реализующих протокол SSH. Secure Shell обеспечивает надежную аутентификацию по паролю и открытому ключу, а также шифрование данных между двумя компьютерами, подключенными через открытую сеть.

Помимо обеспечения надежного шифрования, SSH широко используется для удаленного управления системами и приложениями, выполнения команд и перемещения файлов с одного компьютера на другой.

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

На целевой машине поднят 22 порт SSH, следовательно, мы можем попробовать подобрать логин и пароль от учетной записи

#### Hydra

Для брутфорса можно использовать утилиту [hydra](https://www.kali.org/tools/hydra/)

Чтобы ускорить процесс подбора я буду использовать кастомный список логинов и паролей, среди которых есть валидные имена пользователей `administrator` и `vagrant` и валидный пароль `vagrant`

> Файлы с юзернеймами и паролями находятся в этом каталоге

Введите следующую команду:

```
sudo hydra -L /путь/до/файла/usernames.txt -P /путь/до/файла/passwords.txt ваш-ip ssh
```

Через некоторое время подберутся валидные комбинации

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

С помощью них мы можем подключиться к целевой машине по SSH

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

#### Metasploit

Реализовать брутфорс-атаку можно также с помощью фреймворка Metasploit

> Установить Metasploit Framework можно с помощью команды `sudo apt install metasploit-framework`

Для этого нужно использовать модуль `auxiliary/scanner/ssh/ssh_login`, затем выставить IP-адрес атакуемой машины и файлы, в которых содержатся списки юзернеймов и паролей:

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

После завершения подбора на атакующей машине откроются 2 сессии, которые можно дальше использовать в своих целях

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

#### Nmap

Nmap тоже имеет функцию брутфорса, для этого нужно указать IP-адрес машины и порт, выбрать скрипт ssh-brute и указать файлы с юзернеймами и паролями

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

После некоторого времени вам будут выведены подходящие комбинации

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

Дополнительно на машине в можете обнаружить уязвимость, позволяющую определить, какие имена пользователей используются службой SSH с помощью OpenVAS

<figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

С помощью Metasploit модуля `auxiliary/scanner/ssh/ssh_enumusers` можно реализовать подобную проверку

<figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

Благодаря этому можно увеличить скорость подбора учетных данных, исключив недействительные имена пользователей
