# Встановлення робочої станції адміністратора (AdminTools)
Засіб адміністрування UXP (Administration Tool) використовується для віддаленого керування компонентами UXP.

> ℹ️ **Примітка.** Засіб адміністрування підтримує операційні системи Ubuntu Desktop 22.04 LTS та Windows 10.

---
## 🐧 Ubuntu: Робоча станція адміністратора

### 🔹 Підготовка операційної системи

Операційна система: **Ubuntu Desktop 22.04 LTS**

### 🔹 Крок 1: Налаштування репозиторію
1. Закоментуйте всі активні репозиторії:

    ```bash
    sudo sed -i 's/^[A-Za-z0-9]/#&/' /etc/apt/sources.list
    ```

2. Додайте GPG-ключ:

    ```bash
    wget -O - https://project-repo.trembita.gov.ua:8081/public-keys/public.key.txt | sudo apt-key add -
    ```

3. Додайте репозиторій:

    ```bash
    echo 'https://project-repo.trembita.gov.ua:8081/repository/tr-2-pre-final/ jammy main' | sudo tee -a /etc/apt/sources.list
    ```

4. Оновіть список доступних пакетів:

    ```bash
    sudo apt update
    ```
### 🔹 Крок 2: Встановлення

1. Встановіть Засіб адміністрування:

    ```bash
    sudo apt install uxp-integrity-admintool uxp-admintool-ubuntu
    ```

2. У діалоговому вікні **Postfix Configuration** оберіть опцію **No configuration** (або натисніть Enter).

3. Після встановлення, в меню програм з’явиться нова програма **Admin Tool** з іконкою системи "Трембіта".

    ![](03-adminserver-image/image1.png)

---
## 🖥️ Windows: Робоча станція адміністратора

### 🔹 Підготовка операційної системи

Операційна система: **Windows 10**

### 🔹 Кроки встановлення

1. Завантажте дистрибутив за посиланням:

    > [**Завантажити інсталятор**](https://portal.trembita.gov.ua/...)  
    *(Посилання необхідно додати)*

2. Запустіть **ide_install.exe** з правами адміністратора.

3. Після встановлення відкрийте програму через ярлик **Admin Tool** на Робочому столі.

    ![](03-adminserver-image/image2.png)
