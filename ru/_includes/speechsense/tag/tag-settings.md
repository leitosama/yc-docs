* Название, описание и цвет тега.

* **Критичный тег** — если опция включена, тег подсвечивается, когда вы открываете диалог. Теги с такой опцией помогают быстро определить, есть ли в записанном диалоге критичные и недопустимые слова, например ненормативная лексика.

* **Ключевые слова** — укажите каждую фразу-триггер на отдельной строке. {{ speechsense-name }} ищет ключевые слова в записи разговора и, если находит, назначает записи тег.

* **Точное совпадение** — включите опцию, чтобы искать в диалогах конкретные слова. Отключите, чтобы искать также родственные слова. Например, если опция выключена и в поле **Ключевые слова** указано слово «отказ», {{ speechsense-name }} будет искать диалоги со словами «отказ», «отказался», «отказавшийся» и т. д.

* **Дистанция между словами** — сколько дополнительных слов может быть между словами в ключевой фразе, чтобы разговору был назначен тег.

   {% cut "Пример" %}

   Тегу присвоена ключевая фраза «плохое обслуживание». Поиск этой фразы будет осуществляться по-разному в зависимости от значения поля **Дистанция между словами**:

   | Пример фразы в разговоре | Дистанция: `0` | Дистанция: `1` | Дистанция: `2` |
   | ----------- | ----------- | ----------- | ----------- |
   | «плохое обслуживание» | найдется | найдется | найдется |
   | «плохое же обслуживание» | не найдется | найдется | найдется |
   | «плохое у вас обслуживание» | не найдется | не найдется | найдется |

   {% endcut %}