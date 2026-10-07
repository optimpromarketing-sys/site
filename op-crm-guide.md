# Оптим Про — гайд по созданию страниц для Битрикс24 Маркетплейс

Все лендинги генерируются скриптом `gen2.js` → единый шаблон → 5 HTML-файлов.
CSS и JS хранятся в `op-crm.css` / `op-crm.js` (справочники; в страницы встроены inline).
Общий каталог приложений — `apps-page.html` (отдельная страница, не генерируется gen2.js).

---

## Дизайн-система

### Цвета
```
--cyan:   #2FC7F7   ← основной акцент, иконки, метки
--blue:   #2E7DF6   ← кнопки, градиенты
--violet: #7A5CFF   ← вторичный акцент, градиенты
--muted:  #8A95B0   ← вспомогательный текст
Фон:      #080c17   ← обязательно для .op-root (Битрикс24 перебивает body)
Текст:    #e8edf8
```

**Акцентный градиент страницы** — каждое приложение задаёт пару `--g1` / `--g2`:
```
OptimGPT:     #2E7DF6 → #7A5CFF
Геотрекер:    #2FC7F7 → #2E7DF6
Паспорт:      #0A2A6B → #2E7DF6
Трудозатраты: #2E7DF6 → #2FC7F7
Переводчик:   #7A5CFF → #2FC7F7
```

### Шрифты
```
Display (заголовки): Montserrat 700/800 — fonts.bunny.net
Body (текст):        Open Sans → Arial (системный fallback)
```

> **Важно для Битрикс24:** Montserrat загружается только как weight 400.
> Жирность имитируется через `-webkit-text-stroke`:

| Элемент | Stroke |
|---------|--------|
| `.hero-h1` | `0.7px #fff` |
| `.section-h2`, `.cta-h2` | `0.6px #fff` |
| `.feat-title`, `.setup-title`, `.pain-title` | `0.5px #e8edf8` |
| `.stat-num`, `.segment-title`, `.section-label` | `0.4–0.5px accent` |
| `.faq-q`, `.other-name`, `.lm-title` | `0.4px #e8edf8` |
| `.trust-label` | `0.3px #e8edf8` |
| `.alt-text h3` (градиентный текст) | `0px transparent` — stroke отключён |

### Градиентный текст
```css
.my-gradient-text {
  background: linear-gradient(135deg, #2FC7F7, #7A5CFF);
  -webkit-background-clip: text;
  background-clip: text;
  -webkit-text-fill-color: transparent;
  /* ВАЖНО: хардкод цветов, не CSS-переменные — они могут не резолвиться в Б24 */
  /* ВАЖНО: -webkit-text-stroke должен быть 0 transparent, иначе обводка перекроет градиент */
}
```

---

## Структура HTML-страницы (лендинг приложения)

```html
<body>
<div class="op-root">                          <!-- обязательная обёртка! -->

  <!-- Шрифт и стили — внутри body (head может срезать Б24) -->
  <link rel="stylesheet" href="https://fonts.bunny.net/css?family=montserrat:400,600,700,800&display=swap">
  <style>/* op-crm.css встроен inline */</style>

  <!-- Прогресс-бар -->
  <div id="opProg"></div>

  <!-- Свечение за курсором -->
  <div id="page-glow"></div>

  <!-- HERO ─────────────────────────── -->
  <section class="hero-section">
    <div class="container">
      <div class="hero-eyebrow">Метка · Категория</div>
      <h1 class="hero-h1">Заголовок приложения</h1>
      <p class="hero-hook">Один абзац — главная ценность</p>
      <p class="hero-who">Для кого: роль / отдел / размер компании</p>
      <div class="hero-cta">
        <a class="op-btn op-btn-primary" href="ССЫЛКА_НА_МАРКЕТПЛЕЙС">Установить бесплатно</a>
        <a class="op-btn op-btn-ghost" href="https://b24.pro/#b27818">Получить консультацию</a>
      </div>
    </div>
  </section>

  <!-- СТАТИСТИКА ───────────────────── -->
  <section class="stats-row">
    <div class="stat-item">
      <div class="stat-num">500+</div>
      <div class="stat-label">компаний используют</div>
    </div>
  </section>

  <!-- БОЛИ / ПРОБЛЕМЫ ──────────────── -->
  <section class="section">
    <div class="section-label">Проблема</div>
    <h2 class="section-h2">Что мешает сейчас</h2>
    <div class="pain-list">
      <div class="pain-item">
        <div class="pain-icon"><!-- SVG иконка --></div>
        <div class="pain-title">Боль клиента</div>
        <p class="pain-p">Описание проблемы 1–2 предложения.</p>
      </div>
    </div>
  </section>

  <!-- ФИЧИ ────────────────────────── -->
  <section class="section">
    <div class="section-label">Возможности</div>
    <h2 class="section-h2">Что умеет приложение</h2>
    <div class="feat-grid">
      <div class="feat-card">
        <div class="feat-icon"><!-- SVG --></div>
        <div class="feat-title">Название фичи</div>
        <p class="feat-body">Описание.</p>
      </div>
    </div>
  </section>

  <!-- СКРИНШОТЫ (alt-blocks) ────────── -->
  <!-- Чётные блоки: картинка справа. Нечётные: картинка слева. -->
  <div class="alt-block">
    <div class="alt-img-wrap" data-shot="0"><!-- base64 img --></div>
    <div class="alt-text">
      <h3>Заголовок экрана</h3>  <!-- градиентный текст, stroke = 0 -->
      <p>Описание что показано на скриншоте.</p>
    </div>
  </div>

  <!-- ДЛЯ КОГО ────────────────────── -->
  <section class="section">
    <div class="section-label">Аудитория</div>
    <h2 class="section-h2">Кому подойдёт</h2>
    <div class="segment-grid">
      <div class="segment-card">
        <div class="segment-title">Название сегмента</div>
        <p>Описание.</p>
      </div>
    </div>
  </section>

  <!-- КАК ПОДКЛЮЧИТЬ ───────────────── -->
  <section class="section">
    <div class="section-label">Установка</div>
    <h2 class="section-h2">3 шага — и готово</h2>
    <div class="setup-grid">
      <div class="setup-step">
        <div class="setup-num">01</div>
        <div class="setup-title">Шаг первый</div>
        <p>Описание.</p>
      </div>
    </div>
  </section>

  <!-- РЕЗУЛЬТАТ (цитата/кейс) ─────── -->
  <div class="result-callout">
    «Текст отзыва или результата внедрения.»
    <div class="result-author">Имя, Должность, Компания</div>
  </div>

  <!-- FAQ ──────────────────────────── -->
  <section class="section">
    <h2 class="section-h2">Вопросы и ответы</h2>
    <div class="faq-list">
      <div class="faq-item">
        <button class="faq-q">Вопрос? <span class="faq-arrow">+</span></button>
        <div class="faq-a"><p>Ответ.</p></div>
      </div>
    </div>
  </section>

  <!-- CTA ФИНАЛЬНЫЙ ───────────────── -->
  <section class="cta-section">
    <h2 class="cta-h2">Попробуйте бесплатно</h2>
    <p class="cta-text">Установка через маркетплейс Битрикс24 — одна кнопка.</p>
    <a class="op-btn op-btn-primary" href="ССЫЛКА">Установить</a>
    <a class="op-btn op-btn-ghost" href="https://b24.pro/#b27818">Получить консультацию</a>
  </section>

  <!-- ДРУГИЕ ПРИЛОЖЕНИЯ — генерируется автоматически из APPS в gen2.js -->

  <!-- ЛАЙТБОКС ────────────────────── -->
  <div class="lb-overlay" id="opLb">
    <button id="lbClose">×</button>
    <button class="lb-nav lb-prev" id="lbPrev">‹</button>
    <img id="lbImg" src="" alt="">
    <button class="lb-nav lb-next" id="lbNext">›</button>
    <div class="lb-count" id="lbCount"></div>
  </div>

</div><!-- /.op-root -->
</body>
```

---

## Кнопки

```html
<!-- Основная (градиент) -->
<a class="op-btn op-btn-primary" href="...">Текст</a>

<!-- Призрак (обводка) -->
<a class="op-btn op-btn-ghost" href="...">Текст</a>
```

**Ссылки:**
- Маркетплейс: из карточки приложения на `marketplace.1c-bitrix.ru`
- Консультация: `https://b24.pro/#b27818`

---

## Анимации при скролле

Элементы стартуют невидимыми (`opacity:0`) и появляются когда попадают в viewport.
Класс `.is-v` добавляется через `IntersectionObserver` с fallback на 2.5 сек.

| Класс | Анимация входа |
|-------|----------------|
| `.feat-card` | снизу + scale(.85) |
| `.pain-item` | слева (-50px) + scale(.97) |
| `.segment-card` | снизу + scale(.88) |
| `.setup-step` | слева (-70px) + scale(.96) |
| `.alt-block` (чётные) | справа + scale(.96) |
| `.alt-block` (нечётные) | слева + scale(.96) |
| `.section-label` | слева (-40px) |
| `.section-h2` | снизу + scale(.95) |
| `.result-callout` | снизу |
| `.trust-item` | снизу |

Все используют `cubic-bezier(.16,1,.3,1)` — упругий вход.

---

## Встроенные изображения (base64)

Битрикс24 не может загрузить относительные пути. Все картинки встраиваются как `data:image/webp;base64,...`

**Рабочий процесс:**
```bash
# 1. Положите скриншоты в папку bg/appname/
# 2. Сгенерируйте WebP-кеш (900px, quality 45, ~30 KB на картинку):
python3 gen_imgcache.py
# 3. Сгенерируйте HTML-страницы:
node gen2.js
```

`gen_imgcache.py` сжимает PNG → WebP и сохраняет base64 в `img_cache.json`.
При сборке `gen2.js` читает кеш и встраивает картинки inline — без внешних запросов.

---

## Добавление нового приложения

1. Создайте `bg/newapp/newapp1.png … newappN.png`
2. Запустите `python3 gen_imgcache.py`
3. Добавьте объект в массив `APPS` в `gen2.js`:

```js
{
  id: 'newapp',
  file: 'newapp.html',
  name: 'Название Приложения',
  tag: 'Категория · Подкатегория',
  url: 'https://www.bitrix24.ru/apps/?app=...',
  icon: 'https://...icon.png',
  g1: '#2E7DF6', g2: '#7A5CFF',
  shots: [1,2,3,4,5].map(i => `bg/newapp/newapp${i}.png`),
  hook: 'Главная ценность одним предложением',
  who:  'Для <b>кого</b> это приложение',
  feats: [
    ['Название фичи', 'Что делает'],
  ],
  pains: [
    { title: 'Боль', p: 'Описание.' },
  ],
  stats: [
    { n: '500+', l: 'компаний' },
  ],
  segments: [
    { title: 'Сегмент', p: 'Описание.' },
  ],
  steps: [
    { t: 'Шаг 1', p: 'Описание.' },
  ],
  faq: [
    { q: 'Вопрос?', a: 'Ответ.' },
  ],
  result: 'Текст отзыва.',
  cta: 'https://www.bitrix24.ru/apps/?app=...',
},
```

4. `node gen2.js` — файл `newapp.html` появится в папке.

---

## Общая страница каталога (apps-page.html)

`apps-page.html` — самостоятельная страница, не генерируется `gen2.js`.
Обёртка: `.op-crm` (не `.op-root`).

### Параллакс-объекты (`.op-px`)
Плавающие изображения на фоне. Поведение:
- `data-target="appId"` — объект следует за карточкой этого приложения
- `data-sticky="1"` — объект остаётся видимым даже если карточка скрыта фильтром
- Оба атрибута вместе: нормальный параллакс когда карточка видна; при фильтрации — объект держится на месте (opacity 0.6)
- Авто-затухание у верхнего края (чтобы не перекрывать hero-текст)

```html
<div class="op-px" data-target="gpt" data-sticky="1" data-rot="-7"
     style="top:900px;left:-185px;width:380px">
  <img src="data:image/webp;base64,..." alt="">
</div>
```

### Поиск и синонимы
Поиск ищет по подстроке в поле `kw` каждого приложения + имя, тег, категории, описания.

```js
{
  id: 'gpt',
  kw: 'нейросеть нейро ии искусственный интеллект chatgpt ассистент бот',
  // ...
}
```

### Цветовые переопределения для Б24
Блок в конце `<style>` с `.op-crm .selector { color: ... !important }`:
- Все `h1–h6` → белые
- Цветные элементы (теги, стрелки, голубые em) → `-webkit-text-stroke: 0 !important`
- Фон `.op-crm` → `#080c17 !important`

---

## Битрикс24-специфика

| Проблема | Причина | Решение |
|----------|---------|---------|
| Белый фон, текст невидим | Б24 перебивает `body{background:white}` | `.op-root{background:#080c17}` |
| Тонкий шрифт | Montserrat грузится только как 400 | `-webkit-text-stroke` имитирует жирность |
| Градиентный текст не работает | CSS-переменные не резолвятся | Хардкод `#2FC7F7`, `#7A5CFF` |
| Обводка на голубом тексте | stroke наследуется от родителя | `-webkit-text-stroke: 0 !important` на цветных элементах |
| Подчёркивание ссылок | Б24 добавляет `a{text-decoration:underline}` | `.op-root a:hover{text-decoration:none !important}` |
| `<style>` в `<head>` не работает | Б24 срезает `<head>` | Стили внутри `<body>` внутри `.op-root` |
| Картинки не грузятся | Относительные пути не работают | Все img → base64 data URI |
| Параллакс пропадает при фильтрации | Объект скрывается вместе с картой | `data-sticky="1"` держит объект видимым |


---

## Новые компоненты (из hr_partners)

### Scroll-reveal `.op-rv`

Альтернатива per-element reveal — вешается на любой элемент:

```html
<section class="op-rv">…</section>
<div class="op-rv">…</div>
```

JS из `op-crm.js` автоматически добавляет `.is-v` через `IntersectionObserver`. Цвет — нет (только видимость). Работает параллельно с существующим `revealSel`.

---

### Glow wrap `.op-glow-wrap`

Декоративное cyan-свечение в правом верхнем углу секции:

```html
<section class="op-glow-wrap">…</section>
```

Реализован через `::after` pseudo-element. Не требует JS.

---

### Marquee / бесконечная лента

**CSS:** `@keyframes opMarquee` + `.op-mq-wrap` / `.op-mq-track`

**Для десктопа** (статичная лента, дубли добавлены вручную или сервером):
```html
<div class="op-mq-wrap">
  <div class="op-mq-track">
    <span>Элемент 1</span>
    <span>Элемент 2</span>
    <!-- дубли — те же элементы × 2 для бесшовного loop -->
    <span>Элемент 1</span>
    <span>Элемент 2</span>
  </div>
</div>
```

**Для мобильных** (JS дублирует автоматически):
```html
<div class="op-mq-wrap op-mq-auto">
  <div class="op-mq-track">
    <span>Элемент 1</span>
    <span>Элемент 2</span>
  </div>
</div>
```

`initMobileMarquee(680)` из `op-crm.js` запускается автоматически. Для другого breakpoint вызовите вручную.

**Скорость:** переопределите `animation-duration` на обёртке:
```css
.my-strip .op-mq-track { animation-duration: 16s; }
```

---

### Sol-wheel карусель

Вертикальная карточная карусель: карточки стекируются, текущая видна. Смена по клику, скроллу, тачу.

```html
<div class="op-sol-wrap">
  <div class="op-sol-card">Карточка 1</div>
  <div class="op-sol-card">Карточка 2</div>
  <div class="op-sol-card">Карточка 3</div>
</div>
<div class="op-sol-dots" id="solDots"></div>
```

JS из `op-crm.js` автоматически инициализирует первый `.op-sol-wrap` на странице. Для нескольких каруселей — вызовите вручную:
```js
initSolWheel('#myWrap', '#myDots');
```

Кнопки/ссылки внутри карточки добавьте атрибут `data-no-wheel` чтобы клик не менял слайд:
```html
<a href="…" data-no-wheel>Подробнее</a>
```

| Класс | Состояние |
|-------|-----------|
| `sol-cur` | Активная карточка |
| `sol-below` | Ниже (следующая) |
| `sol-above` | Выше (предыдущая) |
| `sol-anim` | Во время перехода |
| `.is-act` на dot | Активная точка |

---

### FAQ numbered (стиль 2) `.op-faq2`

Пронумерованный аккордеон с градиентными номерами.

```html
<div class="op-faq2">
  <div class="op-faq2-item">
    <div class="op-faq2-q">
      <div class="op-faq2-num">01</div>
      <div class="op-faq2-qtext">Вопрос?</div>
      <button class="op-faq2-btn">
        <svg viewBox="0 0 16 16"><path d="M4 6l4 4 4-4" stroke="currentColor" stroke-width="2" fill="none"/></svg>
      </button>
    </div>
    <div class="op-faq2-a">
      <div class="op-faq2-a__inner">Ответ.</div>
    </div>
  </div>
</div>
```

JS из `op-crm.js` обрабатывает автоматически. Один открытый вопрос — остальные закрываются.

**Отличие от `.faq-item`:** нумерация, градиентный номер, кнопка-стрелка вместо `+`.

---

### Соответствие цветов

Все новые компоненты используют те же переменные что и остальные:

| Роль | Цвет |
|------|------|
| Акцент, иконки, hover | `#2FC7F7` (cyan) |
| Градиент (от) | `#2FC7F7` |
| Градиент (до) | `#7A5CFF` (violet) |
| Вспомогательный текст | `#8A95B0` (muted) |
| Фон | `#080c17` |
