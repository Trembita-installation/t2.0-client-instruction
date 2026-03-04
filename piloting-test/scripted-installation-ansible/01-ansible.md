# 📦 Інсталяція компонентів через Ansible-playbook

Цей розділ описує, як встановити інфраструктурні компоненти системи Трембіта 2.0 за допомогою автоматизованих сценаріїв на базі **Ansible**.

> ⚠️ **Увага!** Перед початком інсталяції переконайтесь, що виконано попередні кроки, які описані в [README](README.md) цієї директорії.

> ## 📌 Примітка
>Інсталяція може виконуватись як **окремо для кожного компонента**, так і **одразу для всіх** (через об’єднаний сценарій або послідовне виконання playbook-файлів).

## 🔧 Компоненти, які встановлюються за допомогою Ansible-playbook

- **UXP Security Server (ШБО)**
- **MinIO**
- **ELK**
- **Graylog**
- **Zabbix**
- **FILEBEAT (для MonHub рішення)**
- **Zabbix Agent2 (для MonHub рішення)**

---

## 📁 Структура директорії

| Файл / Директорія                         | Призначення                                      |
|-------------------------------------------|--------------------------------------------------|
| `ansible.cfg`                             | Конфігурація Ansible                             |
| `inventories/sample/infra.yaml`           | Список серверів (IP, hostname)                   |
| `group_vars/all.yaml`                     | Змінні: паролі, користувачі тощо                 |
| `*_install.yaml`                          | Плейбуки для встановлення окремих компонентів    |

--- 

## ⚙️ Вимоги

- Встановлений **Ansible 2.12+**
  > **Наприклад**, для встановлення Ansible можна скористатися такою командою:  
  >```bash
  >sudo apt update && sudo apt install -y ansible
  >```
  
- SSH-доступ по **ключу** до цільових серверів
- Доступ до Інтернету з цільових серверів

## 🚀 Кроки встановлення

1. **Підготовлена інфраструктура по вимогам:**
    - [Склад, призначення та мінімальні апаратні характеристики компонентів](../manual-installation/01-env-components.md))
    - [Мережева схема підключення](../manual-installation/02-network-diagram.md)


2. **Створити локально папку** з назвою `t2_instal`

3. **Перейти** в папку `t2_instal`

4. **Клонувати репозиторій**  

   ```bash
   git clone https://github.com/Trembita-installation/t2.0-client-deployment.git .
   ```
5. **Перейменуйте папку** `sample` ( /t2.0-client-deployment.git/inventories/**sample** ) в назву Вашого проєкту. І від назви Вашого проєкту змінювати назву в наступних файлах /t2.0-client-deployment.git/inventories/_name_your_project_

6. **Перейти до файлу** `infra_yaml` ( /inventories/_name_your_project_/infra_yaml ) через редактор або консоль

7. **Вписати** внутрішні `ip` та `hostname` відповідних серверів та зберегти зміни

   <img width="406" alt="image" src="https://github.com/user-attachments/assets/2acc046a-db76-4f17-8186-afe6eb43acfe" />

8. **Перейти до файлу** `all.yaml` ( /inventories/_name_your_project_/group_vars/all.yaml ) та вписати `логіни` і `паролі` для подальших доступів до вебінтерфейсів та інших компонентів

> ⚠️ **Увага!** Для налаштування **MonHub** в файлі `all.yaml` треба додати `id`, які надає Адміністратор системи Трембіта
> ```bash
> ######## FILEBEAT ###############
> project_id: "PROVIDED_BY_SUPPORT"
> filebeat_id: "PROVIDED_BY_SUPPORT"
> filebeat_api_key: "PROVIDED_BY_SUPPORT"
> #################################
> ######### MON_ZAB ###############
> zabbix_org: "PROVIDED_BY_SUPPORT"
> zabbix_psk: "PROVIDED_BY_SUPPORT"
> #################################
> ```


> ⚠️ **Увага!** Підтримується зміна імен юзерів, окрім юзера для **SS**
  
  <img width="675" alt="image" src="https://github.com/user-attachments/assets/6d453482-5acc-4d6d-a404-e451c3eb05e2" />

9. **Перейти до директорії** через консоль з Ansible-playbook 

   ```bash
   cd t2_instal/
   ```

10. **Перевірити доступність серверів**

    ```bash
    ansible -i inventories/_name_your_project_/infra.yaml all -m ping --private-key=/path/to/ssh_private_key -u username
    ```
   
11. **Додати репозиторій** на всі сервери
   
   ```bash
   ansible-playbook  -u  your user -i inventories/_name_your_project_/infra.yaml --private-key=/path/your/key repa.yaml
   ```
> ℹ️ **Примітка:**  `/path/your/key` - це шлях до Вашого приватного ключа з яким Ви підключаєтесь до серверів


12. **Встановити компоненти** в наступній послідовності:

  - **FILEBEAT для MonHub рішення**
     ```bash
     ansible-playbook  -u  your user -i inventories/_name_your_project_/infra.yaml --private-key=/path/your/key filebeat_install.yaml
     ```

  - **Zabbix Agent2 для MonHub рішення**
     ```bash
     ansible-playbook  -u  your user -i inventories/_name_your_project_/infra.yaml --private-key=/path/your/key mon_zabbix.yaml
     ```

  - **UXP Security Server (ШБО)**

    ```bash
    ansible-playbook  -u  your user -i inventories/_name_your_project_/infra.yaml --private-key=/path/your/key ss_install.yaml
    ```

  - **MinIO**

     ```bash
     ansible-playbook  -u  your user -i inventories/_name_your_project_/infra.yaml --private-key=/path/your/key minio_install.yaml
     ```
[Після встановлення MinIO перевірте даний параметр](../[manual-installation/04-minio-install-and-settings.md#перевірка-параметра-archive-storage-type](https://github.com/Trembita-installation/t2.0-client-instruction/blob/main/piloting-test/manual-installation/04-minio-install-and-settings.md#%D0%BF%D0%B5%D1%80%D0%B5%D0%B2%D1%96%D1%80%D0%BA%D0%B0-%D0%BF%D0%B0%D1%80%D0%B0%D0%BC%D0%B5%D1%82%D1%80%D0%B0-archive-storage-type)

  - **Elasticsearch + Kibana**

     ```bash
     ansible-playbook  -u  your user -i inventories/_name_your_project_/infra.yaml --private-key=/path/your/key ek_install.yaml
     ```

> ⚠️ **Увага!** Під час виконання `ansible-playbook` **пароль** до вебінтерфейсу Kibana генерується автоматично і відображається у виводі після **успішного завершення**. За потреби пароль можна змінити.

   
  - **Zabbix**

     ```bash
     ansible-playbook  -u  your user -i inventories/_name_your_project_/infra.yaml --private-key=/path/your/key zabbix_install.yaml
     ```
     
  - **Graylog**
    
     ```bash
     ansible-playbook  -u  your user -i inventories/_name_your_project_/infra.yaml --private-key=/path/your/key graylog_install.yaml
     ```
---

## 🖥️ Довідник посилання для доступу до компонентів

- **UXP Security Server (ШБО)**

    ```bash
    https://<security-server>:4000
    ```
> 🔐 **Примітка** <br>
> Логін - `uxpadmin` <br>
> Пароль задається у файлі `all.yaml` <br>

- **MinIO**

     ```bash
     https://<minio-server>:9001
     ```

> 🔐 **Примітка** <br>
> Логін за замовчуванням - `minio` (Актуальний логін задається у файлі `all.yaml`) <br>
> Пароль задається у файлі `all.yaml` <br>


- **Elasticsearch + Kibana**

     ```bash
     https://<ek-server>:5601
     ```
> 🔐 **Примітка** <br>
> Логін - `elastic` <br>
> Пароль генерується автоматично і відображається у виводі після **успішного завершення** `ansible-playbook` <br>

  
- **Zabbix**

     ```bash
     http://<zabbix-server>:8080
     ```

> 🔐 **Примітка** <br>
> Логін - `Admin` <br>
> Пароль - `zabbix` <br>

     
- **Graylog**
    
     ```bash
     https://<graylog-server>
     ```

> 🔐 **Примітка** <br>
> Логін - `admin` <br>
> Пароль задається у файлі `all.yaml` <br>

