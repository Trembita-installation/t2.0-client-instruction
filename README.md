# Пілотування системи Трембіта 2.0

## 1. Призначення репозиторію та зміст

Цей репозиторій призначено для технічного супроводу пілотного впровадження системи Трембіта 2.0. Він містить повний комплект документації, інструкцій та шаблонів, необхідних Учасникам пілотування.

---

## 2. Регламент пілотування

- 📅 **Тривалість:** 90 днів, починаючи з **07.07.2025**

- 👥 **Держатель системи:** Міністерство цифрової трансформації України — забезпечує організаційний супровід

- 👥 **Адміністратор системи:** ДП «ДІЯ» — відповідає за розгортання ядра системи, адміністрування, моніторинг та підготовку звіту

- 👥 **Учасники пілотування:** організації, які долучилися до тестування — відповідають за налаштування компонентів, подання заявок і звітування про проблеми

- 🎯 **Цілі пілотування:**

  - Інсталяція системи Трембіта 2.0 в умовах, наближених до промислової експлуатації

  - Тестування системи Трембіта 2.0 в умовах, наближених до промислової експлуатації
  
  - Перевірка режиму *multi-tenancy*

  - Використання міжнародних криптографічних алгоритмів (ECDSA)

  - Побудова інформаційної взаємодії між учасниками

  - Збір аналітики та зворотного зв’язку щодо функціонування системи

> 📖 [Ознайомитись із повним регламентом](PILOTING-GUIDE.md)

---

## 3. Структура репозиторію

- [/piloting-test](t2.0-client-instruction/piloting-test) — включає два основні підкаталоги:

  - [/manual-installation](t2.0-client-instruction/piloting-test/manual-installation) — покрокові інструкції з ручного встановлення компонентів:
  
    - [01-env-components.md](piloting-test/manual-installation/01-env-components.md) — вимоги до компонентів

    - [02-network-diagram.md](piloting-test/manual-installation/02-network-diagram.md) — мережева схема

    - [03-adminserver-install.md](piloting-test/manual-installation/03-adminserver-install.md) — встановлення AdminTools

    - [04-minio-install-and-settings.md](piloting-test/manual-installation/04-minio-install-and-settings.md) — встановлення та налаштування MinIO

    - [05-uxp-ss-install.md](piloting-test/manual-installation/05-uxp-ss-install.md) — встановлення ШБО

    - [06-uxp-ss-settings.md](piloting-test/manual-installation/06-uxp-ss-settings.md) — налаштування ШБО

    - [06.1-uxp-service-settings.md](piloting-test/manual-installation/06.1-uxp-service-settings.md) — налаштування сервісів ШБО

    - [07-elk-install-and-settings.md](piloting-test/manual-installation/07-elk-install-and-settings.md) — встановлення та налаштування Elasticsearch + Kibana

    - [08-graylog-install-and-settings.md](piloting-test/manual-installation/08-graylog-install-and-settings.md) — встановлення та налаштування Graylog

    - [09-zabbix-install-and-settings.md](piloting-test/manual-installation/09-zabbix-install-and-settings.md) — встановлення та налаштування Zabbix

    - [11-mon-hub-install-and-settings.md](piloting-test/manual-installation/11-mon-hub-install-and-settings.md) — встановлення та налаштування MonHub

  - [/script-installation](t2.0-client-instruction/piloting-test/script-installation) — інструкції зі встановлення компонентів через Ansible:
 
    - ... 

- [FORMS.md](FORMS.md) — перелік форм заявок, які потрібно заповнити в процесі налаштування компонентів

- [PILOTING-GUIDE.md](PILOTING-GUIDE.md) — повний регламент і правила пілотування

- [README.md](t2.0-client-instruction/README.md) — цей файл з описом репозиторію

---

## 4. Як розпочати пілотування

1. Ознайомтесь із [Регламентом пілотування](PILOTING-GUIDE.md)

2. Перевірте [вимоги до середовища](piloting-test/manual-installation/01-env-components.md)

3. Оберіть спосіб встановлення: [ручне](piloting-test/manual-installation) або [через Ansible](piloting-test/script-installation)

4. Почніть інсталяцію відповідно до обраного способу

5. Подайте заявки через відповідні [форми](FORMS.md)

6. Налаштуйте компоненти згідно з інструкціями

7. (Опціонально) Опублікуйте сервіси

8. Перевірте роботу ШБО

9. Почніть тестову інформаційну взаємодію

10. У разі потреби звертайтесь до Адміністратора системи

---

## 5. 📩 Комунікація

У разі виникнення питань:

- ✉️ Email: [trembita@diia.gov.ua](mailto:trembita@diia.gov.ua)
- 💬 Чат: персональний чат у WhatsApp з Адміністратором
