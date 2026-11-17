# Портфолио — Дмитрий

Одностраничный сайт-портфолио для холодных контактов с ресторанами. Один файл `index.html`, без сборки и фреймворков. Внутри — большой кейс «Терра» (обезличенное демо), секция «Ещё концепты» с кофейней «ПЕНА», стейк-хаусом «УГОЛЬ» и грузинским бистро «СОЛЬ», процесс, цена и контакты.

## Как открыть

Дважды кликнуть `start.bat` — откроется http://127.0.0.1:8081/

Или вручную из папки `portfolio`:

```
python -m http.server 8081
```

и открыть http://127.0.0.1:8081/

## Что заменить перед публикацией

1. **Контакты.** Уже стоят реальные: Telegram `@we3monster`, телефон `+7 (918) 046-68-28`, почта `dimaboyko789@gmail.com`. При смене — менять в трёх местах: контактный блок, JSON-LD и этот файл.
2. **Домен и OG.** После деплоя заменить `og:image` на абсолютный URL вида `https://ваш-домен/img/og.png`, добавить `og:url` и canonical.
3. **Демо и фото.** Кейс «Терра» — обезличенное демо: бренд, телефон, адрес, отзывы и фотографии вымышлены, снимки — Unsplash. Исходное демо «Муссонов» с реальными данными и фото из Яндекс Карт лежит в `mussony-demo/` и не публикуется. Если ресторан ответит и разрешит кейс — вернуть реальные данные.
4. **Демо.** `demo/` — обезличенная копия `mussony-demo` (бренд «Терра», локальная карта OSM). Если правишь демо, синхронизируй вручную.
5. **Форма в демо.** Демонстрационная: показывает успех, но заявки никуда не уходят. В портфолио это подписано.
6. **Концепты.** `concepts/coffee/`, `concepts/steak/` и `concepts/salt/` — копии сайтов из корневой папки `concepts/`. После правок их нужно синхронизировать вручную (команда ниже).

## Структура

```
portfolio/
  index.html              — сам сайт
  img/                    — скриншоты кейса, обложки концептов, OG-картинка
  demo/                   — обезличенное демо «Терра»
  demo/anime.html         — вариант первого экрана на anime.js (для сравнения, в навигации не ссылается)
  demo/vendor/            — anime.js 4.5.0, локально, 118 КБ (≈40 КБ gzip)
  concepts/coffee/        — копия сайта кофейни «ПЕНА»
  concepts/steak/         — копия сайта стейк-хауса «УГОЛЬ»
  concepts/salt/          — копия сайта бистро «СОЛЬ»
  preview/og.html         — исходник OG-картинки
  start.bat               — локальный запуск на 8081
```

## Секции

- **01 Кейс «Терра»** — большой разбор: задача, решение, скриншоты, детали.
- **02 Ещё концепты** — три карточки с обложками и бейджами «концепт», ведут на локальные копии сайтов. Все бренды вымышлены, фото — Unsplash; это подписано под карточками.
- **03 Как я работаю** — этапы и сроки.
- **04 Стоимость** — 45 000 ₽, что входит, поддержка.
- **05 Обо мне** — коротко и без выдуманных регалий.
- **06 Контакты** — Telegram, телефон, почта.

## Синхронизация концептов

После правок в `concepts/coffee/`, `concepts/steak/` или `concepts/salt/` из корня проекта:

```powershell
Remove-Item "portfolio\concepts\coffee\img","portfolio\concepts\steak\img","portfolio\concepts\salt\img" -Recurse -Force -ErrorAction SilentlyContinue
New-Item -ItemType Directory -Force "portfolio\concepts\coffee\img","portfolio\concepts\steak\img","portfolio\concepts\salt\img" | Out-Null
Copy-Item "concepts\coffee\index.html" "portfolio\concepts\coffee\index.html" -Force
Copy-Item "concepts\steak\index.html" "portfolio\concepts\steak\index.html" -Force
Copy-Item "concepts\salt\index.html" "portfolio\concepts\salt\index.html" -Force
Copy-Item "concepts\coffee\img\*.jpg" "portfolio\concepts\coffee\img\" -Force
Copy-Item "concepts\steak\img\*.jpg" "portfolio\concepts\steak\img\" -Force
Copy-Item "concepts\salt\img\*.jpg" "portfolio\concepts\salt\img\" -Force
```

`Copy-Item` не создаёт папку назначения сам: если `img` не существует, он создаст файл `img`, и изображения сломаются. Поэтому `New-Item -ItemType Directory` обязателен.

Обложки карточек (`img/concept-coffee.jpg`, `img/concept-steak.jpg`, `img/concept-salt.jpg`) — отдельные скриншоты 1440×1000. Если меняется первый экран концепта, обложку лучше переснять:

```
python concepts/_viewport.py http://localhost:8082/ shots/coffee-hero.png 1440 1000
python concepts/_viewport.py http://localhost:8083/ shots/steak-hero.png 1440 1000
python concepts/_viewport.py http://localhost:8084/ shots/salt-hero.png 1440 900
```

и пересохранить в JPEG q82.

## Цифры, которые обещаем

- 45 000 ₽ под ключ, оплата 50/50.
- Запуск за 3–5 дней с момента получения фото и финального меню.
- 3 месяца поддержки, дальше — 4 000–5 000 ₽/мес по желанию.
- Первому клиенту — домен и хостинг на год за наш счёт.

Если условия меняются — менять их сразу в `index.html`, а не только в переписке.

## Деплой

**Прод:** https://dmitriyweb33.github.io/ — GitHub Pages, репозиторий `Dmitriyweb33/dmitriyweb33.github.io`, ветка `main`, папка `/`.
Деплой: файлы папки `portfolio` (без `start.bat` и `README.md`) загружаются через Git Data API — blobs → tree → commit → ref. Сборка Pages занимает ~30 секунд.

Скрипт деплоя — `build/_deploy.py`, токен передаётся только через переменную окружения:

```powershell
$env:GH_TOKEN = '<fine-grained token, Contents: Read and write>'
python build\_deploy.py
Remove-Item Env:\GH_TOKEN
```

После деплоя токен отзывать: Settings → Developer settings → Personal access tokens.

Почему ушли с Netlify: из Краснодара он отдавал ~1 КБ/с, HTML догружался, а картинки — нет. GitHub Pages с той же сети: 100 КБ/с – 1 МБ/с.

Важно:
- На главной и в `demo/` стоит `<meta name="robots" content="noindex, nofollow">` — снять после оплаты/разрешения ресторана.
- `og:image`, `og:url` и `canonical` — абсолютные, с доменом `dmitriyweb33.github.io`. При переезде на свой домен заменить и не забыть `CNAME`.
- Токен доступа в файлах не хранить. Для повторного деплоя — новый токен.
- Репозиторий публичный: на бесплатном плане GitHub Pages иначе не работает.
- После деплоя проверить: «Открыть концепт», карточки «ПЕНА»/«УГОЛЬ», контакты и превью ссылки в Telegram.

## Проверка перед отправкой

- Открыть на 1440 px и на 390 px.
- Консоль без ошибок, все ссылки и изображения живые (включая `/concepts/coffee/` и `/concepts/steak/`).
- Lighthouse: доступность, SEO, best practices.
- Превью ссылки в Telegram выглядит нормально (OG-картинка).

## Честность

Не выдумывать клиентов и цифры результатов. «Терра» — вымышленный бренд: в публичном кейсе нет ни реального названия, ни фотографий, ни отзывов заведения. «ПЕНА», «УГОЛЬ» и «СОЛЬ» — тоже вымышленные бренды. Везде, где нужно, стоит подпись «концепт».
