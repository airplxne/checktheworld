# CheckTheWorld — Резюме проекта

## Концепция
Агрегатор для путешественников и эмигрантов из России/СНГ. Визы, цены на жильё, авиабилеты, города по 180+ странам. Аналоги: Nomad List, Teleport.org, Expatistan — но на русском с live-данными. Целевая аудитория: туристы, цифровые кочевники, эмигранты из РФ.

---

## Технический стек
- **Фронтенд:** Чистый HTML/CSS/JS, без фреймворков
- **Хостинг:** Cloudflare Pages (бесплатно), домен **checktheworld.ru** привязан через Cloudflare DNS. Деплой из GitHub репо `airplxne/checktheworld`, ветка `main`.
- **Auth & DB:** Firebase (Spark plan) — Authentication (Email/Password + Google), Firestore. Проект: `checktheworld-a631d`
- **Email:** EmailJS — Public key: `EWJ_3i5lkBRcBkjMg`, Service: `service_yj0l9dd`, шаблоны: `template_3sirk17` (пользователю), `template_sa9p4su` (владельцу)
- **Партнёрки:** Travelpayouts Drive скрипт `<script nowprocket src='https://tp-em.com/NTE1NTI2.js?t=515526'>` — вставлен в `<head>` всех страниц. Аккаунт: nikromashin07@gmail.com
- **Шрифты:** Playfair Display + Manrope (Google Fonts)

---

## Firebase конфиг
```js
const firebaseConfig = {
  apiKey: "AIzaSyC_erJyfCASKgjMaMXTF4ovqfwfC4-5GmU",
  authDomain: "checktheworld-a631d.firebaseapp.com",
  projectId: "checktheworld-a631d",
  storageBucket: "checktheworld-a631d.firebasestorage.app",
  messagingSenderId: "1006756841439",
  appId: "1:1006756841439:web:ab81d8f3f469ea3d07666a"
};
```
Firebase SDK подключается через CDN: `https://cdn.jsdelivr.net/npm/firebase@9.23.0/`

### Структура Firestore (коллекция `users`)
```
users/{uid}:
  name: string
  email: string
  createdAt: timestamp
  favorites: string[]   // ключи стран: ['thailand', 'georgia', ...]
  visited: string[]     // посещённые страны
  visas: array          // [{country, entry, allowed}]
  currency: string      // 'rub' | 'usd' | 'eur'
  status: string        // 'planning' | 'moving' | 'living' | 'traveling' | 'returned'
```

---

## Цветовая палитра и дизайн
```css
--bg: #faf8f2;          /* молочный фон */
--white: #ffffff;
--black: #1a1a18;
--g500: #484842;        /* основной текст */
--g300: #848480;        /* вторичный текст */
--g100: #e6e2d8;        /* границы */
--green: #1c7a54;       /* акцент */
--green-mid: #30b278;
--green-light: #d8f2e6;
--amber: #e08c2a;       /* янтарный акцент */
--amber-light: #fef3e2;
```
- Навигация: sticky, янтарная нижняя линия 2px
- Блоб-фон: 2 размытых пятна (зелёный + янтарный), анимация
- Анимации появления: IntersectionObserver, opacity 0→1

---

## Файлы проекта

### Лендинг
- **checktheworld_v9.html** — главный лендинг с формой раннего доступа

### Авторизация и кабинет
- **login.html** — страница входа/регистрации (Firebase Auth, Email+Google, индикатор сложности пароля)
- **dashboard.html** — личный кабинет: профиль, избранное, визовый трекер, калькулятор бюджета

### Каталог
- **index.html** — каталог стран с поиском и фильтрами (безвиз / бюджет / климат)

### Страницы стран (9 штук)
- **thailand.html** — эталонный шаблон, самый проработанный
- **serbia.html** — Сербия
- **turkey.html** — Турция
- **armenia.html** — Армения
- **vietnam.html** — Вьетнам
- **china.html** — Китай
- **georgia.html** — Грузия
- **belarus.html** — Беларусь
- **kazakhstan.html** — Казахстан

### Вспомогательные файлы
- **auth-snippet.html** — сниппет Firebase Auth для навигации (кнопка Войти/имя пользователя), вставлен во все страницы
- **sitemap.xml** — создан ✅
- **/home/claude/build_final.py** — Python-скрипт генерации страниц стран из шаблона Тайланда

---

## Структура страницы страны (шаблон thailand.html)

Каждая страница содержит:
1. **Hero** — флаг, название, регион/валюта/TZ, бейдж визы, 4 стата (бюджет/день, аренда/мес, климат, интернет), липкая quick-карточка справа
2. **Визовый режим** — 4 кликабельные карточки → попап с подробностями (VISAS JS object)
3. **Стоимость жизни** — 9 карточек + переключатель валют (₽/$€/местная)
4. **Авиабилеты** — виджет Aviasales (Travelpayouts)
5. **Жильё** — кнопка → Booking.com
6. **Города и курорты** — горизонтальный слайдер, 4 города, кликабельные → попап
7. **Полезные советы** — 5-6 пунктов с длинными текстами
8. **SIM-карта и страховка** — Yesim (eSIM) + Cherehapa (страховка)
9. **Навигация** между странами (пред/след)

---

## Данные по странам

| Страна | Визовый режим | Бюджет/день | Местная валюта | Курс |
|--------|--------------|-------------|----------------|------|
| Тайланд | 60 дней | ~3 200 ₽ | Баты (฿) | ×33 |
| Сербия | 30 дней | ~2 000 ₽ | Динары (дин.) | ×1.2 |
| Турция | 60 дней | ~2 400 ₽ | Лиры (TRY) | ×2.8 |
| Армения | 180 дней | ~1 600 ₽ | Драмы (AMD) | ×43 |
| Вьетнам | 45 дней | ~1 900 ₽ | Донги (K VND) | ×2.3 |
| Китай | 30 дней | ~3 500 ₽ | Юани (¥) | ×12.5 |
| Грузия | 365 дней | ~1 800 ₽ | Лари (GEL) | ×24 |
| Беларусь | Внутренний паспорт | ~2 500 ₽ | Рубли (BYN) | ×30 |
| Казахстан | Внутренний паспорт | ~2 200 ₽ | Тенге (₸) | ×220 |

---

## Travelpayouts партнёрки
Подключены: Aviasales, Суточно.ру, Яндекс Путешествия, Островок, Cherehapa, Yesim, Airalo.

Виджет Aviasales в thailand.html: календарь цен МСК→BKK, цвет `#1c7a54`.

Партнёрские ссылки используемые в страницах:
- Yesim eSIM: `https://yesim.app`
- Cherehapa страховка: `https://cherehapa.ru`
- Booking.com: `https://www.booking.com/country/[код].ru.html`

---

## Что реализовано ✅
- Лендинг v9 с анимациями, лентой стран, попапами, EmailJS, Travelpayouts
- index.html с поиском и фильтрами
- thailand.html с полным функционалом:
  - Кликабельные визовые карточки с детальными попапами
  - Слайдер городов с галереей фото (попапы с местами и фактами)
  - Переключатель валют ₽/$€/Баты
  - Виджет Aviasales
- 8 страниц стран по шаблону Тайланда с полными данными
- Домен checktheworld.ru привязан к Cloudflare Pages ✅
- sitemap.xml создан ✅
- **Личный кабинет (Firebase)** ✅:
  - login.html — вход/регистрация (Email+Password, Google, восстановление пароля)
  - dashboard.html — профиль, статус переезда, избранное, визовый трекер, калькулятор бюджета
  - Кнопка Войти/имя пользователя во всех страницах навигации

---

## Pending задачи

### Кнопка «В избранное» на страницах стран
Нужно добавить кнопку на каждую страницу страны (thailand.html и др.) которая сохраняет страну в `users/{uid}/favorites` в Firestore. Незалогиненным — редирект на login.html.

### Фото городов
Фото в слайдере городов (thailand.html) ссылаются на Unsplash — не загружаются из-за блокировки без реферера. Нужно скачать вручную и положить в папку `img/`:
- bangkok1.jpg, bangkok2.jpg, bangkok3.jpg
- phuket1.jpg, phuket2.jpg, phuket3.jpg
- chiangmai1.jpg, chiangmai2.jpg, chiangmai3.jpg
- и т.д. для каждого города

### Детальные визовые попапы для 8 стран
В build_final.py VISAS JS для 8 стран упрощён (только `main` ключ). Нужно интегрировать FULL_VISAS в build_final.py.

### Авиабилеты для стран (кроме Тайланда)
Виджет Aviasales использует тайландский код (BKK). Нужно обновить destination:
- Сербия: BEG · Турция: IST · Армения: EVN · Вьетнам: HAN
- Китай: PEK · Грузия: TBS · Беларусь: MSQ · Казахстан: ALA

### SEO
- Open Graph мета-теги для каждой страницы
- Schema.org FAQPage разметку
- Зарегистрировать в Google Search Console

### Ещё не сделанные страницы стран
Из лендинга в очереди: Португалия, ОАЭ, Индонезия, Черногория, Германия, Кипр

---

## Продвижение (план)
1. **Telegram** — посты в чатах эмигрантов при запуске
2. **VC.ru** — статья «Как я делал альтернативу Nomad List»
3. **Product Hunt** — запуск в 00:01 SF time
4. **SEO** — под запросы «виза в Грузию для россиян», «переезд в Сербию» и т.д.
5. **Монетизация** — Travelpayouts уже подключён, добавить Booking.com Affiliate и Wise реферальную

---

## Как пересобрать страницы стран
```bash
python3 /home/claude/build_final.py
```
Скрипт берёт шаблон из `/mnt/user-data/uploads/thailand__9__ПОКА__КРАЙНЯЯ.html` и генерирует 8 файлов в `/mnt/user-data/outputs/`.

**Важно:** после пересборки нужно отдельно применить патч с полными данными VISAS и CITIES. Также нужно заново вставить auth-snippet (кнопка Войти) через inject_auth.py.
