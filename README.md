# Травицкий Сергей

# Домашнее задание к занятию «Защита сети»

### Инструкция по выполнению домашнего задания

1. Сделайте fork [репозитория c шаблоном решения](https://github.com/netology-code/sys-pattern-homework) к себе в Github и переименуйте его по названию или номеру занятия, например, https://github.com/имя-вашего-репозитория/gitlab-hw или https://github.com/имя-вашего-репозитория/8-03-hw).
2. Выполните клонирование этого репозитория к себе на ПК с помощью команды `git clone`.
3. Выполните домашнее задание и заполните у себя локально этот файл README.md:
   - впишите вверху название занятия и ваши фамилию и имя;
   - в каждом задании добавьте решение в требуемом виде: текст/код/скриншоты/ссылка;
   - для корректного добавления скриншотов воспользуйтесь инструкцией [«Как вставить скриншот в шаблон с решением»](https://github.com/netology-code/sys-pattern-homework/blob/main/screen-instruction.md);
   - при оформлении используйте возможности языка разметки md. Коротко об этом можно посмотреть в [инструкции по MarkDown](https://github.com/netology-code/sys-pattern-homework/blob/main/md-instruction.md).
4. После завершения работы над домашним заданием сделайте коммит (`git commit -m "comment"`) и отправьте его на Github (`git push origin`).
5. Для проверки домашнего задания преподавателем в личном кабинете прикрепите и отправьте ссылку на решение в виде md-файла в вашем Github.
6. Любые вопросы задавайте в чате учебной группы и/или в разделе «Вопросы по заданию» в личном кабинете.

Желаем успехов в выполнении домашнего задания.

------

### Подготовка к выполнению заданий

1. Подготовка защищаемой системы:

- установите **Suricata**,
- установите **Fail2Ban**.

2. Подготовка системы злоумышленника: установите **nmap** и **thc-hydra** либо скачайте и установите **Kali linux**.

Обе системы должны находится в одной подсети.

------

### Задание 1

Проведите разведку системы и определите, какие сетевые службы запущены на защищаемой системе:

**sudo nmap -sA < ip-адрес >**

**sudo nmap -sT < ip-адрес >**

**sudo nmap -sS < ip-адрес >**

**sudo nmap -sV < ip-адрес >**

По желанию можете поэкспериментировать с опциями: https://nmap.org/man/ru/man-briefoptions.html.


*В качестве ответа пришлите события, которые попали в логи Suricata и Fail2Ban, прокомментируйте результат.*

<details>
<summary>Ответ</summary>  

- Запускаем suricata  

`sudo suricata -c /etc/suricata/suricata.yaml -i enp0s3`

![img](https://github.com/travickiy67/Network-protection/blob/main/img/1.1.png)  

- Запускаем просмотр логов  

![img](https://github.com/travickiy67/Network-protection/blob/main/img/1.2.png)  

*suricata зафиксировала все ппопытки сканирования  

![img](https://github.com/travickiy67/Network-protection/blob/main/img/1.3.png) 

`sudo nmap -sA 192.168.0.8`

Врзможна утечка информации приоритет 2, зафиксировано на какие порты направлена атака и с какими параметрами  

![img](https://github.com/travickiy67/Network-protection/blob/main/img/1.4.png)  

` sudo nmap -sS 192.168.0.8`

Идентичная информация как и припервом сканировании  

![img](https://github.com/travickiy67/Network-protection/blob/main/img/1.6.png)  

`sudo nmap -sT 192.168.0.8`  

Сканирование обнаружено, классификация: Обнаружен сетевой троян, приоритет 1

![img](https://github.com/travickiy67/Network-protection/blob/main/img/1.5.png)  

`sudo nmap -sV 192.168.0.8`

Классификация: Атака веб-приложений

![img](https://github.com/travickiy67/Network-protection/blob/main/img/1.7.png)  

`sudo nmap -sX 192.168.0.8`

Классификация: Попытка утечки информации

![img](https://github.com/travickiy67/Network-protection/blob/main/img/1.8.png)  

`sudo nmap -sU 192.168.0.8`

Классификация: Попытка утечки информации  

![img](https://github.com/travickiy67/Network-protection/blob/main/img/1.8.png)  

*Все попутки сканирования suricata распознала и квалифицировала, Fail2Ban не отреагировала на nmap сканирование*  

</details>

------

### Задание 2

Проведите атаку на подбор пароля для службы SSH:

**hydra -L users.txt -P pass.txt < ip-адрес > ssh**

1. Настройка **hydra**: 
 
 - создайте два файла: **users.txt** и **pass.txt**;
 - в каждой строчке первого файла должны быть имена пользователей, второго — пароли. В нашем случае это могут быть случайные строки, но ради эксперимента можете добавить имя и пароль существующего пользователя.

Дополнительная информация по **hydra**: https://kali.tools/?p=1847.

2. Включение защиты SSH для Fail2Ban:

-  открыть файл /etc/fail2ban/jail.conf,
-  найти секцию **ssh**,
-  установить **enabled**  в **true**.

Дополнительная информация по **Fail2Ban**:https://putty.org.ru/articles/fail2ban-ssh.html.



*В качестве ответа пришлите события, которые попали в логи Suricata и Fail2Ban, прокомментируйте результат.*

<details>
<summary>Ответ</summary>  

*На атакующей машине запускаем hydra, на атакуемой машмне Fail2Ban остановлен*  

`sudo hydra -L users.txt -P pass.txt 192.168.0.8 ssh`  

Пароль и логин подобран  

[img](https://github.com/travickiy67/Network-protection/blob/main/img/2.1.png)  

Логи suricata  

[img](https://github.com/travickiy67/Network-protection/blob/main/img/2.2.png)  

Fail2Ban запущен, первое сканирование ошибки не показала, но пароль не падобран, второе сканирование выдало ошибку. Fail2Ban забанил злоумышленика.  

[img](https://github.com/travickiy67/Network-protection/blob/main/img/2.3.png)  
 
Проверяем логи, обнаружена многократная попытка авторизации   

`sudo tail -f /var/log/auth.log`  

[img](https://github.com/travickiy67/Network-protection/blob/main/img/2.4.png)  

Проверяем логи Fail2ban, попытка взломв заблокирована  

`sudo tail -f /var/log/fail2ban.log`  

[img](https://github.com/travickiy67/Network-protection/blob/main/img/2.5.png)  

</details>
 




