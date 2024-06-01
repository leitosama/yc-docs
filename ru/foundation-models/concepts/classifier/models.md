# Модели классификаторов на базе {{ yagpt-name }}

Обращение к модели классификатора на базе {{ yagpt-name }} по API возможно по ее [URI](https://ru.wikipedia.org/wiki/URI), содержащему [идентификатор каталога](../../../resource-manager/operations/folder/get-id.md). Опция `/latest` указывает версию вызываемой модели и является опциональной. Чтобы обратиться к модели классификатора на базе {{ yagpt-name }}, используйте метод [fewShotClassify](../../text-classification/api-ref/TextClassification/fewShotClassify.md) Text Classification API.

Если стандартной модели вам недостаточно, вы можете [дообучить](../../../datasphere/concepts/models/foundation-models.md#classifier-training) классификатор на базе {{ yagpt-name }} в сервисе [{{ ml-platform-full-name }}]({{ link-datasphere-main }}), чтобы модель точнее классифицировала на ваши запросы. Чтобы обратиться к дообученной в {{ ml-platform-full-name}} модели классификатора, используйте метод [classify](../../text-classification/api-ref/TextClassification/classify.md) Text Classification API.

| Модель | URI | Режим работы |
|---|---|---|
| Классификатор по промту | `cls://<идентификатор_каталога>/yandexgpt/latest` | Синхронный |
| Модель классификатора, дообученная в {{ ml-platform-full-name}} | `cls://<идентификатор_дообученной_модели>` | Синхронный |