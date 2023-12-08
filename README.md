## Installation

- Clone this repository on your local computer
- configure .env as needed
- Run the `docker compose build && docker-compose up -d`.

```shell
git clone https://github.com/BDdennis/in-telligent-web-mock.git
cd docker-compose-lamp/
cp sample.env .env
// modify sample.env as needed
docker compose build && docker-compose up -d
// visit localhost
// P.S run docker build only if you update your orchestrator contract yml
```

Your LAMP stack is now ready!! You can access it via `http://localhost`.

## Configuration and Usage

### General Information

This Docker Stack is build for local development and not for production usage.

### Configuration

This package comes with default configuration options. You can modify them by creating `.env` file in your root directory.
To make it easy, just copy the content from `sample.env` file and update the environment variable values as per your need.

### Configuration Variables

There are following configuration variables available and you can customize them by overwritting in your own `.env` file.

---

#### PHP

---

_**PHPVERSION**_
Is used to specify which PHP Version you want to use. Defaults always to latest PHP Version.

_**PHP_INI**_
Define your custom `php.ini` modification to meet your requirments.

---

#### Apache

---

_**DOCUMENT_ROOT**_

It is a document root for Apache server. The default value for this is `./www`. All your sites will go here and will be synced automatically.

_**APACHE_DOCUMENT_ROOT**_

Apache config file value. The default value for this is /var/www/html.

_**VHOSTS_DIR**_

This is for virtual hosts. The default value for this is `./config/vhosts`. You can place your virtual hosts conf files here.

> Make sure you add an entry to your system's `hosts` file for each virtual host.

_**APACHE_LOG_DIR**_

This will be used to store Apache logs. The default value for this is `./logs/apache2`.