# ChatGPT Telegram Bot Адаптированный под простого пользователя
![python-version](https://img.shields.io/badge/python-3.9-blue.svg)
[![openai-version](https://img.shields.io/badge/openai-1.3.3-orange.svg)](https://openai.com/)


Это [Telegram bot](https://core.telegram.org/bots/api), который интегрируется с официальными API [ChatGPT](https://openai.com/blog/chatgpt/), [DALL·E](https://openai.com/product/dall-e-2) и [Whisper](https://openai.com/research/whisper) от OpenAI для предоставления ответов. Готов к использованию с минимальной необходимой конфигурацией.
Для создания которого использовался репозиторий: [n3d1117](https://github.com/n3d1117/chatgpt-telegram-bot.git)

## Особенности
- [x] Поддержка markdown в ответах
- [x] Сброс разговора с помощью команды `/reset`
- [x] Индикатор набора текста во время генерации ответа
- [x] Доступ может быть ограничен путем указания списка разрешенных пользователей
- [x] Поддержка Docker и прокси
- [x] Генерация изображений с использованием DALL·E с помощью команды `/image`
- [x] Транскрибация аудио- и видеосообщений с использованием Whisper (может потребоваться [ffmpeg](https://ffmpeg.org))
- [x] Автоматическое создание сводки разговора для избежания избыточного использования токенов
- [x] Отслеживание использования токенов для каждого пользователя - от [@AlexHTW](https://github.com/AlexHTW)
- [x] Получение статистики использования личных токенов с помощью команды `/stats` - от [@AlexHTW](https://github.com/AlexHTW)
- [x] Бюджеты пользователей и гостей - от [@AlexHTW](https://github.com/AlexHTW)
- [x] Поддержка потоковой передачи данных
- [x] Поддержка GPT-4о
- [x] Локализация языка бота
  - Доступные языки :brazil: :cn: :finland: :de: :indonesia: :iran: :it: :malaysia: :netherlands: :poland: :ru: :saudi_arabia: :es: :taiwan: :tr: :ukraine: :gb: :uzbekistan: :vietnam:
- [x] Улучшенная поддержка встроенных запросов для групповых и личных чатов - от [@bugfloyd](https://github.com/bugfloyd)
  - Для использования этой функции, включите встроенные запросы для вашего бота в BotFather с помощью команды `/setinline` [command](https://core.telegram.org/bots/inline)
- [x] Поддержка *новых моделей* [анонсированных 13 июня 2023](https://openai.com/blog/function-calling-and-other-api-updates)
- [x] Поддержка *функций* (плагинов) для расширения функциональности бота с помощью услуг сторонних поставщиков
  - Погода, Spotify, Веб-поиск, текст в речь и другие. См. [здесь](#available-plugins) список доступных плагинов
- [x] Поддержка неофициальных API, совместимых с OpenAI - от [@kristaller486](https://github.com/kristaller486)
- [x] (НОВО!) Поддержка GPT-4 Turbo и DALL·E 3 [анонсированная 6 ноября 2023](https://openai.com/blog/new-models-and-developer-products-announced-at-devday) - от [@AlexHTW](https://github.com/AlexHTW)
- [x] (НОВО!) Поддержка текст в речь [анонсированная 6 ноября 2023](https://platform.openai.com/docs/guides/text-to-speech) - от [@gilcu3](https://github.com/gilcu3)
- [x] (НОВО!) Поддержка визуальных данных [анонсированная 6 ноября 2023](https://platform.openai.com/docs/guides/vision) - от [@gilcu3](https://github.com/gilcu3)

## Дополнительные функции - нужна помощь!
Если вы хотите помочь, загляните в раздел [issues](https://github.com/n3d1117/chatgpt-telegram-bot/issues) и внесите свой вклад!  
Если вы хотите помочь с переводами, загляните в [Руководство по переводу](https://github.com/n3d1117/chatgpt-telegram-bot/discussions/219)

PR всегда приветствуются!

## Предварительные требования
- Python 3.9+
- [Telegram бот](https://core.telegram.org/bots#6-botfather) и его токен (см. [руководство](https://core.telegram.org/bots/tutorial#obtain-your-bot-token))
- Учетная запись [OpenAI](https://openai.com) (см. раздел [настройка](#configuration))

## Начало работы

### Настройка
Настройте конфигурацию, скопировав файл `.env.example` и переименовав его в `.env`, а затем отредактируйте необходимые параметры по желанию:

| Параметр                   | Описание                                                                                                                                                                                                                   |
|-----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `OPENAI_API_KEY`            | Ваш ключ API OpenAI, вы можете получить его [здесь](https://platform.openai.com/account/api-keys)                                                                                                                                 |
| `TELEGRAM_BOT_TOKEN`        | Токен вашего Telegram бота, полученный с помощью [BotFather](http://t.me/botfather) (см. [руководство](https://core.telegram.org/bots/tutorial#obtain-your-bot-token))                                                                  |
| `ADMIN_USER_IDS`            | ID пользователей Telegram-администраторов. Эти пользователи имеют доступ к специальным административным командам, информации и не имеют ограничений бюджета. ID администраторов не обязательно добавлять в `ALLOWED_TELEGRAM_USER_IDS`. **Примечание**: по умолчанию нет администраторов (`-`) |
| `ALLOWED_TELEGRAM_USER_IDS` | Список ID пользователей Telegram, которым разрешено взаимодействовать с ботом (используйте [getidsbot](https://t.me/getidsbot) для поиска своего ID пользователя). **Примечание**: по умолчанию разрешено *всем* (`*`)                       |

### Дополнительная настройка
Следующие параметры являются необязательными и могут быть установлены в файле `.env`:

#### Бюджеты
| Параметр             | Описание                                                                                                                                                                                                                                                                                                                                                                               | Значение по умолчанию      |
|-----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------|
| `BUDGET_PERIOD`       | Определяет временной интервал применения всех бюджетов. Доступные периоды: `daily` *(сбрасывает бюджет ежедневно)*, `monthly` *(сбрасывает бюджеты в первый день каждого месяца)*, `all-time` *(не сбрасывает бюджет никогда)*. См. [Руководство по бюджету](https://github.com/n3d1117/chatgpt-telegram-bot/discussions/184) для получения дополнительной информации | `monthly`                   |
| `USER_BUDGETS`        | Список сумм в долларах для каждого пользователя из списка `ALLOWED_TELEGRAM_USER_IDS`, чтобы установить индивидуальное ограничение использования токенов API OpenAI для каждого. Для пользователей из списка `*` первому пользователю из списка `USER_BUDGETS` назначается значение. **Примечание**: по умолчанию *нет ограничений* для любого пользователя (`*`). См. [Руководство по бюджету](https://github.com/n3d1117/chatgpt-telegram-bot/discussions/184) для получения дополнительной информации | `*`                         |
| `GUEST_BUDGET`        | Сумма в долларах в качестве ограничения использования для всех гостевых пользователей. Гостевые пользователи - пользователи в групповых чатах, которые не находятся в списке `ALLOWED_TELEGRAM_USER_IDS`. Значение игнорируется, если в бюджетах пользователей не установлены ограничения использования (`USER_BUDGETS`=`*`). См. [Руководство по бюджету](https://github.com/n3d1117/chatgpt-telegram-bot/discussions/184) для получения дополнительной информации                                                   | `100.0`                     |
| `TOKEN_PRICE`         | Стоимость в долларах за 1000 использованных токенов для вычисления информации о стоимости в статистике использования. Источник: https://openai.com/pricing                                                                                                                                                                                                                                                                          | `0.002`                     |
| `IMAGE_PRICES`        | Список, разделенный запятыми, из 3 элементов цен для различных размеров изображений: `256x256`, `512x512` и `1024x1024`. Источник: https://openai.com/pricing                                                                                                                                                                                                                                  | `0.016,0.018,0.02`          |
| `TRANSCRIPTION_PRICE` | Стоимость в долларах за одну минуту аудио-транскрибации. Источник: https://openai.com/pricing                                                                                                                                                                                                                                                                                                       | `0.006`                     |
| `VISION_TOKEN_PRICE`  | Стоимость в долларах за 1000 токенов интерпретации изображения. Источник: https://openai.com/pricing                                                                                                                                                                                                                                                                                                       | `0.01`                      |
| `TTS_PRICES`          | Список, разделенный запятыми, из цен на модели tts: `tts-1`, `tts-1-hd`. Источник: https://openai.com/pricing                                                                                                                                                                                                                                                                            | `0.015,0.030`               |

Проверьте [Руководство по бюджету](https://github.com/n3d1117/chatgpt-telegram-bot/discussions/184) для возможных конфигураций бюджета.

#### Дополнительные необязательные параметры конфигурации
| Параметр                 | Описание                                                                                                                                                                                                                                                                      | Значение по умолчанию        |
|--------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------|
| `ENABLE_QUOTING`         | Включить ли цитирование сообщений в личных чатах                                                                                                                                                                                                                             | `true`                        |
| `ENABLE_IMAGE_GENERATION`| Включить ли генерацию изображений с помощью команды `/image`                                                                                                                                                                                                                | `true`                        |
| `ENABLE_TRANSCRIPTION`   | Включить ли транскрибацию аудио- и видеосообщений                                                                                                                                                                                                                           | `true`                        |
| `ENABLE_TTS_GENERATION`  | Включить ли генерацию текста в речь с помощью `/tts`                                                                                                                                                                                                                        | `true`                        |
| `ENABLE_VISION`          | Включить ли возможности зрения в поддерживаемых моделях                                                                                                                                                                                                                      | `true`                        |
| `PROXY`                  | Прокси, используемый для OpenAI и Telegram бота (например, `http://localhost:8080`)                                                                                                                                                                                          | -                             |
| `OPENAI_PROXY`           | Прокси, используемый только для OpenAI (например, `http://localhost:8080`)                                                                                                                                                                                                    | -                             |
| `TELEGRAM_PROXY`         | Прокси, используемый только для Telegram бота (например, `http://localhost:8080`)                                                                                                                                                                                             | -                             |
| `OPENAI_MODEL`           | Модель OpenAI для генерации ответов. Вы можете найти все доступные модели [здесь](https://platform.openai.com/docs/models/)                                                                                                                                                | `gpt-3.5-turbo`               |
| `OPENAI_BASE_URL`        | URL конечной точки для неофициальных API, совместимых с OpenAI (например, LocalAI или text-generation-webui)                                                                                                                                                                 | URL API OpenAI по умолчанию    |
| `ASSISTANT_PROMPT`       | Системное сообщение, которое устанавливает тон и контролирует поведение помощника                                                                                                                                                                                             | `Вы - полезный помощник.`     |
| `SHOW_USAGE`             | Показывать ли информацию об использовании токенов OpenAI после каждого ответа                                                                                                                                                                                                 | `false`                       |
| `STREAM`                 | Потоковая передача ответов. **Примечание**: несовместима с `N_CHOICES`, установленным больше 1                                                                                                                                                                               | `true`                        |
| `MAX_TOKENS`             | Верхняя граница того, сколько токенов вернет API ChatGPT                                                                                                                                                                                                                     | `1200` для GPT-3, `2400` для GPT-4 |
| `VISION_MAX_TOKENS`      | Верхняя граница того, сколько токенов вернут модели зрения                                                                                                                                                                                                                   | `300` для gpt-4-vision-preview |
| `VISION_MODEL`           | Модель зрения для использования. Допустимые значения: `gpt-4-vision-preview`                                                                                                                                                                                                   | `gpt-4-vision-preview`        |
| `ENABLE_VISION_FOLLOW_UP_QUESTIONS` | Если true, после отправки изображения боту он использует настроенную модель VISION_MODEL до завершения разговора. В противном случае он использует OPENAI_MODEL для продолжения разговора. Допустимые значения: `true` или `false` | `true`                   |
| `MAX_HISTORY_SIZE`       | Максимальное количество сообщений, хранимых в памяти, после чего разговор будет суммирован, чтобы избежать чрезмерного использования токенов                                                                                                                                  | `15`                          |
| `MAX_CONVERSATION_AGE_MINUTES` | Максимальное количество минут, в течение которого разговор должен жить с момента последнего сообщения, после чего разговор будет сброшен                                                                                                                                 | `180`                         |
| `VOICE_REPLY_WITH_TRANSCRIPT_ONLY` | Отвечать на голосовые сообщения только транскриптом или с ответом ChatGPT на транскрипт                                                                                                                                                                                    | `false`                       |
| `VOICE_REPLY_PROMPTS`    | Список фраз, разделенных точкой с запятой (например, `Привет бот; Привет, чат`). Если транскрипт начинается с любой из них, он будет рассматриваться как подсказка, даже если `VOICE_REPLY_WITH_TRANSCRIPT_ONLY` установлен в `true`                                                                                                      | -                             |
| `VISION_PROMPT`          | Фраза (например, `Что на этом изображении`). Модели зрения используют ее как подсказку для интерпретации отправленного изображения. Если в отправленном боту изображении есть подпись, это подменяет этот параметр                                                                                                                        | `Что на этом изображении`     |
| `N_CHOICES`              | Количество ответов для генерации на каждое входное сообщение. **Примечание**: установка этого значения больше 1 не будет работать должным образом, если включена потоковая передача `STREAM`                                                                                   | `1`                           |
| `TEMPERATURE`            | Число от 0 до 2. Большие значения сделают выход более случайным                                                                                                                                                                                                                | `1.0`                         |
| `PRESENCE_PENALTY`       | Число от -2.0 до 2.0. Положительные значения наказывают новые токены в зависимости от того, появляются ли они в тексте до этого                                                                                                                                               | `0.0`                         |
| `FREQUENCY_PENALTY`      | Число от -2.0 до 2.0. Положительные значения наказывают новые токены на основе их существующей частоты в тексте до этого                                                                                                                                                     | `0.0`                         |
| `IMAGE_FORMAT`           | Режим получения изображения Telegram. Допустимые значения: `document` или `photo`                                                                                                                                                                                               | `photo`                       |
| `IMAGE_MODEL`            | Используемая модель DALL·E. Доступные модели: `dall-e-2` и `dall-e-3`, найдите текущие доступные модели [здесь](https://platform.openai.com/docs/models/dall-e)                                                                                                              | `dall-e-2`                    |
| `IMAGE_QUALITY`          | Качество изображений DALL·E, доступно только для модели `dall-e-3`. Возможные варианты: `standard` или `hd`, учитывайте [различия в ценообразовании](https://openai.com/pricing#image-models).                                                                                      | `standard`                    |
| `IMAGE_STYLE`            | Стиль для генерации изображений DALL·E, доступен только для модели `dall-e-3`. Возможные варианты: `vivid` или `natural`. Проверьте доступные стили [здесь](https://platform.openai.com/docs/api-reference/images/create).                                                             | `vivid`                       |
| `IMAGE_SIZE`             | Размер генерируемого изображения DALL·E. Должен быть `256x256`, `512x512` или `1024x1024` для dall-e-2. Должен быть `1024x1024` для моделей dall-e-3.                                                                                                                      | `512x512`                     |
| `VISION_DETAIL`          | Параметр детализации для моделей зрения, объясненный [Руководство по зрению](https://platform.openai.com/docs/guides/vision). Допустимые значения: `low` или `high`                                                                                                           | `auto`                        |
| `GROUP_TRIGGER_KEYWORD`  | Если установлено, бот в групповых чатах будет отвечать только на сообщения, начинающиеся с этого ключевого слова                                                                                                                                                             | -                             |
| `IGNORE_GROUP_TRANSCRIPTIONS` | Если установлено значение true, бот не будет обрабатывать транскрипции в групповых чатах                                                                                                                                                                                     | `true`                        |
| `IGNORE_GROUP_VISION`    | Если установлено значение true, бот не будет обрабатывать запросы зрения в групповых чатах                                                                                                                                                                                     | `true`                        |
| `BOT_LANGUAGE`           | Язык общих сообщений бота. В настоящее время доступны: `en`, `de`, `ru`, `tr`, `it`, `fi`, `es`, `id`, `nl`, `zh-cn`, `zh-tw`, `vi`, `fa`, `pt-br`, `uk`, `ms`, `uz`, `ar`.  [Внесите свой вклад с дополнительными переводами](https://github.com/n3d1117/chatgpt-telegram-bot/discussions/219) | `en`                          |
| `WHISPER_PROMPT`         | Чтобы повысить точность службы транскрибации Whisper, особенно для конкретных имен или терминов, вы можете настроить пользовательское сообщение.  [Речь в текст - Подсказки](https://platform.openai.com/docs/guides/speech-to-text/prompting)                                                    | `-`                           |
| `TTS_VOICE`              | Голос для использования в тексте к речи. Допустимые значения: `alloy`, `echo`, `fable`, `onyx`, `nova` или `shimmer`                                                                                                                                                         | `alloy`                       |
| `TTS_MODEL`              | Модель текста в речь для использования. Допустимые значения: `tts-1` или `tts-1-hd`                                                                                                                                                                                           | `tts-1`                       |
| `TTS_AUDIO_FORMAT`       | Формат аудиофайла ответа на текст. Возможные значения: `mp3` или `ogg_vorbis`                                                                                                                                                                                                 | `mp3`                         |
| `TTS_SAMPLE_RATE`        | Частота дискретизации аудиофайла. Допустимые значения: `22050`, `24000`, `44100`, `48000`. Зависит от значения `TTS_AUDIO_FORMAT`.                                                                                                                                              | `22050`                       |
| `TTS_VOLUME`             | Громкость аудиофайла. Значение должно находиться в диапазоне от 0,0 до 2,0.                                                                                                                                                                                                  | `1.0`                         |

### Для Работы с плагинами

В файле .env необходимо прописать:
PLUGINS=weather,ddg_web_search,ddg_translate,ddg_image_search,youtube_audio_extractor,dice,gtts_text_to_speech,auto_tts,whois,webshot
в примере указаны плагины которые не требует дополнительной настройки, и готовы сразу к использованию.
Для работы плагина ютуб ( Извлечение музыки из видео) достаточно просто скопировать ссылку и вставить боту.
Для работы плагина "скриншот сайта" достаточно прислать боту URL страницы сайта.
Для работы плагина погода, просто достаточно написать "какая погода (город) и до 7 дней прогноз."


P.S. для работы на машине для тестов , не используйте PROXY достаточно просто запустить файл main.py
Огромная благодарность [n3d1117](https://github.com/n3d1117) исходный код находится на его странице.