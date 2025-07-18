# Інсталяція робочої станції адміністратора (AdminTools)

**Admin Tool** — це окремий компонент, який дозволяє адміністратору віддалено керувати компонентами UXP (сервери безпеки, клієнтами, сертифікатами тощо).

> ℹ️ **Підтримувані ОС:** Ubuntu Desktop 22.04 LTS та Windows 10
> 
> ℹ️ **Перед встановленням:** обов’язково налаштуйте **статичну IP-адресу** та переконайтесь у наявності **доступу до Інтернету**.

---

## 🐧 Ubuntu 22.04: встановлення Admin Tool

### 🔹 Підготовка

1. Закоментуйте всі активні репозиторії:
```bash
sudo sed -i 's/^[A-Za-z0-9]/#&/' /etc/apt/sources.list
```

2. Додайте GPG-ключ для репозиторію:
```bash
wget -O - https://project-repo.trembita.gov.ua:8081/public-keys/public.key.txt | sudo apt-key add -
```

3. Додайте репозиторій:
```bash
echo 'deb https://project-repo.trembita.gov.ua:8081/repository/t2-stack-1.22.7/ jammy main' | sudo tee -a /etc/apt/sources.list
```

4. Оновіть список пакетів:
```bash
sudo apt update
```

---

### 🔹 Встановлення

1. Встановіть Admin Tool:
```bash
sudo apt install uxp-integrity-admintool uxp-admintool-ubuntu
```

2. У вікні **Postfix Configuration** оберіть **No configuration**.

3. Після встановлення відкрийте **Admin Tool** через меню “Показати програми”.

![](03-adminserver-image/image1.png)

---

## 🖥️ Windows 10: встановлення Admin Tool

1. Завантажте інсталятор:  
👉 [Завантажити Admin Tool](https://project-repo.trembita.gov.ua:8081/files/t2/windows_10.zip)

2. Запустіть **ide_install.exe** з правами адміністратора.

3. Після встановлення використовуйте ярлик **Admin Tool** на Робочому столі.

  ![](03-adminserver-image/image2.png)
  
---

🔐 **Безпека:**  
Засіб адміністрування виконує автоматичні перевірки цілісності для забезпечення захищеного з’єднання з компонентами системи:

- кожні 10 хвилин у фоновому режимі;

- під час запуску Portable Firefox;

- за ініціативою користувача вручну.
