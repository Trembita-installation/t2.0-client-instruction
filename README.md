# Пілотування системи Трембіта 2.0

## 1. Призначення репозиторію та зміст

Цей репозиторій призначено для технічного супроводу пілотного впровадження системи Трембіта 2.0. Він містить повний комплект документації, інструкцій та шаблонів, необхідних Учасникам пілотування.

---

## 2. Регламент пілотування

- 📅 **Тривалість:** 90 днів, починаючи з **07.07.2025**

- 👥 **Держатель системи:** Міністерство цифрової трансформації України — забезпечує організаційний супровід

- 👥 **Адміністратор системи:** ДП «ДІЯ» — відповідає за розгортання ядра системи, адміністрування, моніторинг та підготовку звіту

- 👥 **Учасники пілотування:** організації, які долучилися до тестування — відповідають за налаштування компонентів, подання заявок і звітування про проблеми

- 🎯 **Ключова ціль:**

    - Тестування системи Трембіта 2.0 в умовах, наближених до промислової експлуатації

- 🎯 **Підцілі пілотування:**

    - Тестування міжнародних криптографічних алгоритмів (ECDSA)
      
    - Перевірка режиму *multi-tenancy*

> 📖 [Ознайомитись із повним регламентом](02_PILOTING-GUIDE.md)

---

## 3. Структура репозиторію

- [/piloting-test](piloting-test) — включає два основні підкаталоги:

  - [/manual-installation](piloting-test/manual-installation) — покрокові інструкції з ручного встановлення компонентів:
  
    - [01-env-components.md](piloting-test/manual-installation/01-env-components.md) — вимоги до компонентів
    - [02-network-diagram.md](piloting-test/manual-installation/02-network-diagram.md) — мережева схема
    - [03-adminserver-install.md](piloting-test/manual-installation/03-adminserver-install.md) — встановлення AdminTools
    - [04-minio-install-and-settings.md](piloting-test/manual-installation/04-minio-install-and-settings.md) — встановлення та налаштування MinIO
    - [05-uxp-ss-install.md](piloting-test/manual-installation/05-uxp-ss-install.md) — встановлення ШБО
    - [06-uxp-ss-settings.md](piloting-test/manual-installation/06-uxp-ss-settings.md) — налаштування ШБО
    - [06.1-uxp-service-settings.md](piloting-test/manual-installation/06.1-uxp-service-settings.md) — налаштування сервісів в ШБО
    - [06.2-uxp-ss-user-guide.md](piloting-test/manual-installation/06.2-uxp-ss-user-guide.md) — опціональні налаштування ШБО  
    - [07-elk-install-and-settings.md](piloting-test/manual-installation/07-elk-install-and-settings.md) — встановлення та налаштування ELK
    - [08-graylog-install-and-settings.md](piloting-test/manual-installation/08-graylog-install-and-settings.md) — встановлення та налаштування Graylog
    - [09-zabbix-install-and-settings.md](piloting-test/manual-installation/09-zabbix-install-and-settings.md) — встановлення та налаштування Zabbix
    - [10-mon-hub-install-and-settings.md](piloting-test/manual-installation/10-mon-hub-install-and-settings.md) — встановлення та налаштування MonHub
    - [README.md](piloting-test/manual-installation/README.md) — вступ до ручного встановлення

  - [/scripted-installation-ansible](piloting-test/scripted-installation-ansible) — інструкції зі встановлення компонентів через Ansible:

    - [01-ansible.md](piloting-test/scripted-installation-ansible/01-ansible.md) — повна інструкція з використання Ansible-playbook
    - [02-uxp-ss-settings.md](piloting-test/scripted-installation-ansible/02-uxp-ss-settings.md) — налаштування ШБО після встановлення
    - [02.1-uxp-service-settings.md](piloting-test/scripted-installation-ansible/02.1-uxp-service-settings.md) — налаштування сервісів на ШБО
    - [README.md](piloting-test/scripted-installation-ansible/README.md) — вступ до встановлення з використання Ansible-playbook

- [01_VYMOGY.md](01_VYMOGY.md) — технічні вимоги для пілотування системи
- [02_PILOTING-GUIDE.md](02_PILOTING-GUIDE.md) — повний регламент і правила пілотування
- [03_FORMS.md](03_FORMS.md) — перелік форм заявок, які потрібно заповнити в процесі налаштування та реєстрації компонентів
- [04_CATALOG_SIMULATION.md](04_CATALOG_SIMULATION.md) — симуляція Каталогу
- [README.md](README.md) — головний файл з описом структури і змісту всього репозиторію

---

## 4. Як розпочати пілотування

1. Ознайомтесь із [Регламентом пілотування](02_PILOTING-GUIDE.md)

2. Ознайомтесь із [Технічними вимогами для пілотування системи](01_VYMOGY.md)

3. Перевірте [вимоги до середовища](piloting-test/manual-installation/01-env-components.md)

4. Оберіть спосіб встановлення: [ручне](piloting-test/manual-installation) або [через Ansible](piloting-test/scripted-installation-ansible)

5. Почніть інсталяцію відповідно до обраного способу

6. Подайте заявки через відповідні [форми](03_FORMS.md)

7. Налаштуйте компоненти згідно з інструкціями

8. (Опціонально) Опублікуйте сервіси

9. Перевірте роботу ШБО

10. Почніть тестову інформаційну взаємодію

11. У разі потреби звертайтесь до Адміністратора системи

---

## 5. 📩 Комунікація

У разі виникнення питань:

- ✉️ Email: [trembita@diia.gov.ua](mailto:trembita@diia.gov.ua)
- 💬 Чат: персональний чат у WhatsApp з Адміністратором
