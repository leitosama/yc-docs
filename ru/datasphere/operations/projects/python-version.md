---
title: "Как изменить версию Python для проекта в {{ ml-platform-full-name }}"
description: "Следуя данной инструкции, вы сможете изменить версию Python для проекта." 
---

# Выбрать версию Python

{% include [python-disclaimer](../../../_includes/datasphere/python-disclaimer.md) %}

По умолчанию в проектах {{ ml-platform-short-name }} используется Python 3.10. Чтобы изменить версию для проекта:

1. {% include [include](../../../_includes/datasphere/ui-find-project.md) %}
1. В разделе **{{ ui-key.yc-ui-datasphere.project-page.project-resources }}** выберите ![docker](../../../_assets/console-icons/logo-docker.svg) **{{ ui-key.yc-ui-datasphere.resources.docker }}**.
1. Выберите шаблон [Docker-образа](../../concepts/docker.md) с нужной версией Python.
1. Нажмите **{{ ui-key.yc-ui-datasphere.common.activate }}**.

{% note warning %}

Системный образ DS Default (Python 3.7) не работает с [конфигурациями](../../concepts/configurations.md) g2.x (GPU A100).

{% endnote %}