# ЕТАП 1. Підготовчі дії

* 1.1. ОС та вимоги, порти.

| OC | RAM | ROM | CPU |
| --- | --- | --- | --- |
| Ubuntu Server 22.04 LTS (64-bit) | 4/16 | 40/200 | 2/4 |

| **Порти** |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Вхідні |  | 4000 | 2082 | 80 | 443 | 4001 |  |  |  |  |
| Вихідні |  | 514 | 9000 | 80 | 443 | 4001 | 10051 | 8080 | 9200 | 5500 |



# ЕТАП 2. Встановлення UXP

**2.1 Налаштування HOSTNAME :**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2.1.1.  Для перевірки імені хоста

```c
hostname -A 
```

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2.1.2. Якщо імʼя хоста не співпадає:

```
sudo hostnamectl set-hostname <your-hostname>
```

**2.2. Додати ключ та репозиторій UXP**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2.2.1. Закоментуйте репозиторії

```
sudo sed -i 's/^[A-Za-z0-9]/#&/' /etc/apt/sources.list
```

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2.2.2. Додати актуальний репозиторій

```
echo 'deb https://project-repo.trembita.gov.ua:8081/repository/tr-2-pre-final/ jammy main' | sudo tee -a /etc/apt/sources.list
```

![](зображення.png){width=70%}

**2.3. Встановити сервер UXP**

```
sudo apt update
```

```
sudo apt install uxp-securityserver-trembita
```

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2.3.1.  Натиснути "Y" щоб продовжити встановлення

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2.3.2. Ввести користувача та пароль адміністратора

![](зображення1.png){width=70%}

![](зображення2.png)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2.3.3. **Вказати параметри MinIO: URL, bucket, access key, secret key**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:red;">(Якщо MinIO не був встановлений, можна пропустити цей крок і потім додати значення в файл local.ini)</span>

![](зображення3.png){width=70%}

![](зображення4.png){width=70%}

(Замість \<ip-adress-or-hostname\> вказати дійсну ip minio  )



2.3.4. Вказати імʼя bucket , який був створений на minio

![](зображення5.png){width=70%}

2.3.5.  Ввести access key

![](зображення6.png)

2.3.6. Ввести secret key

![](зображення7.png)

2.3.7. Вказати шлях до сертифікату

![](зображення8.png){width=70%}

2.3.8. Натиснути ОК

![](зображення9.png){width=70%}

2.3.9. Натиснути ОК

![](зображення10.png)



---

### 

# ЕТАП 3. Контролер цілісності (AIDE)

3.1. Встановлення контролю цілісності

```
sudo apt install uxp-integrity-securityserver 
```

3.2. Поставити uxp-integrity на паузу

```
sudo uxp-integrity pause 
```

3.3. Перейти до файлу allowed_hosts.conf  та додати ip

```
nano /etc/uxp/nginx/allowed_hosts.conf
```

3.4. Зберегти файл та викнонати наступну команду

```
service nginx restart 
```

3.5. Виконати оновлення uxp-integrity

```
sudo uxp-integrity update
```

3.6. Перевірка сервісів

```
systemctl status uxp-*
```

3.7. Перейдіть на сторінку https://ip-uxp:4000







Для підключення Гряди:



sudo uxp-integrity pause

sudo apt install pcscd libccid pcsc-tools libpcsclite1 opensc



sudo unzip -o -j NCMGryada301PKCS11Libs-Linux.zip -d /usr/share/uxp/lib/

sudo chown root:root /usr/share/uxp/lib/\*.so

sudo chmod 644 /usr/share/uxp/lib/\*.so



nano /usr/share/uxp/lib/osplm.ini


