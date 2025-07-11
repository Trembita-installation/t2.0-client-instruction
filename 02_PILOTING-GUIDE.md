# Регламент пілотування системи Трембіта 2.0

##  🔹 1. Загальні положення

### 1.1 Мета та предмет регламенту

Цей документ визначає порядок проведення пілотування модернізованої системи електронної взаємодії державних електронних інформаційних ресурсів “Трембіта” (далі – система Трембіта 2.0).

### 1.2 Тлумачення термінів

У цьому Регламенті терміни і визначення вживаються у наступному значенні:

- **Відповідальна особа Учасника пілотування** — співробітник, який уповноважений Учасником пілотування системи Трембіта 2.0 здійснювати комунікацію з Адміністратором системи Трембіта з технічних питань, що виникають у процесі пілотування.
  
- **Режим multi-tenancy** — режим, який надає можливість розміщувати на шлюзі безпечного обміну (ШБО) однієї організації сервіси та підсистеми іншої, створювати кабінет відповідної організації з ролями: ```адміністратор ШБО```, ```адміністратор сервісів```, ```аудитор``` тощо.
  
- **Сертифікат автентифікації** — кваліфікований сертифікат електронної печатки, який використовується для автентифікації ШБО та встановлення з’єднань.

> ℹ️ **Примітка:** Отримання та застосування сертифіката Учасником пілотування системи Трембіта 2.0 має відбуватись відповідно до Порядку використання електронних довірчих послуг в органах державної влади, органах місцевого самоврядування, підприємствах, установах та організаціях державної форми власності, який затверджений постановою Кабінету Міністрів України від 01 серпня 2023 р. № 798, та Вимог до надавачів послуг електронної ідентифікації та електронних довірчих послуг, які затверджені постановою Кабінету Міністрів України від 28 червня 2024 р. № 764.

> Усі інші терміни використовуються згідно з Регламентом роботи системи “Трембіта”.

### 1.3 Цілі пілотування

- **Ключова ціль:**

    - Тестування системи Трембіта 2.0 в умовах, наближених до промислової експлуатації

- **Підцілі пілотування:**

    - Тестування міжнародних криптографічних алгоритмів (ECDSA)
      
    - Перевірка режиму *multi-tenancy*

### 1.4 Термін проведення пілотування

**Термін пілотування** — 90 днів з **07.07.2025** року.

### 1.5 Сторони пілотування:

| Сторона                       | Функція                                       |
|------------------------------|-----------------------------------------------|
| Адміністратор системи        | ДП "ДІЯ"                                      |
| Держатель системи            | Міністерство цифрової трансформації України   |
| Учасник пілотування          | Організація з інфраструктурою для тестування  |

Учасники пілотування виявили бажання прийняти участь у пілотуванні системи Трембіта 2.0 шляхом подання Держателю системи Трембіта форми підтвердження участі в пілотуванні.

### 1.6 Роль Держателя системи

```Держатель системи``` Трембіта здійснює організаційне забезпечення пілотування системи Трембіта 2.0.

### 1.7 Обов’язки Адміністратора системи

```Адміністратор системи``` Трембіта під час проведення пілотування системи Трембіта 2.0 здійснює:

-	розгортання ядра тестового середовища системи Трембіта 2.0;
  
-	координацію процесу пілотування та взаємодії Учасників пілотування системи Трембіта 2.0;
  
-	технічну підтримку Учасників пілотування системи Трембіта 2.0;
  
-	проведення моніторингу та збір аналітичних даних щодо процесу і результатів пілотування;
  
-	підготовку звіту про результати пілотування і рекомендацій Держателю системи Трембіта щодо подальшого введення в промислову експлуатацію системи Трембіта 2.0.

### 1.8 Обов’язки Учасників пілотування

```Учасник пілотування``` системи Трембіта 2.0 під час проведення пілотування системи Трембіта 2.0 забезпечує:

- дотримання вимог цього Регламенту, інструкцій, наданих Адміністратором системи Трембіта на період пілотування;
  
- налаштування локального компоненту системи Трембіта 2.0 та локального компоненту підсистеми моніторингу доступу до персональних даних (далі – ПМДПД);
  
- побудову електронної інформаційної взаємодії з іншими Учасниками пілотування системи Трембіта 2.0;
  
- подання Адміністратору  системи Трембіта визначених цим Регламентом технічних заявок, а також повідомлень про виявлені помилки у роботі локальних компонентів системи Трембіта 2.0 у процесі пілотування.

---

## 🔹 2. Підготовка до пілотування

### 2.1 Вимоги до підготовки інфраструктури

У процесі підготовки до пілотування Учасник пілотування системи Трембіта 2.0 повинен:

- **2.1.1**: Підготовка обладнання  [Склад, призначення та мінімальні апаратні характеристики компонентів тестового середовища](piloting-test/manual-installation/01-env-components.md#hardware-characteristics)
  
- **2.1.2**: Виділення IP-адреси (у випадку обмеженої кількості доступних вільних IP-адрес в Учасника пілотування системи Трембіта 2.0, для ШБО можна налаштувати довільний TCP-порт вхідних з’єднань для зовнішньої статичної IP-адреси, що дозволяє використовувати одну зовнішню адресу для декількох ШБО (NAT))
  
- **2.1.3**: Налаштування мережі [Мережева схема підключення до тестового середовища СЕВДЕІР під час пілотування](piloting-test/manual-installation/02-network-diagram.md#network-diagram)
  
- **2.1.4**: Отримання сертифікатів за міжнародним криптоалгоритмом ECDSA (автентифікації та підпису)
  
- **2.1.5**: Наявність віртуалізації з підтримкою USB-токенів (VMware vSphere Hypervisor, Proxmox)

- 

### 2.2 Вимоги до сертифікатів

- у атрибутах сертифіката має бути зазначено код ```ЄДРПОУ``` Учасника пілотування системи Трембіта 2.0;
  
-	сертифікат автентифікації повинен містити позначку ```«Key Agreement»``` («узгодження ключів») в полі ```KeyUsage```;
  
-	сертифікат підпису повинен містити позначку ```«Non Repudiation»``` («невідмовність») в полі ```KeyUsage```.

---

## 🔹 3. Організаційні питання

### 3.1 Встановлення компонентів

Пілотування системи Трембіта 2.0 здійснюється у тестовому середовищі системи  Трембіта 2.0 та починається з встановлення та налаштування локального компоненту системи Трембіта 2.0.

### 3.2 Обов’язкові компоненти для пілотування

Для проходження пілотування системи Трембіта 2.0 Учасник зобов’язаний встановити всі компоненти системи Трембіта 2.0, які були зазначені у формі згоди на участь у пілотуванні. Обрані у цій формі компоненти є обов’язковими до встановлення. За бажанням також може бути встановлений локальний компонент підсистеми моніторингу доступу до персональних даних (ПМДПД).

### 3.3 Інструкції до локальних компонентів

Для роботи з Локальним компонентом Системи Трембіта 2.0 скористайтесь відповідними інструкціями:

- [Склад, призначення та мінімальні апаратні характеристики компонентів тестового середовища](piloting-test/manual-installation/01-env-components.md)

- [Мережева схема підключення до тестового середовища СЕВДЕІР під час пілотування](piloting-test/manual-installation/02-network-diagram.md)

- [Встановлення робочої станції адміністратора (AdminTools)](piloting-test/manual-installation/03-adminserver-install.md)

- [Інсталяція та налаштування сховища S3 (MinIO) для архівування журналів повідомлень](piloting-test/manual-installation/04-minio-install-and-settings.md)

- [Встановлення пакета uxp-securityserver-ua та залежностей (ШБО)](piloting-test/manual-installation/05-uxp-ss-install.md)

- [Початкове налаштування ШБО](piloting-test/manual-installation/06-uxp-ss-settings.md)

- [Керування сервісами на ШБО](piloting-test/manual-installation/06.1-uxp-service-settings.md)

- [Опціональне налаштування ШБО](piloting-test/manual-installation/06.2-uxp-ss-user-guide.md)

- [Інсталяція серверу аналізу транзакцій ELK](piloting-test/manual-installation/07-elk-install-and-settings.md)

- [Інсталяція та налаштування серверу аналізу журналів подій до ШБО (Graylog)](piloting-test/manual-installation/08-graylog-install-and-settings.md)

- [Інсталяція та налаштування серверу моніторингу за працездатністю Zabbix](piloting-test/manual-installation/09-zabbix-install-and-settings.md)

- [Інсталяція та налаштування MonHub](piloting-test/manual-installation/10-mon-hub-install-and-settings.md)

- [Інсталяція компонентів через Ansible-playbook](piloting-test/scripted-installation-ansible/01-ansible.md)

Для роботи з Локальним компонентом ПМДПД скористайтесь відповідними інструкціями:

- [Інструкція з інсталяції Локального компоненту ПМДПД](https://portal.trembita.gov.ua/media/website-media/LK_PMDPD.pdf)

- [Інструкція адміністратора Локального компоненту ПМДПД](https://portal.trembita.gov.ua/media/website-media/%D0%86%D0%BD%D1%81%D1%82%D1%80%D1%83%D0%BA%D1%86%D1%96%D1%8F_%D0%90%D0%B4%D0%BC%D1%96%D0%BD%D1%96%D1%81%D1%82%D1%80%D0%B0%D1%82%D0%BE%D1%80%D0%B0_%D0%9B%D0%BE%D0%BA%D0%B0%D0%BB%D1%8C%D0%BD%D0%BE%D0%B3%D0%BE_%D0%BA%D0%BE%D0%BC%D0%BF%D0%BE%D0%BD%D0%B5%D0%BD%D1%82%D1%83_%D0%9F%D0%9C%D0%94%D0%9F%D0%94.pdf)

- [Інструкція відповідального за захист персональних даних](https://portal.trembita.gov.ua/media/website-media/%D0%86%D0%BD%D1%81%D1%82%D1%80%D1%83%D0%BA%D1%86%D1%96%D1%8F_%D0%92%D1%96%D0%B4%D0%BF%D0%BE%D0%B2%D1%96%D0%B4%D0%B0%D0%BB%D1%8C%D0%BD%D0%BE%D0%B3%D0%BE_%D0%B7%D0%B0_%D0%B7%D0%B0%D1%85%D0%B8%D1%81%D1%82_%D0%BF%D0%B5%D1%80%D1%81%D0%BE%D0%BD%D0%B0%D0%BB%D1%8C%D0%BD%D0%B8%D1%85_%D0%B4%D0%B0%D0%BD%D0%B8%D1%85.pdf)

У разі відсутності можливості тестування взаємодії з іншими учасниками пілоту, можна скористатися прикладом реалізації тестових вебсервісів і вебклієнтів:

- [Матеріали для розробників тестових вебсервісів та вебклієнтів (на різних мовах програмування)](https://github.com/Trembita-installation/dev-services-and-clients)

Це дозволить перевірити працездатність налаштувань на власному середовищі.

### 3.4 Участь у тестуванні ПМДПД

За згодою учасник пілотування системи Трембіта 2.0 може розгорнути локальний компонент  ПМДПД з метою його тестування.

Кожен локальний компонент ПМДПД Учасника пілотування системи Трембіта 2.0 повинен обмінюватись даними із центральним компонентом ПМДПД через окрему технологічну підсистему. 

Локальний компонент ПМДПД повинен однозначно ідентифікуватись в центральному компоненті ```Іменем клієнту``` та ```API ключем```. Ім’я клієнту формується Адміністратором системи Трембіта під час реєстрації локального компоненту ПМДПД, як клієнта центрального компоненту ПМДПД.

### 3.5 Покроковий план реєстрації

Покрокові дії:

1. Форма [Запит на отримання Конфігураційних Даних](https://forms.gle/2CHfj58AjEY6L4aZ8)

2. Форма [Реєстрація ШБО Трембіти 2.0](https://forms.gle/fsD3osa9rzXihcLU9) 

3. Форма [Реєстрація підсистеми на ШБО](https://forms.gle/wwUE9cZ4PDqEnGnm6)

4. Публікація сервісу (опціонально)

5. Форма [Реєстрація локального компоненту ПМДПД](https://forms.gle/nkLd1HszPkVewfem7) (опціонально)

6. Форма [Реєстрація підсистеми на ШБО](https://forms.gle/wwUE9cZ4PDqEnGnm6) (опціонально - технологічна підсистема для локального компоненту ПМДПД) 

7. Форма [Реєстрація сервісної декларації](https://forms.gle/cxsfYUiDmwFcqspq7) (опціонально)

8. Побудова інформаційної взаємодії


<span id="form-fields"></span>
### 3.6 Перелік полів у формах

Для кожного з етапів пілотування передбачено окрему Google-форму. У кожній з форм потрібно заповнити обовʼязкові поля, що містять технічну та ідентифікаційну інформацію про Учасника пілотування та його компоненти.

1. Форма Запит на отримання Конфігураційних Даних:

- Назва організації (з випадаючого списку)

- E-mail відповідальної особи

> ℹ️ **Примітка:** Конфігураційні дані (ліцензія та файл конфігурації) будуть надіслані на E-mail відповідальної особи, вказаний у Формі, для подальшого налаштування системи.

2. Форма Реєстрація ШБО Трембіти 2.0:

- Назва організації (з випадаючого списку)

- Код ШБО

- Публічна IP-адреса або доменне ім'я ШБО

- Файл сертифікату автентифікації (додається файл сертифікату автентифікації)

3. Форма Реєстрація підсистеми на ШБО:

- Назва організації (з випадаючого списку)

- Тип підсистеми (вибрати із списку ```Звичайна```, ```Технологічна для локального компоненту МДПД у фораті "PDAMS_LC_number"``` або ```Для локального компоненту МДПД з префіксом "PD_Descr"```)

- Код ШБО

- Код підсистеми на ШБО

4. Форма Реєстрація локального компоненту ПМДПД:

- Назва організації (з випадаючого списку)

- Код ЄДРПОУ

- Код технологічної підсистеми на ШБО

- E-mail відповідальної особи для надсилання ```Ключ АРІ``` та ```Ім’я клієнта в Центральному компоненті ПМДПД```

> ℹ️ **Примітка:** За результатами обробки заявки Адміністратором системи, Учаснику буде надано ```Ключ АРІ``` та ```Ім’я клієнта в Центральному компоненті ПМДПД```, які буде надіслано на E-mail відповідальної особи, вказану у Формі

5. Форма Реєстрація сервісної декларації:

- Назва організації (з випадаючого списку)

- Сервіс працює у (вибрати ```Відокремленому режимі``` або ```Режимі проксі```)

- Код ЄДРПОУ

- Ідентифікатор сервісу

- Назва сервісу

- Опис сервісу

- Технічний опис сервісу

---

> ⚠️ **Увага:** У разі виявлення помилок або невідповідностей у поданій заявці, Адміністратор системи Трембіта залишає за собою право відхилити заявку, про що буде надіслано відповідне повідомлення на E-mail відповідальної особи.

---

### 3.7 Укладання договорів на період пілотування

Налаштування електронної інформаційної взаємодії між Учасниками пілотування системи Трембіта 2.0 здійснюється за потреби за участю Адміністратора системи Трембіта.

Для побудови електронної інформаційної взаємодії при пілотуванні системи Трембіта 2.0   договір про інформаційну взаємодію не укладається.

### 3.8 Імітація Каталогу

Під час проведення пілотування системи Трембіта 2.0, у зв’язку з відсутністю публічного Каталогу, його імітацію представлено у [файлі-симуляції Каталогу](04_CATALOG_SIMULATION.md).

### 3.9 Канали комунікації

Комунікація з Адміністратором системи Трембіта здійснюється через:

– загальний WhatsApp-чат — для організаційних питань

- індивідуальні WhatsApp-чати — для технічної підтримки

- E-mail trembita@diia.gov.ua — за потреби


---

## 🔹 4. Вимоги до назв об'єктів під час пілотування

### 4.1 Формат ідентифікаторів ШБО 

```MemberCode_SS_Т2_Number_FreeSymbols```, де:

```MemberCode``` – код ЄДРПОУ Учасника пілотування системи Трембіта 2.0

```Т2``` – код середовища системи

```Number``` – порядковий номер ШБО Учасника пілотування системи Трембіта 2.0 (у межах тестового середовища починається з ```01```)

```FreeSymbols``` – (за потреби) літери латиницею верхнім регістром та цифри, які Учасники пілотування системи Трембіта 2.0 можуть додавати до ідентифікатора ШБО задля власної зручності.

Ідентифікатори ШБО формує відповідальна особа Учасника пілотування системи Трембіта 2.0, що інсталює відповідний ШБО.

### 4.2 Формат коду підсистеми

```PD_Descr_FreeSymbols```, де:

```PD``` – префікс, що додається до коду підсистеми виключно у випадку, якщо на підсистемі планується публікувати вебсервіси, що містять персональні дані, а обмін даними буде здійснюватися із застосуванням локального компоненту ПМДПД

```Descr``` – короткий опис електронного інформаційного ресурсу англійськими словами, побудований за принципом UpperCamelCase. Опис має складатися не більше, ніж з п'яти слів англійською мовою

```FreeSymbols``` – (за потреби) літери латиницею та цифри, які можна додавати до ідентифікатора підсистеми задля зручності.

Ідентифікатори підсистеми формує відповідальна особа Учасника пілотування системи Трембіта 2.0.


### 4.3 Формат ідентифікатора ПМДПД

```PDAMS_LC_number```, де:

```PDAMS_LC``` – незмінним набір символів та ідентифікує, що підсистема є технологічною підсистемою локального компоненту ПМДПД 

```number``` - порядковий номер локального компоненту ПМДПД певного Учасника пілотування системи Трембіта 2.0 (починається з ```01```)).

Код технологічної підсистеми формується відповідальна особа Учасника пілотування системи Трембіта 2.0.

---

## 🔹 5. Моніторинг пілотування системи Трембіта 2.0 (MonHub)

### 5.1 Повідомлення про помилки

Кожна виявлена у процесі пілотування помилка має бути описана Учасником пілотування системи Трембіта 2.0 у повідомленні про помилки в довільній формі і направлена Адміністратору системи Трембіта в індивідуальні WhatsApp-чати або e-mail для подальшого аналізу.

### 5.2 Моніторинг серверів під час пілоту

Протягом усього процесу пілотування за згодою Учасників пілотування системи Трембіта 2.0 Адміністратор системи Трембіта проводить моніторинг усіх серверів системи Трембіта 2.0 з метою якісного тестування та детального аналізу роботи системи Трембіта 2.0 в умовах навантаження. 

Зібрана інформація передається Адміністратору системи Трембіта для подальшого аналізу та буде включена до звіту про результати пілотування системи Трембіта 2.0.

### 5.3 Обсяг даних, що передаються до MonHub

MonHub охоплює ресурси сервера (RAM, CPU, DISK тощо), а також передбачає передачу всіх логів компонентів системи Трембіта 2.0 клієнта (програмне забезпечення UXP) та системних журналів (syslog), за винятком логів, що стосуються SSH, login/logout, mail-сесій.

Якщо в системі використовується інше програмне забезпечення, логування якого потрібно виключити з центрального моніторингу, це передбачається в конфігурації за погодженням – Учасник пілотування системи Трембіта 2.0 має завчасно повідомити про це Адміністратора системи Трембіта.

---
