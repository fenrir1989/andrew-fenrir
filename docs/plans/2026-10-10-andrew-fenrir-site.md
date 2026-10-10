# Сайт-портфолио andrew-fenrir: Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Публичный одностраничный сайт-портфолио на GitHub Pages о 8 проектах аккаунта fenrir1989.

**Architecture:** Статика без сборки: `index.html` + `assets/css/style.css` + `assets/js/main.js`, раздаётся GitHub Pages из корня `main`. Относительные пути (сайт живёт в подпути `/andrew-fenrir/`). Скриншоты публичных витрин коммитятся в `assets/img/`. `.nojekyll` отключает Jekyll.

**Tech Stack:** HTML5, CSS3 (кастомные свойства, grid, IntersectionObserver), ванильный JS. Шрифты Google Fonts с кириллицей: Unbounded (дисплей), Manrope (текст), JetBrains Mono (метки) — кириллица подтверждена curl-ом по css2 API (16 cyrillic-блоков).

**Spec:** Задача владельца от 10.10.2026: проанализировать проекты GitHub (read-only), построить план, создать публичный репозиторий `andrew-fenrir` и разместить сайт.

## Design Read

Портфолио AI-инженера для клиентов и партнёров. Archetype: Ethereal Glass (OLED-чёрный #050505, mesh-градиенты, glass-карточки, гротеск). Layout: Asymmetrical Bento для проектов. Тема locked dark. Dials: VARIANCE 6 / MOTION 5 / DENSITY 4. Один акцент: мятно-изумрудный `#6ee7b7`. Двоеточие стиля: без em-dash, без AI-фиолетового, без фейковых скриншотов и выдуманных цифр.

## Global Constraints

- Контент сайта на русском, `lang="ru"`.
- Ноль em-dash (`—`) и en-dash-разделителей (`–`) в видимом тексте.
- Все пути к ассетам относительные (`./assets/...`), никаких ведущих слэшей.
- Только проверенные факты: описания проектов из README репозиториев (прочитаны 10.10.2026), домены проверены curl (все 200). Ничего не выдумывать.
- Существующие репозитории не трогать: создание ТОЛЬКО нового репо `andrew-fenrir`.
- Ссылки на приватные репозитории не давать (404 для посетителей); ссылаться на профиль GitHub и публичные витрины.
- `prefers-reduced-motion` уважать; анимировать только `transform` и `opacity`.
- Контраст WCAG AA на всех текстах и кнопках.

## Review Focus

1. Подпуть Pages: любые абсолютные пути сломают ассеты на `fenrir1989.github.io/andrew-fenrir/`. Проверка: curl каждого ассета на живом URL на 200.
2. Кириллица: подгрузка шрифтов без `display=swap` даёт FOIT. Проверка: в URL шрифтов есть `display=swap`.
3. Мобильный коллапс: bento ниже 768px обязан складываться в одну колонку. Проверка: скриншот 390px ширины.
4. Приватность: ни одной ссылки вида `github.com/fenrir1989/<приватный-репо>` в HTML. Проверка: grep по index.html.
5. Честность контента: числа на сайте только из реальных данных API (8 репо, 5 живых витрин). Проверка: сличить с выводом `gh repo list` выше.

---

### Task 1: Скелет репозитория

**Files:**
- Create: `README.md`, `.nojekyll`, `docs/plans/2026-10-10-andrew-fenrir-site.md`

- [ ] **Step 1:** Создать `.nojekyll` (пустой файл), краткий `README.md` репозитория.
- [ ] **Step 2:** `git init`, первый коммит.

### Task 2: Скриншоты публичных витрин

**Files:**
- Create: `assets/img/shot-smyslit.jpg`, `shot-direct.jpg`, `shot-dayrika.jpg`, `shot-seoai.jpg`

- [ ] **Step 1:** Через локальный браузер снять полностраничные скриншоты 4 публичных витрин (smyslit.com, direct.fenrirai.tech, dayrika.ru, seoai.fenrirai.tech), ширина 1440.
- [ ] **Step 2:** Проверка: 4 файла >20 KB каждый, открываются как изображения.

### Task 3: Контент и разметка `index.html`

**Files:**
- Create: `index.html`

- [ ] **Step 1:** Секции: hero (имя, подзаголовок ≤20 слов, 2 CTA), проекты (bento из 8 карточек, 4 со скриншотами и ссылками на живые витрины), стек, контакт, футер. Floating glass nav. OG-теги, favicon.svg, meta description.
- [ ] **Step 2:** Проверка: `grep -c 'github.com/fenrir1989/'` находит только ссылки на профиль; em-dash отсутствует: `grep -c '—' index.html` = 0.

### Task 4: Дизайн-система `style.css`

**Files:**
- Create: `assets/css/style.css`

- [ ] **Step 1:** Токены (фон #050505, акцент #6ee7b7, радиусы 1.5rem/2rem, glass-панели), bento grid `grid-template-columns: repeat(12, 1fr)`, коллапс в 1 колонку <768px, reveal-анимации через `.in-view`, `@media (prefers-reduced-motion: reduce)` гасит движение.
- [ ] **Step 2:** Проверка: в css нет `position: fixed` blur на скроллящихся контейнерах; только `transform`/`opacity` в transition.

### Task 5: Motion `main.js`

**Files:**
- Create: `assets/js/main.js`

- [ ] **Step 1:** IntersectionObserver для reveal (stagger 60ms), без `window.addEventListener('scroll')`.
- [ ] **Step 2:** Проверка: `grep -c "addEventListener('scroll'" assets/js/main.js` = 0.

### Task 6: Локальная проверка

- [ ] **Step 1:** `python -m http.server` в корне, curl `http://localhost:8000/` = 200, все ассеты 200.
- [ ] **Step 2:** Скриншот локальной страницы (desktop 1440 + mobile 390), визуальный контроль вёрстки.

### Task 7: Публикация

- [ ] **Step 1:** Коммит всех файлов.
- [ ] **Step 2:** `gh repo create andrew-fenrir --public --source=. --push`.
- [ ] **Step 3:** `gh api -X POST repos/fenrir1989/andrew-fenrir/pages -f "source[branch]=main" -f "source[path]=/"`.
- [ ] **Step 4:** Опрос `https://fenrir1989.github.io/andrew-fenrir/` до 200 (первая сборка ~1 мин).

### Task 8: Визуальная приёмка

- [ ] **Step 1:** Скриншоты живого сайта (desktop + mobile), визуальный судья по чек-листу design-скиллов.
- [ ] **Step 2:** Найденные дефекты правятся и пушатся, повторная проверка живого URL.
