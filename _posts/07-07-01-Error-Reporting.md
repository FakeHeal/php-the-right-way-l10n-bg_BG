---
title:   Докладване на грешки
isChild: true
---

## Докладване на грешки

Записването на грешките в лог файл може да бъде полезе начин за търсене на проблеми в вашето приложение, но също може да
разкрие за външния свят информация за структурата на вашето приложение. За ефективна защита на приложението от проблеми
свързани с извеждането на тези съобщения, вие трябва да конфигурирате съвръра различно в зависимост от средата на която
се изпълнява приложението - т.е. развойна среда и реална среда.

### Развойна среда

За да показвате грешките във вашата <strong>развойна</strong> среда, задайте следните настройки в `php.ini`:

- display_errors: On
- error_reporting: E_ALL
- log_errors: On

### Реална среда

За да скриете грешките в <strong>реална</strong> среда, настройте `php.ini` както следва:

- display_errors: Off
- error_reporting: E_ALL
- log_errors: On

С тези настройки за реална среда, грешките ще бъдат запосвани в лог файла за грешки на сървъра и няма да бъдат
показвани на потребителите. За повече информация, посъветвайте се с документацията на PHP:

* [еrror_reporting](http://www.php.net/manual/en/errorfunc.configuration.php#ini.error-reporting)
* [display_errors](http://www.php.net/manual/en/errorfunc.configuration.php#ini.display-errors)
* [log_errors](http://www.php.net/manual/en/errorfunc.configuration.php#ini.log-errors)
