# Інструкція з налаштування MonHub-клієнта

> 🛠️ **Це рішення встановлюється на всі сервери, що беруть участь у пілотуванні (крім адмін-станцій).**
>
> 📄 Усі організації надали офіційні згоди на моніторинг, і дані збираються виключно в межах цих згод.

Використовується `zabbix-agent2` у режимі *active checks*, з автоматичною реєстрацією на сервері через `HostMetadata`.

---

## ✅ 1. Встановлення Zabbix agent2 (Ubuntu 22.04)

```bash
wget https://repo.zabbix.com/zabbix/7.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_7.0-1+ubuntu22.04_all.deb
sudo dpkg -i zabbix-release_7.0-1+ubuntu22.04_all.deb
sudo apt update
sudo apt install zabbix-agent2 zabbix-agent2-plugin-*
```

> 💡 Пакети також будуть доступні у внутрішньому репозиторії Трембіта.

---

## ✅ 2. Налаштування Zabbix Agent2

Відредагуйте файл конфігурації:

```bash
sudo nano /etc/zabbix/zabbix_agent2.conf
```

### Основні параметри:

```ini
PidFile=/var/run/zabbix/zabbix_agent2.pid
LogFile=/var/log/zabbix/zabbix_agent2.log
LogFileSize=0

ServerActive=zabt2.trembita.gov.ua
HostnameItem=system.hostname
HostMetadata=type={{ zabbix_client_type }};component={{ zabbix_component }};org={{ zabbix_org }}

TLSConnect=psk
TLSAccept=psk
TLSPSKIdentity=psk-client-identity
TLSPSKFile=/etc/zabbix/zabbix_agent2.psk

Include=/etc/zabbix/zabbix_agent2.d/*.conf
PluginSocket=/run/zabbix/agent.plugin.sock
ControlSocket=/run/zabbix/agent.sock
Include=/etc/zabbix/zabbix_agent2.d/plugins.d/*.conf
```

> 📌 **HostMetadata** повинен відповідати шаблонам `autoregistration`:
> - `type`: `heavy` або `light`:  
>    - `heavy` — використовується для серверів із рекомендованими ресурсами
>    - `light` — використовується для серверів із мінімально допустимими ресурсами
> - `component`: `ss`, `minio`, `ek`, `graylog`, `zabbix`  
> - `org`: код ЄДРПОУ


> 💡 **Приклад HostMetadata для серверу MinIO:** <br>
> HostMetadata=type=heavy;component=minio;org=43395033
---

## ✅ 3. Автоматичне визначення типу (через Ansible, optional)

```yaml
zabbix_client_type: "{{ 'heavy' if ansible_processor_vcpus > 4 else 'light' }}"
zabbix_component: ss
zabbix_org: 43395033
```

---

## ✅ 4. Перезапуск і автозапуск

```bash
sudo systemctl restart zabbix-agent2
sudo systemctl enable zabbix-agent2
```

---

## ✅ 5. Перевірка логів агента

```bash
tail -f /var/log/zabbix/zabbix_agent2.log
```

Очікувані повідомлення:

```
active checks on server [194.42.200.134:10051]: OK
sending metadata: type=heavy;component=ss;org=43395033
```

---

## ✅ 6. Налаштування TLS (PSK)

1. Отримайте ключ `PSK` від адміністратора. Збережіть його в файл:

```bash
/etc/zabbix/zabbix_agent2.psk
```

2. Налаштуйте права доступу:

```bash
chown zabbix:zabbix /etc/zabbix/zabbix_agent2.psk
chmod 600 /etc/zabbix/zabbix_agent2.psk
```

3. Перезапустіть агента:

```bash
sudo systemctl restart zabbix-agent2
```

---



## ✅ 8. Встановлення та налаштування Filebeat

> Filebeat дозволяє передавати журнали подій з компонентів системи до центрального лог-сховища (Elasticsearch).  
> Ми збираємо лише дозволені типи логів. Логи з адмін-станцій **не збираються**.

### 🔹 Крок 1: Завантаження та встановлення Filebeat

```bash
curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-8.18.1-amd64.deb
sudo dpkg -i filebeat-8.18.1-amd64.deb
```

---

### 🔹 Крок 2: Налаштування конфігурації

Відредагуйте файл:

```bash
sudo nano /etc/filebeat/filebeat.yml
```

Використайте підготовлений шаблон конфігурації, який:
- збирає логи `syslog`, `postgresql`, `uxp`;
- виключає технічний шум (CRON, audit, NetworkManager тощо);
- зберігає ключові поля: `project_id`, `host_role`, `log_type`, `log_file`;
- надсилає дані через TLS до Trembita Elasticsearch.

**Важливо:** `project_id: "DaVinci"` має бути незмінним.

---

### 🔹 Крок 3: Сертифікат довіри та API ключ

1. Додайте файл CA-сертифіката:

```bash
/etc/filebeat/ca.crt
```

2. Додайте API Key (буде надано адміністраторами) до `filebeat.yml` в секції `output.elasticsearch`.

---

### 🔹 Крок 4: Запуск Filebeat

```bash
sudo systemctl enable filebeat
sudo systemctl start filebeat
```

Перевірка статусу:

```bash
sudo systemctl status filebeat
tail -f /var/log/filebeat/filebeat
```

---

## ✅ Підсумки щодо моніторингу

- Логи збираються лише на дозволених серверах.
- Усі журнали — в межах офіційних згод організацій.
- MonHub використовується **тільки під час пілоту**.
## ✅ 7. Очікувана поведінка на сервері

Після запуску агент:

- надсилає своє ім’я хоста та метадані;
- автоматично реєструється на Zabbix-сервері;
- отримує відповідний шаблон (`heavy` або `light`);
- потрапляє до 3 груп: за типом, компонентом, організацією;
- має відповідні теги для дашбордів;
- встановлює захищене TLS-з’єднання з сервером (PSK).

---

## ✅ 8. Встановлення Filebeat

```bash
curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-8.18.1-amd64.deb
sudo dpkg -i filebeat-8.18.1-amd64.deb
```

---

## ✅ 9. Налаштування filebeat.yml

> Зверніть увагу: `project_id: "DaVinci"` — це назва проєкту і має залишатись незмінною.

Зберігайте сертифікат у `/etc/filebeat/ca.crt`

> 🔑 Отримайте `api_key` та `ca.crt` від адміністратора Trembita.

---

## ✅ 10. Запуск та перевірка Filebeat

```bash
sudo systemctl enable filebeat
sudo systemctl start filebeat
sudo systemctl status filebeat
tail -f /var/log/filebeat/filebeat
```

---

## ℹ️ Додатково

- **Журнали з адмін-станцій не збираються.**
- Усі дані — лише в межах погодженого з організаціями переліку.
- MonHub використовується лише під час **пілоту** для покращення системи.
