---
title: "Ключи капчи в {{ captcha-full-name }}"
description: "Из статьи вы узнаете, что такое клиентский и серверный ключи капчи и для чего они используются."
---

# Ключи капчи

При создании капчи автоматически генерируется пара ключей: клиентский и серверный. Эти ключи генерируются один раз при создании капчи и никогда не меняются.

Клиентский ключ (`client_key`) используется для [добавления виджета](./widget-methods.md) {{ captcha-name }} на сайт. Он является публичным и указывается на той странице, где размещен виджет.

Серверный ключ (`server_key`) используется бэкендом сайта, чтобы получить [результат проверки](./validation.md#validation-result) запроса пользователя. Серверный ключ — приватный, поэтому храните его в надежном месте. Например, вы можете использовать для этого сервис [{{ lockbox-full-name }}](../../lockbox/).

{% note warning %}

Никогда не пересылайте серверный ключ {{ captcha-name }}, не храните его в незашифрованном виде и не допускайте, чтобы он попал в открытый доступ. Если ключ все-таки оказался в открытом доступе, [создайте новую капчу](../operations/create-captcha.md) и замените ей старую.

{% endnote %}

Подробнее о том, как получить клиентский и серверный ключи, см. в разделе [{#T}](../operations/get-keys.md).

## Формат ключей {#key-format}

Клиентский ключ всегда имеет префикс `ysc1_`, серверный — `ysc2_`. Следующие 20 символов у них совпадают.

Примеры ключей:

* `client_key` : `ysc1_yzF85********`
* `server_key` : `ysc2_yzF85********`