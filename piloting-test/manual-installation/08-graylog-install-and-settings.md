# Т2.0 Graylog інсталяція
Graylog допомагає ефективно здійснювати консолідацію, пошук та аналіз у великих об’ємах журнальних даних.

## **Підготовка:**

### **Встановити операційну систему**

Виконати встановлення операційної системи на основі дистрибутиву: Ubuntu Server 22.04 LTS

### **Необхідні для вхідних з’єднань Graylog порти**

| Порт (TCP) | Призначення | Область мережі |
| --- | --- | --- |
| 443 | вебінтерфейс Graylog | ПРИВАТНА |
| 514 | Безпечне ведення системного журналу (TCP з TLS) | ПРИВАТНА |

---

## Крок 1: Налаштування репозиторію

1. Закоментувати репозиторії:

```bash
sudo sed -i 's/^[A-Za-z0-9]/#&/' /etc/apt/sources.list
```

2. Додати GPG-ключ для репозиторію:

```bash
wget -O - https://project-repo.trembita.gov.ua:8081/public-keys/public.key.txt | sudo apt-key add -
```

2. Додати репозиторій:

```bash
echo 'deb https://project-repo.trembita.gov.ua:8081/repository/tr-2-pre-final/ jammy main' | sudo tee -a /etc/apt/sources.list
```

3. Оновлюємо списки пакетів з репозиторіїв:

```bash
sudo apt update
```

---

## Крок 2: Встановлення MongoDB

Для зберігання своїх метаданих, Graylog використовує MongoDB.

1. Встановіть MongoDB:

```bash
sudo apt install -y mongodb-org
```

2. Оновлення конфігурації systemd:

```bash
sudo systemctl daemon-reload
```

3. Додаємо в автозапуск і запускаємо MongoDB:

```bash
sudo systemctl enable --now mongod
```

4. Зафіксуйте щойно встановлену версію пакета MongoDB, щоб не допустити його автоматичного оновлення на новішу версію під час встановлення системних оновлень:

```bash
sudo apt-mark hold mongodb-org
```

---

## Крок 3: Встановлення OpenSearch

1. Встановіть OpenSearch:

OpenSearch при встановленні, вимагає налаштування змінної середовища OPENSEARCH_INITIAL_ADMIN_PASSWORD

```bash
sudo OPENSEARCH_INITIAL_ADMIN_PASSWORD=$(tr -dc A-Z-a-z-0-9_@#%^-_=+ < /dev/urandom | head -c${1:-32}) \
apt -y install opensearch
```

2. Зафіксуйте щойно встановлену версію пакета OpenSearch щоб не допустити його автоматичного оновлення на новішу версію під час встановлення системних оновлень:

```bash
sudo apt-mark hold opensearch
```

## Крок 4: Налаштування OpenSearch

1. Налаштуйте базову конфігурацію:

```bash
sudo nano /etc/opensearch/opensearch.yml
```

Оновіть наступні параметри для забезпечення принаймні мінімально можливого не захищеного режиму роботи (на одному вузлі):

```bash
path.data: /var/lib/opensearch
path.logs: /var/log/opensearch

cluster.name: graylog
node.name: ${HOSTNAME}
discovery.type: single-node
network.host: 127.0.0.1
action.auto_create_index: false
plugins.security.disabled: true
```

2. Змініть налаштування JVM:

```bash
sudo nano /etc/opensearch/jvm.options
```

Оновіть налаштування **Xms** & **Xmx** встановивши значення рівними половині наявної у системі оперативної пам’яті, наприклад 4 Гігабайта:

```bash
-Xms4g
-Xmx4g
```

3. Налаштуйте параметри ядра:

   Тимчасова зміна (до перезавантаження):

```bash
sudo sysctl -w vm.max_map_count=262144
```

Постійна зміна (зберігається після перезавантаження):

```bash
echo 'vm.max_map_count=262144' | sudo tee -a /etc/sysctl.conf
```

4. Оновити конфігурацію systemd:

```bash
sudo systemctl daemon-reload
```

5. Увімкнути та запустити службу OpenSearch:

```bash
sudo systemctl enable --now opensearch.service
```

---

## Крок 4: Встановлення Graylog

1. Встановіть:

```bash
sudo apt install graylog-server
```

2. Зафіксуйте версію пакету, щоб він не оновлювався випадково під час оновлень серверу Graylog:

```bash
sudo apt-mark hold graylog-server
```

---

## Крок 5: Налаштування Graylog

### Генерування сертифікатів для безпечного з’єднання через HTTPS

1. Створіть каталог для сертифікату:

```bash
sudo mkdir -p /opt/graylog/ssl/site
```

2. Створіть конфігураційний файл для видачі самопідписаного сертифікату:

```bash
sudo nano /opt/graylog/ssl/site.conf
```

Вкажіть дані для сертифікату і не забудьте замінити значення **\<graylog-server-address\>** (у двох місцях) та **\<graylog-server-ip\>** новими значеннями:
<span style="background-color:mistyrose;">Не заповнені поля прибрати</span>

```bash
[req]
distinguished_name = req_distinguished_name
x509_extensions = v3_req
prompt = no

# Details about the issuer of the certificate
[req_distinguished_name]
C = EE
ST = Some-State
L = Some-City
O = Organization
OU = Department
CN = <graylog-server-address>

[v3_req]
keyUsage = nonRepudiation, digitalSignature, keyEncipherment
extendedKeyUsage = serverAuth
subjectAltName = @alt_names

# IP addresses and DNS names the certificate should include
# Use IP.### for IP addresses and DNS.### for DNS names,
# with "###" being a consecutive number.
[alt_names]
IP.1 = <graylog-server-ip>
DNS.1 = <graylog-server-address>
```

3. Згенеруйте самопідписаний сертифікат:

```bash
sudo openssl req -x509 -newkey ec -pkeyopt ec_paramgen_curve:P-256 \
-config /opt/graylog/ssl/site.conf \
-keyout /opt/graylog/ssl/site/site.key \
-out /opt/graylog/ssl/site/site.crt \
-days 3650 -nodes
```

4. Змініть власника для ключа та сертифікату на graylog. Серверний процес повинен мати доступ до цих файлів:

```bash
sudo chown graylog:graylog /opt/graylog/ssl/site/site.*
```

### Додавання сертифікату в довірене сховище JVM

1. Створіть копію стандартного сховища ключів. Збережіть сертифікати у якомусь іншому місці, щоб оновлення чи видалення Java не мало на них впливу:

```bash
sudo cp /usr/share/graylog-server/jvm/lib/security/cacerts /opt/graylog/ssl/graylog.jks
```

2. Змініть власника сховища ключів на graylog:

```bash
sudo chown graylog:graylog /opt/graylog/ssl/graylog.jks
```

3. Імпортуйте сертифікат серверу в сховище ключів:

```bash
sudo /usr/share/graylog-server/jvm/bin/keytool -importcert \
-keystore /opt/graylog/ssl/graylog.jks -storepass changeit \
-alias graylog-https -file /opt/graylog/ssl/site/site.crt
```

Перегляньте сертифікат, введіть **yes** , якщо інформація коректна, натисніть клавішу **Enter**.

4. Додайте копію сховища ключів у параметри Graylog JVM.

```bash
sudo nano /etc/default/graylog-server
```

Додайте наступний рядок після останньої змінної GRAYLOG_SERVER_JAVA_OPTS:

```bash
GRAYLOG_SERVER_JAVA_OPTS="$GRAYLOG_SERVER_JAVA_OPTS -Djavax.net.ssl.trustStore=/opt/graylog/ssl/graylog.jks"
```

### Генерування паролю для Graylog

Щоб створити значення для вашого параметру **password_secret**, запустіть таку команду:

```bash
< /dev/urandom tr -dc A-Z-a-z-0-9 | head -c${1:-96};echo;
```

Створіть для паролю параметр **root_password_sha2**.

```bash
echo -n "Enter Password: " && head -1 </dev/stdin | tr -d '\n' | sha256sum | cut -d" " -f1
```

Обидва цих значення необхідні для виконання наступного кроку.

### Коригування конфігураційного файлу Graylog

1. Відкрийте конфігураційний файл Graylog:

```bash
sudo nano /etc/graylog/server/server.conf
```

Вкажіть значення для наступних параметрів:

Не забудьте замінити значення **\<generated password\>**, **\<hash of generated password\>** та **\<graylog-server-address\>** актуальними даними.

```bash
password_secret = <generated password>
root_username = admin
root_password_sha2 = <hash of generated password>
root_timezone = Europe/Kyiv
http_bind_address = 0.0.0.0:443
http_publish_uri = https://<graylog-server-address>/
http_enable_tls = true
http_tls_cert_file = /opt/graylog/ssl/site/site.crt
http_tls_key_file = /opt/graylog/ssl/site/site.key
elasticsearch_hosts = http://127.0.0.1:9200
```

2. Оновити конфігурацію systemd:

```bash
sudo systemctl daemon-reload
```

3. Увімкнути та запустити службу Graylog:

```bash
sudo systemctl enable --now graylog-server.service
```

Після цього, вебінтерфейс Graylog має бути доступний за адресою *https://\<graylog-server-address\>/*.

Використайте дані користувача root, визначені раніше у конфігураційному файлі, а саме, **root_username** для імені користувача та **password_secret** для паролю.

## Крок 6: Налаштування вхідних даних Graylog

### Генерування сертифікатів для TLS

1. Створіть каталог для сертифікатів:

```bash
sudo mkdir -p /opt/graylog/ssl/inputs
```

2. Згенеруйте сертифікати:

```bash
sudo openssl req -x509 -config /opt/graylog/ssl/site.conf \
-newkey ec -pkeyopt ec_paramgen_curve:P-256 \
-keyout /opt/graylog/ssl/inputs/input_rsyslog.key \
-out /opt/graylog/ssl/inputs/input_rsyslog.crt \
-days 3650 -nodes
```

3. Змініть власника для ключа та сертифікату на graylog:

```bash
sudo chown graylog:graylog /opt/graylog/ssl/inputs/input_rsyslog.*
```

4. Створіть каталог для зберігання сертифікатів компонентів. У наступних кроках ми додамо у нього сертифікати для взаємного TLS між rsyslog та Graylog:

```bash
sudo mkdir -p /opt/graylog/ssl/certs
```

### Налаштування прослуховування вхідних даних для Graylog

Graylog потребує налаштувань для приймання журналів з різних джерел. Щоб налаштувати вхідні дані у вебінтерфейсі Graylog:

1. Перейдіть у **System** \> **Inputs**.

![](08-graylog-image/image.png)

2. Натисніть **Select input** та оберіть **Syslog TCP**.
   та натисніть **Launch New Input**.

![](08-graylog-image/image1.png)

3. У вікні, що з’явиться, заповніть обов’язкові поля:

◦ Title - щось для ідентифікації цих вхідних даних, наприклад, **Component Syslog Inputs**

◦ TLS cert file - **/opt/graylog/ssl/inputs/input_rsyslog.crt**

◦ TLS private key file - **/opt/graylog/ssl/inputs/input_rsyslog.key**

◦ Ввімкніть позначку **Enable TLS**;

◦ Оберіть для TLS client authentication опцію **required**;

◦ TLS Client Auth Trusted Certs - **/opt/graylog/ssl/certs**;

◦ Ввімкніть позначку **Expand structured data?** для розшифровки структурованих даних;

◦ Оберіть для значення **Time Zone** коректний часовий пояс.

![](08-graylog-image/image9.png)

![](08-graylog-image/image10.png)

![](08-graylog-image/image11.png)

## Крок 7: Налаштування Rsyslog на сервері компоненту

Пересилання журналів за допомогою Rsyslog

1. ### Імпортуйте сертифікат Graylog на ваш сервер компоненту.

   Для забезпечення взаємно автентифікованих з’єднань, вам потрібно отримати та скопіювати сертифікат Graylog на сервер компоненту. Адміністратор серверу Graylog може знайти сертифікат за таким шляхом:
   **/opt/graylog/ssl/inputs/input_rsyslog.crt**
   Цей сертифікат необхідно скопіювати на ваш сервер компоненту та розмістити його за таким шляхом:
   **/etc/uxp/ssl/graylog_remote.crt**

   > **Приклад передачі сертифіката** за допомогою команди scp.
   >
   > Скопіюємо в домашній каталог.
   >
   > ```bash
   > sudo cp /opt/graylog/ssl/inputs/input_rsyslog.crt .
   > ```
   >
   > Змінюємо власника
   >
   > ```bash
   > sudo chown $USER:$USER input_rsyslog.crt
   > ```
   >
   > І передамо сертифікат.
   >
   > ```bash
   > scp user@192.168.0.30:~/input_rsyslog.crt user@192.168.0.20:~
   > ```
   >
   > **На ШБО**
   >
   > Перемістимо переданий сертифікат в каталог
   >
   > ```bash
   > sudo mv input_rsyslog.crt /etc/uxp/ssl/graylog_remote.crt
   > ```

2. ### Дозвольте доступ rsyslog до сертифікату Graylog:

```bash
sudo chown root:uxp /etc/uxp/ssl/graylog_remote.crt
```

3. ### Експортуйте ваш сертифікат rsyslog на Graylog.

Скопіюйте згенерований сертифікат (**/etc/uxp/ssl/rsyslog_remote.crt**) із вашого серверу компоненту і передайте його адміністратору серверу Graylog. Далі, цьому адміністратору потрібно розмістити ваш сертифікат серверу компоненту в каталог /opt/graylog/ssl/certs/ на сервері Graylog.

> **Приклад передачі сертифіката** за допомогою команди scp.
>
> ```bash
> sudo cp /etc/uxp/ssl/rsyslog_remote.crt .
> ```
>
> Змінимо власника.
>
> ```bash
> sudo chown $USER:$USER rsyslog_remote.crt
> ```
>
> Передамо сертифікат.
>
> ```bash
> scp user@192.168.0.30:~/rsyslog_remote.crt user@192.168.0.20:~
> ```
>
> **На сервері Graylog**
>
> Переміщяємо
>
> ```bash
> sudo mv rsyslog_remote.crt /opt/graylog/ssl/certs/gate.crt
> ```

Змінимо власника:

```bash
sudo chown root:root /opt/graylog/ssl/certs/gate.crt
```

І привілеї:

```bash
sudo chmod 644 /opt/graylog/ssl/certs/gate.crt
```

### На ШБО налаштовуємо підключення до Graylog

Перш ніж змінювати файл конфігурації, призупиніть роботу контролера цілісності на сервері компоненту, за допомогою такої команди:

1. Тимчасово призупиніть роботу контролера цілісності:

```bash
sudo uxp-integrity pause
```

2. Налаштуйте адресу до Graylog:

```bash
sudo nano /etc/rsyslog.d/45-uxp-graylog.conf.example
```

Замініть значення **\<graylog-server-address\>** на IP або DNS адресу серверу Graylog (значення має співпадати із значенням параметру CN у сертифікаті Graylog). Є два параметри, в яких вам потрібно вставити це значення: **target** та **StreamDriverPermittedPeers**.

3. Перейменуйте конфігураційний файл:
   Щоб конфігурація почала діяти, вам необхідно видалити .example у кінці назви файлу.

```bash
sudo mv /etc/rsyslog.d/45-uxp-graylog.conf.example /etc/rsyslog.d/45-uxp-graylog.conf
```

4. Перезапустіть rsyslog:

```bash
sudo systemctl restart rsyslog
```

5. Оновіть контролера цілісності:

```bash
sudo uxp-integrity update
```
