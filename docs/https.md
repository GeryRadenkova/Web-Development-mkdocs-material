## HTTP (HyperText Transfer Protocol Secure)

**HTTPS** е сигурната версия на HTTP – протокола, който се използва за комуникация между уеб браузър и уеб сървър. Основната разлика между HTTP и HTTPS е, че HTTPS използва шифроване, за да защити данните, които се обменят между клиента и сървъра.

---

## Разлика между HTTP и HTTPS

| Характеристика        | HTTP                                           | HTTPS                                      |
|-----------------------|------------------------------------------------|-------------------------------------------|
| **Сигурност**         | Данните се изпращат в чист текст (незащитени). | Данните са **шифровани** и защитени.     |
| **Използва шифроване** | Не                                            | Да (SSL/TLS)                          |
| **Цел**             | Подходящ за несигурни уебсайтове (без чувствителни данни). | Задължителен за сигурни сайтове (банки, онлайн магазини, социални мрежи). |
| **Порт**            | 80                                            | 443                                       |
| **Скорост**         | По-бърз (без допълнително криптиране).        | По-бавен, но по-сигурен.                 |
| **SEO и доверие**   | Google не го препоръчва.                     | Google предпочита HTTPS сайтове (по-добър ранкинг). |

---

## Как работи HTTPS?

HTTPS използва SSL/TLS (Secure Sockets Layer / Transport Layer Security) за шифроване на връзката между клиента и сървъра.

Процес на работа на HTTPS:

1. **Клиентът (браузърът) изпраща заявка** към HTTPS уебсайта.
2. **Сървърът отговаря с SSL/TLS сертификат** – той съдържа публичен ключ за криптиране.
3. **Клиентът проверява валидността на сертификата.**
4. **Ако сертификатът е валиден**, клиентът и сървърът установяват шифрована връзка.
5. **Всички данни, изпратени между клиента и сървъра, са криптирани.**

---

## Какво е SSL/TLS сертификат?

**SSL (Secure Sockets Layer) и TLS (Transport Layer Security)**

* **SSL** и **TLS** са криптографски протоколи, които осигуряват сигурна комуникация.
* **TLS** е по-новата и по-сигурна версия на SSL.
* Когато чуете "SSL сертификат", често се има предвид **TLS**.

**Пример:** Ако въвеждате парола в сайт с HTTPS, тя се шифрова и не може да бъде прихваната от хакери.

**Видове SSL/TLS сертификати**

| Вид сертификат                   | Ниво на защита           | Използване                        |
|------------------------------    |------------------------- |-----------------------------------|
| **DV (Domain Validation)**       | Основно шифроване        | Блогове, лични сайтове            |
| **OV (Organization Validation)** | Верифицирана организация | Бизнес сайтове                    |
| **EV (Extended Validation)**     | Най-висока защита        | Банки, правителствени сайтове     |

---

## Защо HTTPS е важен?

1. Защита на данните (Encryption)

    * Всички данни между браузъра и сървъра са шифровани.
    * Дори ако някой прихване трафика, няма да може да разчете съдържанието.

2. Защита от MITM атаки (Man-In-The-Middle)

    * Без HTTPS, хакери могат да прихванат и манипулират заявките между клиент и сървър.
    * HTTPS предотвратява тези атаки чрез шифроване.

3. По-добър SEO ранкинг

    * Google дава предимство на HTTPS сайтове.
    * Несигурни HTTP сайтове се маркират като "Not Secure" в браузърите.

4. Доверие на потребителите

    * Потребителите избягват сайтове без HTTPS, особено ако въвеждат чувствителни данни.

---

## Как да направим уебсайт HTTPS?

1. Получаване на SSL/TLS сертификат

    * Можете да получите безплатен сертификат от [Let's Encrypt](https://letsencrypt.org/).
    * Платените сертификати предлагат повече защити (напр. от DigiCert, GlobalSign).

2. Инсталиране на сертификата на уеб сървъра

    * Ако използвате Apache:
    ```shell
    sudo a2enmod ssl
    sudo service apache2 restart
    ```

    * Ако използвате Nginx:
    ```shell
    sudo nano /etc/nginx/sites-available/default
    ```

3. Пренасочване на HTTP към HTTPS

    * В `.htaccess` файл за Apache:
    ```perl
    RewriteEngine On
    RewriteCond %{HTTPS} !=on
    RewriteRule ^ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]
    ```

    * В `nginx.conf`:
    ```perl
    server {
        listen 80;
        server_name example.com;
        return 301 https://$host$request_uri;
    }
    ```
