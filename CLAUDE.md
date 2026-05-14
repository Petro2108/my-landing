# CLAUDE.md — Контекст проекта для Claude Code

> Этот файл читается автоматически в начале каждой сессии.
> Обновляй его при значимых изменениях в проекте.

---

## ПРОЕКТ

**Название:** Лендинг-портфолио цифрового специалиста  
**Владелец:** ИП Хололеенко Петр Михайлович  
**Статус:** В активной разработке  
**Последнее обновление:** 2026-05-14

---

## КОНТАКТЫ ВЛАДЕЛЬЦА

| Канал | Значение |
|---|---|
| Email | 3183104@mail.ru |
| Telegram | @petr2108 |
| Телефон | +375 29 652-22-30 |
| GitHub | https://github.com/Petro2108 |

---

## РЕПОЗИТОРИЙ И ДЕПЛОЙ

| | |
|---|---|
| **GitHub** | https://github.com/Petro2108/my-landing |
| **Живой сайт** | https://petro2108.github.io/my-landing/ |
| **Хостинг** | GitHub Pages (ветка `main`, папка `/`) |
| **Деплой** | Автоматически при `git push` в `main` |

---

## ТЕХНИЧЕСКИЙ СТЕК

Сайт — **один файл** `index.html`. Никакого сборщика, никаких зависимостей.

| Слой | Решение |
|---|---|
| Разметка | HTML5 в одном файле |
| Стили | CSS (встроен в `<style>`) |
| Скрипты | Vanilla JS (встроен в `<script>`) |
| Шрифты | Google Fonts CDN (Syne 700/800, Inter 400/500/600, JetBrains Mono 400) |
| Логотип | `logo.png` в корне проекта |
| Форма | Telegram Bot API + Web3Forms (оба параллельно) |

---

## СТРУКТУРА ФАЙЛОВ

```
my-landing/
├── index.html        — весь сайт (разметка + стили + скрипты), ~93 KB
├── logo.png          — логотип (белый через CSS filter: brightness(0) invert(1))
├── brief.md          — техническое задание (v1.1)
├── PLAN.md           — план развития по фазам
├── research.md       — 5-агентное исследование (конкуренты, стек, UX)
├── CLAUDE.md         — этот файл
├── skills-lock.json  — lock-файл скиллов Claude Code
└── .agents/
    └── skills/
        ├── frontend-design/SKILL.md
        └── fullstack-developer/SKILL.md
```

---

## ДИЗАЙН-СИСТЕМА

### Цвета (CSS custom properties)

```css
--bg:       #0F172A   /* основной фон */
--surface:  #1E293B   /* карточки */
--elevated: #263349   /* hover */
--violet:   #6366F1   /* основной акцент */
--cyan:     #06B6D4   /* вторичный акцент */
--green:    #22C55E   /* CTA-кнопки */
--text:     #F8FAFC   /* основной текст */
--text2:    #94A3B8   /* второстепенный */
--muted:    #475569   /* метки, теги */
```

### Шрифты

- **Заголовки:** Syne 700/800
- **Текст:** Inter 400/500/600
- **Код/теги:** JetBrains Mono 400

### Ключевые классы

- `.rev` — элемент анимируется при скролле (IntersectionObserver добавляет `.on`)
- `.d1`, `.d2` — задержки анимации (0.15s, 0.3s)
- `.btn.btn-g` — зелёная CTA-кнопка
- `.btn.btn-ghost` — прозрачная кнопка с рамкой
- `.magnetic` — кнопка с эффектом притяжения (только desktop)
- `.grad` — анимированный градиентный текст (violet → cyan → green)

---

## СЕКЦИИ САЙТА

| ID | Название | Описание |
|---|---|---|
| `#hero` | Hero | Заголовок, лид, CTA, code window |
| `#problem` | Боль | 4 карточки проблем клиентов |
| `#services` | Услуги | 3 карточки: сайты, приложения, ИИ-агенты |
| `#proof` | Доказательство | Метрики: 3 направления / 24ч / 100% |
| `#process` | Процесс | 5 шагов работы |
| `#portfolio` | Портфолио | Bento grid с кейсами (пока заглушки) |
| `#testimonials` | Отзывы | 3 карточки (пока заглушки) |
| `#faq` | FAQ | Аккордеон, 7 вопросов |
| `#cta` | CTA + форма | Финальный призыв, форма, контакты |

---

## ИНТЕГРАЦИИ

### Telegram Bot (уведомления о заявках)

```
Токен: 8471130210:AAEMn_fWRwRI2QFTRMSCy5qigdmp4ZeNphI
Chat ID: 1671119500
```

Форма отправляет POST на `https://api.telegram.org/bot{TOKEN}/sendMessage`.  
Сообщение приходит с именем, email и текстом заявки.

> ⚠️ Токен находится в публичном репозитории — это осознанный компромисс для статичного сайта.
> При злоупотреблении — перевыпустить токен у @BotFather и обновить в index.html.

### Web3Forms (email-уведомления)

```
Access Key: 7c7014b1-ce95-4dde-80cc-97cdf3117357
Email: 3183104@mail.ru
Домен: petro2108.github.io
```

Форма отправляет POST на `https://api.web3forms.com/submit`.  
Оба запроса (Telegram + Web3Forms) идут параллельно через `Promise.all`.  
Успех показывается если хотя бы один из них ответил ок.

---

## АНИМАЦИИ И ИНТЕРАКТИВ

Все десктопные эффекты обёрнуты в `if (!touch)`:

```javascript
const touch = window.matchMedia('(pointer: coarse)').matches;
```

| Эффект | Только desktop | Описание |
|---|---|---|
| Кастомный курсор | ✅ | Dot + ring с lerp 0.13 |
| Cursor trail | ✅ | 6 точек, GPU transform |
| 3D tilt карточек | ✅ | perspective(900px) rotateX/Y на mousemove |
| Magnetic buttons | ✅ | Притяжение в радиусе 80px |
| Parallax aurora | ✅ | Blobs двигаются при скролле |
| Scroll reveal | ❌ | IntersectionObserver, `.rev` → `.on` |
| Counter animation | ❌ | 0 → N за 55 шагов |
| FAQ аккордеон | ❌ | max-height transition |
| Мобильное меню | ❌ | visibility + opacity transition |

---

## МОБИЛЬНАЯ ВЕРСИЯ

- Брейкпоинты: 640px (бургер-меню), 860px (сетки → 1 колонка)
- Высота hero: `100dvh` (учитывает iOS safe area)
- `overflow-x: hidden` на body
- Все cursor/tilt/trail/parallax/magnetic отключены на touch-устройствах
- `prefers-reduced-motion` — отключает анимации

---

## ЧТО СДЕЛАНО ✅

- [x] Прототип index.html — все 9 секций, анимации, мобилка
- [x] Реальные контакты (email, Telegram, телефон, GitHub)
- [x] Деплой на GitHub Pages
- [x] Форма подключена: Telegram бот + Web3Forms
- [x] Логотип logo.png (белый через CSS filter)
- [x] Schema.org Person с реальными данными
- [x] bypassPermissions в ~/.claude/settings.json

## ЧТО ЖДЁТ ⏳

- [ ] Favicon (favicon.io → добавить в `<head>`)
- [ ] OG-изображение 1200×630px (Canva → загрузить как og.png)
- [ ] Реальные кейсы в портфолио (заглушки → реальные проекты)
- [ ] Реальные отзывы
- [ ] Calendly или Cal.com для бронирования звонка
- [ ] Telegram-бот прокси через Cloudflare Worker (скрыть токен)
- [ ] SEO: sitemap.xml, robots.txt, Google Search Console

---

## ПРАВИЛА РАБОТЫ С КОД-БАЗОЙ

1. **Всё в одном файле** — index.html. Не создавать отдельные CSS/JS файлы без явной просьбы.
2. **После правок** — всегда `git add` + `git commit` + `git push`. GitHub Pages обновляется автоматически.
3. **Мобилка прежде всего** — любой новый элемент проверяй на 390px.
4. **Не трогай** без просьбы: Schema.org, prefers-reduced-motion, touch-гейтинг.
5. **Токены** в коде — осознанно, не удалять и не выносить в .env (статичный сайт).
6. **Комментарии в HTML** — секции разделены блоками `<!-- ══ НАЗВАНИЕ ══ -->`.

---

## КЛЮЧЕВЫЕ ДОКУМЕНТЫ

| Файл | Описание |
|---|---|
| `brief.md` | Полное ТЗ: дизайн-система, структура, контент, SEO |
| `PLAN.md` | Roadmap: Фазы 0–6, чеклисты, приоритеты |
| `research.md` | 5-агентное исследование: конкуренты, UX, копирайтинг |
