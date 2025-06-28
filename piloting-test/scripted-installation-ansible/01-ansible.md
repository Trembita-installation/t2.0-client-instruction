# 📦 Інсталяція компонентів через Ansible-playbook

Цей розділ описує, як встановити інфраструктурні компоненти системи Трембіта 2.0 за допомогою автоматизованих сценаріїв на базі **Ansible**.

> ⚠️ **Увага!** Перед початком інсталяції переконайтесь, що виконано попередні кроки, описані в [README](README.md) цієї директорії.

## 🔧 Компоненти, які встановлюються

- **UXP Security Server (ШБО)**
- **MinIO**
- **ELK**
- **Graylog**
- **Zabbix**
- **MonHub**

---

## 📁 Структура директорії

| Файл / Директорія                          | Призначення                                      |
|-------------------------------------------|--------------------------------------------------|
| `ansible.cfg`                             | Конфігурація Ansible                             |
| `inventories/test_ansible/infra.yaml`     | Список серверів (IP, hostname)                   |
| `group_vars/all.yaml`                     | Змінні: паролі, користувачі тощо                 |
| `*_install.yaml`                          | Плейбуки для встановлення окремих компонентів    |

--- 

## ⚙️ Вимоги

- ОС: **Ubuntu Server 22.04**
- Встановлений **Ansible 2.12+**
  > **Наприклад**, для встановлення Ansible можна скористатися такою командою:  
  >```bash
  >sudo apt update && sudo apt install -y ansible
  >```
  
- SSH-доступ по ключу до цільових серверів
- Доступ до Інтернету з цільових серверів

## 🚀 Кроки встановлення

1. **Клонувати репозиторій**  
   ```bash
   git clone https://github.com/Trembita-installation/t2.0-client-deployment.git
   ```
2. **Перейти до файлу infra_yaml ( /t2.0-client-deployment.git/inventories/test_ansible/infra_yaml ) через редактор або консоль**
3. **Вписати внутрішні ip та hostname відповідних серверів та зберегти зміни**

   <img width="386" alt="image" src="https://github.com/user-attachments/assets/270e1fb8-c9f0-4306-8211-9cb7d53336d6" /> 

5. **Перейти до файлу all.yaml ( /t2.0-client-deployment.git/inventories/test_ansible/all.yaml ) та вписати логін і пароль до відповідних компонентів для подальших доступів до вебінтерфейсів та інших компонентів**

   <img width="764" alt="image" src="https://github.com/user-attachments/assets/606f5748-1796-430c-9891-6a75df0970f9" />


   

2. **Перейти до директорії зі скриптами**
   ```bash
   cd t2.0-client-deployment/
   ```

3. **Перевірити доступність серверів**
   ```bash
   ansible -i inventories/test_ansible/infra.yaml all -m ping
   ```

4. **Підготувати систему (оновлення пакетів, налаштування репозиторіїв)**
   ```bash
   ansible-playbook -i inventories/test_ansible/infra.yaml repa.yaml
   ```

5. **Встановити окремі компоненти:**

   - **Elasticsearch + Kibana**
     ```bash
     ansible-playbook -i inventories/test_ansible/infra.yaml ek_install.yaml
     ```

   - **MinIO**
     ```bash
     ansible-playbook -i inventories/test_ansible/infra.yaml minio_install.yaml
     ```

   - **Graylog**
     ```bash
     ansible-playbook -i inventories/test_ansible/infra.yaml graylog_install.yaml
     ```

   - **Zabbix**
     ```bash
     ansible-playbook -i inventories/test_ansible/infra.yaml zabbix_install.yaml
     ```

   - **UXP Security Server (ШБО)**
     ```bash
     ansible-playbook -i inventories/test_ansible/infra.yaml ss_install.yaml
     ```

---

## 🛠 Корисні прапорці

Для детального логування:
```bash
ansible-playbook -vvv -i inventories/test_ansible/infra.yaml <playbook>.yaml
```

---

## 🔧 Змінні

Змінні конфігурації розміщено у:
```
inventories/test_ansible/group_vars/all.yaml
```

---

## 📌 Примітка

Інсталяція може виконуватись як **окремо для кожного компонента**, так і **одразу для всіх** (через об’єднаний сценарій або послідовне виконання playbook-файлів).

