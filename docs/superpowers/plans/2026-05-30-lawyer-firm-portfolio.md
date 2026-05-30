# Law Firm Portfolio SPA — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a single-page static website (Ukrainian) for a law firm ("адвокатське об'єднання") in the "Класична довіра" navy + gold style, with all content driven by a single `SITE_DATA` config object.

**Architecture:** Pure static site — `index.html` (semantic markup), `styles.css` (theme + responsive), `script.js` (one `SITE_DATA` config object at top + DOM rendering + interactions). No build step, no backend, no framework. Opens via `file://`, deployable to Netlify / GitHub Pages.

**Tech Stack:** HTML5, CSS3 (custom properties, grid/flex), vanilla JS (ES6), Google Fonts (Playfair Display + Inter), inline SVG icons, Google Maps embed iframe.

**Verification note:** No Node/test runner exists. Each task is verified by opening `index.html` in a browser (`open index.html` on macOS, or the editor preview panel) and visually confirming the described result, plus checking the browser DevTools console for errors.

---

## File Structure

- Create: `index.html` — all sections as semantic markup; containers that JS fills (`#team-grid`, `#services-grid`, `#testimonials-grid`, `#faq-list`).
- Create: `styles.css` — CSS variables (colors, fonts, spacing), reset, layout, all section styles, responsive breakpoints.
- Create: `script.js` — `SITE_DATA` config object (top), render functions, interaction handlers, init on `DOMContentLoaded`.
- Create: `README.md` — how to edit `SITE_DATA`, how to view, how to deploy.
- Reference: `docs/superpowers/specs/2026-05-30-lawyer-firm-portfolio-design.md` — the source spec.

---

## Task 1: Scaffold + theme base + nav shell

**Files:**
- Create: `index.html`
- Create: `styles.css`
- Create: `script.js`

- [ ] **Step 1: Create `index.html` skeleton**

```html
<!DOCTYPE html>
<html lang="uk">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Адвокатське об'єднання</title>
  <meta name="description" content="Адвокатське об'єднання — юридична допомога фізичним особам і бізнесу.">
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&family=Playfair+Display:wght@600;700;800&display=swap" rel="stylesheet">
  <link rel="stylesheet" href="styles.css">
</head>
<body>
  <header class="nav" id="nav">
    <div class="container nav__inner">
      <a href="#hero" class="nav__logo" id="nav-logo">Назва об'єднання</a>
      <nav class="nav__menu" id="nav-menu">
        <a href="#about">Про нас</a>
        <a href="#team">Команда</a>
        <a href="#services">Послуги</a>
        <a href="#testimonials">Відгуки</a>
        <a href="#faq">FAQ</a>
        <a href="#contacts">Контакти</a>
      </nav>
      <a href="#contacts" class="btn btn--primary nav__cta">Консультація</a>
      <button class="nav__burger" id="nav-burger" aria-label="Меню" aria-expanded="false">
        <span></span><span></span><span></span>
      </button>
    </div>
  </header>

  <main>
    <!-- sections added in later tasks -->
  </main>

  <footer class="footer" id="footer"></footer>

  <button class="to-top" id="to-top" aria-label="Нагору">↑</button>

  <script src="script.js"></script>
</body>
</html>
```

- [ ] **Step 2: Create `styles.css` with theme variables, reset, container, nav, buttons**

```css
:root{
  --navy:#0e1c33;
  --navy-2:#0b1729;
  --card:#13243f;
  --gold:#d9b46a;
  --text:#f4f1ea;
  --muted:#c4cdda;
  --muted-2:#8fa0bb;
  --border:rgba(255,255,255,.08);
  --maxw:1140px;
  --gap:24px;
  --serif:'Playfair Display',Georgia,serif;
  --sans:'Inter',system-ui,sans-serif;
  --radius:12px;
}
*{margin:0;padding:0;box-sizing:border-box}
html{scroll-behavior:smooth;scroll-padding-top:78px}
body{font-family:var(--sans);background:var(--navy);color:var(--text);line-height:1.6}
a{color:inherit;text-decoration:none}
img{max-width:100%;display:block}
.container{max-width:var(--maxw);margin:0 auto;padding:0 24px}
h1,h2,h3{font-family:var(--serif);line-height:1.12;font-weight:700}
.section{padding:84px 0}
.section--alt{background:var(--navy-2)}
.section__eyebrow{color:var(--gold);font-size:13px;letter-spacing:2px;text-transform:uppercase;font-family:var(--sans);font-weight:600;margin-bottom:12px}
.section__title{font-size:34px;margin-bottom:18px}
.section__lead{color:var(--muted);max-width:640px;margin-bottom:40px}

.btn{display:inline-block;padding:13px 26px;border-radius:8px;font-size:15px;font-weight:600;cursor:pointer;border:1px solid transparent;transition:transform .15s ease,opacity .15s ease}
.btn:hover{transform:translateY(-1px)}
.btn--primary{background:var(--gold);color:var(--navy)}
.btn--ghost{background:transparent;color:var(--text);border-color:rgba(255,255,255,.25)}

/* Nav */
.nav{position:sticky;top:0;z-index:50;background:rgba(14,28,51,.92);backdrop-filter:blur(8px);border-bottom:1px solid var(--border)}
.nav__inner{display:flex;align-items:center;justify-content:space-between;height:64px}
.nav__logo{font-family:var(--serif);font-weight:700;color:var(--gold);font-size:20px}
.nav__menu{display:flex;gap:26px;font-size:14px}
.nav__menu a:hover{color:var(--gold)}
.nav__cta{padding:9px 18px;font-size:14px}
.nav__burger{display:none;flex-direction:column;gap:5px;background:none;border:none;cursor:pointer;padding:6px}
.nav__burger span{width:24px;height:2px;background:var(--text);transition:.2s}

@media(max-width:860px){
  .nav__menu{position:fixed;inset:64px 0 auto 0;flex-direction:column;background:var(--navy-2);padding:18px 24px;gap:18px;border-bottom:1px solid var(--border);transform:translateY(-120%);transition:transform .25s ease}
  .nav__menu.is-open{transform:translateY(0)}
  .nav__cta{display:none}
  .nav__burger{display:flex}
}
```

- [ ] **Step 3: Create `script.js` with the full `SITE_DATA` config (placeholder content) and a DOMContentLoaded init stub**

```js
/* =========================================================================
   SITE_DATA — РЕДАГУЙТЕ ТУТ. Усі тексти, посилання й контакти в одному місці.
   Замініть приклади-заглушки на справжні дані об'єднання.
   ========================================================================= */
const SITE_DATA = {
  brand: {
    name: "Право і Захист",
    tagline: "Адвокатське об'єднання",
    heroTitle: "Захищаємо ваші права професійно та до результату",
    heroSubtitle: "Юридична допомога фізичним особам і бізнесу. Чесна оцінка перспектив справи на першій консультації."
  },
  stats: [
    { num: "15+", label: "років практики" },
    { num: "1200+", label: "справ" },
    { num: "94%", label: "виграних справ" }
  ],
  about: {
    text: "Наше адвокатське об'єднання — це команда досвідчених юристів, які працюють заради результату клієнта. Ми поєднуємо глибоке знання права з людяним підходом до кожної справи.",
    advantages: [
      { icon: "shield", title: "Досвід і репутація", text: "Понад 15 років успішної практики в різних галузях права." },
      { icon: "scale", title: "Чесність", text: "Реалістична оцінка перспектив справи без порожніх обіцянок." },
      { icon: "clock", title: "Оперативність", text: "Швидко реагуємо та тримаємо вас у курсі на кожному етапі." },
      { icon: "lock", title: "Конфіденційність", text: "Гарантуємо повну адвокатську таємницю." }
    ]
  },
  team: [
    { name: "Олена Ковальчук", role: "Керуючий партнер · судовий захист", bio: "Спеціалізується на складних судових спорах та представництві в усіх інстанціях.", photo: "" },
    { name: "Андрій Мельник", role: "Партнер · корпоративне право", bio: "Супровід бізнесу, договірне та податкове право, due diligence.", photo: "" },
    { name: "Ірина Шевченко", role: "Адвокат · сімейне право", bio: "Сімейні, спадкові та житлові спори, захист інтересів дітей.", photo: "" }
  ],
  services: [
    { icon: "gavel", title: "Судовий захист", text: "Представництво в судах усіх інстанцій у цивільних, господарських та адміністративних справах." },
    { icon: "home", title: "Нерухомість", text: "Супровід угод, оформлення та спори щодо майна." },
    { icon: "doc", title: "Договірне право", text: "Складання, аналіз та супровід будь-яких договорів." },
    { icon: "briefcase", title: "Бізнес і корпоративне", text: "Реєстрація, супровід та захист інтересів компаній." },
    { icon: "family", title: "Сімейне право", text: "Розлучення, аліменти, поділ майна, спадщина." },
    { icon: "shield", title: "Кримінальний захист", text: "Захист на всіх стадіях кримінального провадження." }
  ],
  testimonials: [
    { text: "Виграли справу, яку інші вважали безнадійною. Дуже вдячна за професіоналізм!", author: "Марія К." },
    { text: "Чітко, швидко й по суті. Супроводжували угоду без жодних проблем.", author: "Олександр В." },
    { text: "Пояснили все простою мовою й вели справу від початку до кінця.", author: "Тетяна Л." }
  ],
  faq: [
    { q: "Скільки коштує перша консультація?", a: "Перша консультація для оцінки перспектив справи безкоштовна." },
    { q: "Чи працюєте ви дистанційно?", a: "Так, ми консультуємо онлайн і ведемо справи по всій Україні." },
    { q: "Скільки триває розгляд справи?", a: "Залежить від категорії справи; на консультації ми надамо реалістичні строки." },
    { q: "Як відбувається оплата послуг?", a: "Вартість фіксуємо в договорі заздалегідь — без прихованих платежів." }
  ],
  social: {
    instagram: "https://instagram.com/",
    youtube: "https://youtube.com/",
    tiktok: "https://tiktok.com/",
    telegram: "https://t.me/"
  },
  contacts: {
    phone: "+38 (000) 000-00-00",
    email: "info@example.com",
    address: "м. Київ, вул. Прикладна, 1",
    hours: "Пн–Пт, 9:00–18:00",
    telegramUrl: "https://t.me/",
    // Google Maps embed: Maps -> Поділитися -> Вбудувати карту -> скопіювати src з iframe
    mapEmbedUrl: "https://www.google.com/maps?q=Київ&output=embed"
  }
};

document.addEventListener('DOMContentLoaded', () => {
  // render + interaction calls added in later tasks
});
```

- [ ] **Step 4: Verify in browser**

Run: `open index.html`
Expected: Sticky navy nav bar with gold logo "Назва об'єднання", menu links, gold "Консультація" button. Resize below 860px → links hide, burger (☰) appears. DevTools console: no errors.

- [ ] **Step 5: Commit**

```bash
git add index.html styles.css script.js
git commit -m "feat: scaffold static site, theme base, nav shell + SITE_DATA"
```

---

## Task 2: Hero section

**Files:**
- Modify: `index.html` (inside `<main>`)
- Modify: `styles.css`
- Modify: `script.js`

- [ ] **Step 1: Add hero markup inside `<main>` in `index.html`**

```html
<section class="hero" id="hero">
  <div class="container hero__inner">
    <div class="hero__content">
      <p class="hero__eyebrow" id="hero-eyebrow"></p>
      <h1 class="hero__title" id="hero-title"></h1>
      <p class="hero__subtitle" id="hero-subtitle"></p>
      <div class="hero__actions">
        <a href="#contacts" class="btn btn--primary">Безкоштовна консультація</a>
        <a href="#services" class="btn btn--ghost">Наші послуги</a>
      </div>
      <div class="hero__stats" id="hero-stats"></div>
    </div>
    <div class="hero__media" id="hero-media" aria-hidden="true"></div>
  </div>
</section>
```

- [ ] **Step 2: Add hero styles to `styles.css`**

```css
.hero{padding:72px 0 64px}
.hero__inner{display:grid;grid-template-columns:1.2fr .8fr;gap:48px;align-items:center}
.hero__eyebrow{color:var(--gold);font-size:13px;letter-spacing:2px;text-transform:uppercase;font-weight:600;margin-bottom:16px}
.hero__title{font-size:46px;margin-bottom:20px}
.hero__subtitle{color:var(--muted);font-size:18px;margin-bottom:30px;max-width:480px}
.hero__actions{display:flex;gap:14px;flex-wrap:wrap}
.hero__stats{display:flex;gap:36px;margin-top:40px}
.hero__stats .num{font-size:30px;font-weight:700;color:var(--gold);font-family:var(--serif)}
.hero__stats .cap{font-size:13px;color:var(--muted-2)}
.hero__media{aspect-ratio:4/5;border-radius:var(--radius);background:linear-gradient(160deg,#1a3157,#0b1526);border:1px solid rgba(217,180,106,.3);display:flex;align-items:center;justify-content:center;color:var(--gold);font-family:var(--serif);font-size:64px}
.hero__media::after{content:"⚖";opacity:.85}
@media(max-width:860px){
  .hero__inner{grid-template-columns:1fr;gap:32px}
  .hero__title{font-size:34px}
  .hero__media{max-width:320px;order:-1}
}
```

- [ ] **Step 3: Add hero render + call inside DOMContentLoaded in `script.js`**

```js
function renderHero(){
  document.getElementById('hero-eyebrow').textContent = SITE_DATA.brand.tagline;
  document.getElementById('hero-title').textContent = SITE_DATA.brand.heroTitle;
  document.getElementById('hero-subtitle').textContent = SITE_DATA.brand.heroSubtitle;
  document.getElementById('nav-logo').textContent = SITE_DATA.brand.name;
  document.title = SITE_DATA.brand.name + " — " + SITE_DATA.brand.tagline;
  document.getElementById('hero-stats').innerHTML = SITE_DATA.stats
    .map(s => `<div><div class="num">${s.num}</div><div class="cap">${s.label}</div></div>`).join('');
}
```

Add `renderHero();` as the first line inside the `DOMContentLoaded` callback.

- [ ] **Step 4: Verify in browser**

Run: `open index.html`
Expected: Hero shows tagline eyebrow, large serif title, subtitle, two buttons, and 3 gold stats from `SITE_DATA`. Nav logo now reads "Право і Захист". On narrow width the media block moves above text. Console clean.

- [ ] **Step 5: Commit**

```bash
git add index.html styles.css script.js
git commit -m "feat: hero section rendered from SITE_DATA"
```

---

## Task 3: About section (text + advantage cards)

**Files:**
- Modify: `index.html`, `styles.css`, `script.js`

- [ ] **Step 1: Add about markup after hero in `index.html`**

```html
<section class="section section--alt" id="about">
  <div class="container">
    <p class="section__eyebrow">Про нас</p>
    <h2 class="section__title">Команда, якій довіряють</h2>
    <p class="section__lead" id="about-text"></p>
    <div class="grid grid--4" id="about-advantages"></div>
  </div>
</section>
```

- [ ] **Step 2: Add grid + card + icon styles to `styles.css`**

```css
.grid{display:grid;gap:var(--gap)}
.grid--4{grid-template-columns:repeat(4,1fr)}
.grid--3{grid-template-columns:repeat(3,1fr)}
.grid--2{grid-template-columns:repeat(2,1fr)}
.card{background:var(--card);border:1px solid var(--border);border-radius:var(--radius);padding:24px;transition:transform .18s ease,border-color .18s ease}
.card:hover{transform:translateY(-3px);border-color:rgba(217,180,106,.35)}
.card__ico{width:44px;height:44px;border-radius:10px;background:rgba(217,180,106,.15);color:var(--gold);display:flex;align-items:center;justify-content:center;margin-bottom:14px}
.card__ico svg{width:22px;height:22px}
.card h3{font-size:18px;margin-bottom:8px;font-family:var(--sans);font-weight:600}
.card p{color:var(--muted);font-size:14px}
@media(max-width:980px){.grid--4{grid-template-columns:repeat(2,1fr)}}
@media(max-width:620px){.grid--4,.grid--3,.grid--2{grid-template-columns:1fr}}
```

- [ ] **Step 3: Add an ICONS map + about render to `script.js`**

Add this ICONS object near the top of `script.js` (after `SITE_DATA`):

```js
/* Inline SVG icons keyed by name used in SITE_DATA. */
const ICONS = {
  shield:'<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8"><path d="M12 3l7 3v5c0 4.5-3 7.5-7 9-4-1.5-7-4.5-7-9V6l7-3z"/></svg>',
  scale:'<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8"><path d="M12 3v18M5 7h14M7 7l-3 6a3 3 0 006 0L7 7zm10 0l-3 6a3 3 0 006 0l-3-6zM8 21h8"/></svg>',
  clock:'<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8"><circle cx="12" cy="12" r="9"/><path d="M12 7v5l3 2"/></svg>',
  lock:'<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8"><rect x="5" y="11" width="14" height="9" rx="2"/><path d="M8 11V8a4 4 0 018 0v3"/></svg>',
  gavel:'<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8"><path d="M14 4l6 6-3 3-6-6 3-3zM9 9l-5 5 3 3 5-5M4 21h8"/></svg>',
  home:'<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8"><path d="M4 11l8-7 8 7M6 10v9h12v-9"/></svg>',
  doc:'<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8"><path d="M7 3h7l4 4v14H7zM14 3v4h4M9 12h6M9 16h6"/></svg>',
  briefcase:'<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8"><rect x="3" y="7" width="18" height="13" rx="2"/><path d="M8 7V5a2 2 0 012-2h4a2 2 0 012 2v2"/></svg>',
  family:'<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8"><circle cx="8" cy="8" r="3"/><circle cx="16" cy="8" r="3"/><path d="M3 20c0-3 2-5 5-5s5 2 5 5M13 20c0-3 2-5 5-5"/></svg>'
};
function icon(name){ return ICONS[name] || ICONS.scale; }

function renderAbout(){
  document.getElementById('about-text').textContent = SITE_DATA.about.text;
  document.getElementById('about-advantages').innerHTML = SITE_DATA.about.advantages
    .map(a => `<div class="card"><div class="card__ico">${icon(a.icon)}</div><h3>${a.title}</h3><p>${a.text}</p></div>`).join('');
}
```

Add `renderAbout();` inside `DOMContentLoaded`.

- [ ] **Step 4: Verify in browser**

Run: `open index.html`
Expected: "Про нас" section (slightly darker bg) with intro text and 4 advantage cards, each with a gold-tinted SVG icon. Cards lift on hover. Below 980px → 2 columns; below 620px → 1 column. Console clean.

- [ ] **Step 5: Commit**

```bash
git add index.html styles.css script.js
git commit -m "feat: about section with advantage cards + inline SVG icons"
```

---

## Task 4: Team section

**Files:**
- Modify: `index.html`, `styles.css`, `script.js`

- [ ] **Step 1: Add team markup after about in `index.html`**

```html
<section class="section" id="team">
  <div class="container">
    <p class="section__eyebrow">Команда</p>
    <h2 class="section__title">Наші адвокати</h2>
    <p class="section__lead">Досвідчені фахівці у ключових галузях права.</p>
    <div class="grid grid--3" id="team-grid"></div>
  </div>
</section>
```

- [ ] **Step 2: Add team card styles to `styles.css`**

```css
.member__photo{width:96px;height:96px;border-radius:50%;object-fit:cover;margin-bottom:16px;background:linear-gradient(160deg,#1a3157,#0b1526);border:1px solid rgba(217,180,106,.3);display:flex;align-items:center;justify-content:center;color:var(--gold);font-family:var(--serif);font-size:32px;font-weight:700}
.member__role{color:var(--gold);font-size:13px;font-weight:600;margin:4px 0 10px}
.member h3{font-size:19px;font-family:var(--sans);font-weight:600}
.member p{color:var(--muted);font-size:14px}
```

- [ ] **Step 3: Add team render to `script.js`**

```js
function initials(name){ return name.split(' ').map(w=>w[0]).slice(0,2).join('').toUpperCase(); }
function renderTeam(){
  document.getElementById('team-grid').innerHTML = SITE_DATA.team.map(m => {
    const photo = m.photo
      ? `<img class="member__photo" src="${m.photo}" alt="${m.name}">`
      : `<div class="member__photo">${initials(m.name)}</div>`;
    return `<div class="card member">${photo}<h3>${m.name}</h3><div class="member__role">${m.role}</div><p>${m.bio}</p></div>`;
  }).join('');
}
```

Add `renderTeam();` inside `DOMContentLoaded`.

- [ ] **Step 4: Verify in browser**

Run: `open index.html`
Expected: "Команда" section with 3 member cards. Since `photo` is empty in placeholders, each shows a round gold-on-navy monogram (e.g. "ОК"), name, gold role line, and bio. Setting a `photo` URL in `SITE_DATA.team` would show the image instead. Console clean.

- [ ] **Step 5: Commit**

```bash
git add index.html styles.css script.js
git commit -m "feat: team section with photo/monogram cards from SITE_DATA"
```

---

## Task 5: Services section

**Files:**
- Modify: `index.html`, `script.js` (styles reuse `.grid--3`/`.card` from Task 3)

- [ ] **Step 1: Add services markup after team in `index.html`**

```html
<section class="section section--alt" id="services">
  <div class="container">
    <p class="section__eyebrow">Послуги</p>
    <h2 class="section__title">Напрями роботи</h2>
    <p class="section__lead">Повний спектр юридичних послуг для фізичних осіб і бізнесу.</p>
    <div class="grid grid--3" id="services-grid"></div>
  </div>
</section>
```

- [ ] **Step 2: Add services render to `script.js`**

```js
function renderServices(){
  document.getElementById('services-grid').innerHTML = SITE_DATA.services
    .map(s => `<div class="card"><div class="card__ico">${icon(s.icon)}</div><h3>${s.title}</h3><p>${s.text}</p></div>`).join('');
}
```

Add `renderServices();` inside `DOMContentLoaded`.

- [ ] **Step 3: Verify in browser**

Run: `open index.html`
Expected: "Послуги" section (darker bg) with 6 cards (icon + title + text) in a 3-column grid, collapsing to 1 column on mobile. Console clean.

- [ ] **Step 4: Commit**

```bash
git add index.html script.js
git commit -m "feat: services section rendered from SITE_DATA"
```

---

## Task 6: Testimonials section

**Files:**
- Modify: `index.html`, `styles.css`, `script.js`

- [ ] **Step 1: Add testimonials markup after services in `index.html`**

```html
<section class="section" id="testimonials">
  <div class="container">
    <p class="section__eyebrow">Відгуки</p>
    <h2 class="section__title">Що кажуть клієнти</h2>
    <div class="grid grid--3" id="testimonials-grid"></div>
  </div>
</section>
```

- [ ] **Step 2: Add quote styles to `styles.css`**

```css
.quote{background:var(--card);border:1px solid var(--border);border-radius:var(--radius);padding:26px;position:relative}
.quote::before{content:"”";font-family:var(--serif);color:var(--gold);font-size:54px;line-height:1;position:absolute;top:10px;right:18px;opacity:.5}
.quote p{font-size:15px;margin-bottom:16px}
.quote__author{color:var(--gold);font-weight:600;font-size:14px}
```

- [ ] **Step 3: Add testimonials render to `script.js`**

```js
function renderTestimonials(){
  document.getElementById('testimonials-grid').innerHTML = SITE_DATA.testimonials
    .map(t => `<div class="quote"><p>${t.text}</p><div class="quote__author">${t.author}</div></div>`).join('');
}
```

Add `renderTestimonials();` inside `DOMContentLoaded`.

- [ ] **Step 4: Verify in browser**

Run: `open index.html`
Expected: "Відгуки" section with 3 quote cards, each with a decorative gold quotation mark, text, and gold author name. Console clean.

- [ ] **Step 5: Commit**

```bash
git add index.html styles.css script.js
git commit -m "feat: testimonials section from SITE_DATA"
```

---

## Task 7: FAQ accordion

**Files:**
- Modify: `index.html`, `styles.css`, `script.js`

- [ ] **Step 1: Add FAQ markup after testimonials in `index.html`**

```html
<section class="section section--alt" id="faq">
  <div class="container container--narrow">
    <p class="section__eyebrow">FAQ</p>
    <h2 class="section__title">Часті запитання</h2>
    <div class="faq" id="faq-list"></div>
  </div>
</section>
```

- [ ] **Step 2: Add FAQ styles to `styles.css`**

```css
.container--narrow{max-width:780px}
.faq__item{border:1px solid var(--border);border-radius:10px;margin-bottom:12px;background:var(--card);overflow:hidden}
.faq__q{width:100%;text-align:left;background:none;border:none;color:var(--text);font-size:16px;font-weight:600;padding:18px 22px;cursor:pointer;display:flex;justify-content:space-between;align-items:center;gap:16px;font-family:var(--sans)}
.faq__q .mark{color:var(--gold);font-size:22px;transition:transform .2s ease;flex:none}
.faq__item.is-open .faq__q .mark{transform:rotate(45deg)}
.faq__a{max-height:0;overflow:hidden;transition:max-height .25s ease}
.faq__a p{padding:0 22px 18px;color:var(--muted);font-size:15px}
```

- [ ] **Step 3: Add FAQ render + toggle to `script.js`**

```js
function renderFAQ(){
  document.getElementById('faq-list').innerHTML = SITE_DATA.faq.map(f =>
    `<div class="faq__item">
       <button class="faq__q" type="button">${f.q}<span class="mark">+</span></button>
       <div class="faq__a"><p>${f.a}</p></div>
     </div>`).join('');

  document.querySelectorAll('.faq__q').forEach(btn => {
    btn.addEventListener('click', () => {
      const item = btn.parentElement;
      const isOpen = item.classList.contains('is-open');
      document.querySelectorAll('.faq__item').forEach(i => {
        i.classList.remove('is-open');
        i.querySelector('.faq__a').style.maxHeight = null;
      });
      if(!isOpen){
        item.classList.add('is-open');
        const ans = item.querySelector('.faq__a');
        ans.style.maxHeight = ans.scrollHeight + 'px';
      }
    });
  });
}
```

Add `renderFAQ();` inside `DOMContentLoaded`.

- [ ] **Step 4: Verify in browser**

Run: `open index.html`
Expected: "FAQ" section (narrow column) with 4 items. Clicking a question expands its answer smoothly and rotates the "+" into "×"; opening another closes the previous (one open at a time). Console clean.

- [ ] **Step 5: Commit**

```bash
git add index.html styles.css script.js
git commit -m "feat: FAQ accordion from SITE_DATA"
```

---

## Task 8: Social links section

**Files:**
- Modify: `index.html`, `styles.css`, `script.js`

- [ ] **Step 1: Add social markup after FAQ in `index.html`**

```html
<section class="section" id="social">
  <div class="container">
    <p class="section__eyebrow">Соцмережі</p>
    <h2 class="section__title">Стежте за нами</h2>
    <p class="section__lead">Корисний контент про право — у зручному для вас форматі.</p>
    <div class="social" id="social-links"></div>
  </div>
</section>
```

- [ ] **Step 2: Add social styles to `styles.css`**

```css
.social{display:grid;grid-template-columns:repeat(4,1fr);gap:var(--gap)}
.social__btn{display:flex;flex-direction:column;align-items:center;gap:12px;padding:28px 16px;background:var(--card);border:1px solid var(--border);border-radius:var(--radius);transition:transform .18s ease,border-color .18s ease}
.social__btn:hover{transform:translateY(-3px);border-color:rgba(217,180,106,.45)}
.social__btn svg{width:34px;height:34px;color:var(--gold)}
.social__btn span{font-weight:600;font-size:15px}
@media(max-width:620px){.social{grid-template-columns:repeat(2,1fr)}}
```

- [ ] **Step 3: Add social icons + render to `script.js`**

Add these entries to the existing `ICONS` object:

```js
  instagram:'<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8"><rect x="3" y="3" width="18" height="18" rx="5"/><circle cx="12" cy="12" r="4"/><circle cx="17.5" cy="6.5" r="1" fill="currentColor" stroke="none"/></svg>',
  youtube:'<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8"><rect x="2" y="5" width="20" height="14" rx="4"/><path d="M10 9l5 3-5 3z" fill="currentColor" stroke="none"/></svg>',
  tiktok:'<svg viewBox="0 0 24 24" fill="currentColor"><path d="M16 3c.3 2.2 1.7 3.9 3.9 4.2v2.8c-1.4 0-2.7-.4-3.9-1.1v5.6c0 3-2.4 5.5-5.5 5.5S5 17.6 5 14.5 7.4 9 10.5 9c.3 0 .6 0 .9.1v2.9c-.3-.1-.6-.2-.9-.2-1.5 0-2.6 1.2-2.6 2.7s1.1 2.7 2.6 2.7 2.6-1.2 2.6-2.7V3H16z"/></svg>',
  telegram:'<svg viewBox="0 0 24 24" fill="currentColor"><path d="M21 4L3 11l5 2 2 6 3-4 4 3 4-14zM9 13l8-5-6 6-.3 3z"/></svg>'
```

Add the render function:

```js
function renderSocial(){
  const s = SITE_DATA.social;
  const items = [
    { key:'instagram', label:'Instagram', url:s.instagram },
    { key:'youtube', label:'YouTube', url:s.youtube },
    { key:'tiktok', label:'TikTok', url:s.tiktok },
    { key:'telegram', label:'Telegram', url:s.telegram }
  ];
  document.getElementById('social-links').innerHTML = items
    .map(i => `<a class="social__btn" href="${i.url}" target="_blank" rel="noopener">${icon(i.key)}<span>${i.label}</span></a>`).join('');
}
```

Add `renderSocial();` inside `DOMContentLoaded`.

- [ ] **Step 4: Verify in browser**

Run: `open index.html`
Expected: "Соцмережі" section with 4 large link buttons (Instagram, YouTube, TikTok, Telegram), each with a gold SVG icon, opening in a new tab. 4 columns on desktop, 2 on mobile. Console clean.

- [ ] **Step 5: Commit**

```bash
git add index.html styles.css script.js
git commit -m "feat: social links section from SITE_DATA"
```

---

## Task 9: Contacts section (info + action buttons + map)

**Files:**
- Modify: `index.html`, `styles.css`, `script.js`

- [ ] **Step 1: Add contacts markup after social in `index.html`**

```html
<section class="section section--alt" id="contacts">
  <div class="container">
    <p class="section__eyebrow">Контакти</p>
    <h2 class="section__title">Звʼяжіться з нами</h2>
    <div class="contacts">
      <div class="contacts__info">
        <ul class="contacts__list" id="contacts-list"></ul>
        <div class="contacts__actions" id="contacts-actions"></div>
      </div>
      <div class="contacts__map">
        <iframe id="contacts-map" title="Мапа" loading="lazy" referrerpolicy="no-referrer-when-downgrade"></iframe>
      </div>
    </div>
  </div>
</section>
```

- [ ] **Step 2: Add contacts styles to `styles.css`**

```css
.contacts{display:grid;grid-template-columns:1fr 1fr;gap:40px;align-items:start}
.contacts__list{list-style:none;margin-bottom:24px}
.contacts__list li{padding:10px 0;border-bottom:1px solid var(--border);display:flex;justify-content:space-between;gap:16px}
.contacts__list .k{color:var(--muted-2);font-size:14px}
.contacts__list .v{font-weight:600}
.contacts__actions{display:flex;gap:12px;flex-wrap:wrap}
.contacts__map iframe{width:100%;height:340px;border:0;border-radius:var(--radius);filter:grayscale(.2) contrast(1.05)}
@media(max-width:860px){.contacts{grid-template-columns:1fr}}
```

- [ ] **Step 3: Add contacts render to `script.js`**

```js
function renderContacts(){
  const c = SITE_DATA.contacts;
  const telHref = 'tel:' + c.phone.replace(/[^+\d]/g,'');
  document.getElementById('contacts-list').innerHTML = `
    <li><span class="k">Телефон</span><span class="v">${c.phone}</span></li>
    <li><span class="k">Email</span><span class="v">${c.email}</span></li>
    <li><span class="k">Адреса</span><span class="v">${c.address}</span></li>
    <li><span class="k">Графік</span><span class="v">${c.hours}</span></li>`;
  document.getElementById('contacts-actions').innerHTML = `
    <a class="btn btn--primary" href="${c.telegramUrl}" target="_blank" rel="noopener">Написати в Telegram</a>
    <a class="btn btn--ghost" href="${telHref}">Зателефонувати</a>
    <a class="btn btn--ghost" href="mailto:${c.email}">Email</a>`;
  document.getElementById('contacts-map').src = c.mapEmbedUrl;
}
```

Add `renderContacts();` inside `DOMContentLoaded`.

- [ ] **Step 4: Verify in browser**

Run: `open index.html`
Expected: "Контакти" section (darker bg) with a list (phone/email/address/hours) and 3 action buttons (Telegram, Зателефонувати → `tel:`, Email → `mailto:`), plus an embedded Google map on the right (single column on mobile). Map loads from `mapEmbedUrl`. Console clean.

- [ ] **Step 5: Commit**

```bash
git add index.html styles.css script.js
git commit -m "feat: contacts section with actions and embedded map"
```

---

## Task 10: Footer

**Files:**
- Modify: `styles.css`, `script.js` (footer element already exists in `index.html`)

- [ ] **Step 1: Add footer styles to `styles.css`**

```css
.footer{background:var(--navy-2);border-top:1px solid var(--border);padding:36px 0}
.footer__inner{display:flex;justify-content:space-between;align-items:center;gap:20px;flex-wrap:wrap}
.footer__brand{font-family:var(--serif);color:var(--gold);font-size:18px}
.footer__links{display:flex;gap:20px;font-size:14px}
.footer__links a:hover{color:var(--gold)}
.footer__copy{color:var(--muted-2);font-size:13px;width:100%}
```

- [ ] **Step 2: Add footer render to `script.js`**

```js
function renderFooter(){
  const year = new Date().getFullYear();
  document.getElementById('footer').innerHTML = `
    <div class="container footer__inner">
      <div class="footer__brand">${SITE_DATA.brand.name}</div>
      <div class="footer__links">
        <a href="#about">Про нас</a>
        <a href="#services">Послуги</a>
        <a href="#faq">FAQ</a>
        <a href="#contacts">Контакти</a>
      </div>
      <div class="footer__copy">© ${year} ${SITE_DATA.brand.name}. Усі права захищено.</div>
    </div>`;
}
```

Add `renderFooter();` inside `DOMContentLoaded`.

- [ ] **Step 3: Verify in browser**

Run: `open index.html`
Expected: Footer with brand name, quick links, and dynamic-year copyright. Console clean.

- [ ] **Step 4: Commit**

```bash
git add styles.css script.js
git commit -m "feat: footer with quick links and copyright"
```

---

## Task 11: Interactions — mobile menu, back-to-top, scroll reveal

**Files:**
- Modify: `styles.css`, `script.js`

- [ ] **Step 1: Add to-top + reveal styles to `styles.css`**

```css
.to-top{position:fixed;right:22px;bottom:22px;width:46px;height:46px;border-radius:50%;border:none;background:var(--gold);color:var(--navy);font-size:20px;cursor:pointer;opacity:0;pointer-events:none;transition:opacity .25s ease,transform .25s ease;z-index:60;box-shadow:0 8px 20px rgba(0,0,0,.3)}
.to-top.is-visible{opacity:1;pointer-events:auto}
.to-top:hover{transform:translateY(-2px)}
.reveal{opacity:0;transform:translateY(24px);transition:opacity .5s ease,transform .5s ease}
.reveal.is-in{opacity:1;transform:none}
@media(prefers-reduced-motion:reduce){.reveal{opacity:1;transform:none;transition:none}}
```

- [ ] **Step 2: Add interaction handlers to `script.js`**

```js
function initInteractions(){
  // Mobile burger menu
  const burger = document.getElementById('nav-burger');
  const menu = document.getElementById('nav-menu');
  burger.addEventListener('click', () => {
    const open = menu.classList.toggle('is-open');
    burger.setAttribute('aria-expanded', open ? 'true' : 'false');
  });
  menu.querySelectorAll('a').forEach(a => a.addEventListener('click', () => {
    menu.classList.remove('is-open');
    burger.setAttribute('aria-expanded','false');
  }));

  // Back-to-top button
  const toTop = document.getElementById('to-top');
  window.addEventListener('scroll', () => {
    toTop.classList.toggle('is-visible', window.scrollY > 500);
  });
  toTop.addEventListener('click', () => window.scrollTo({ top:0, behavior:'smooth' }));

  // Scroll reveal
  const obs = new IntersectionObserver((entries) => {
    entries.forEach(e => { if(e.isIntersecting){ e.target.classList.add('is-in'); obs.unobserve(e.target); } });
  }, { threshold:0.12 });
  document.querySelectorAll('.section, .hero').forEach(el => { el.classList.add('reveal'); obs.observe(el); });
}
```

Add `initInteractions();` as the LAST call inside `DOMContentLoaded` (after all render functions, so observed elements and the rendered grids already exist).

- [ ] **Step 3: Verify in browser**

Run: `open index.html`
Expected:
- Resize <860px → burger toggles the menu open/closed; tapping a link closes it and scrolls to section.
- Scroll down >500px → gold round "↑" appears bottom-right; clicking scrolls smoothly to top.
- Sections fade/slide in as they enter the viewport.
- Console clean. Test on desktop and narrow widths.

- [ ] **Step 4: Commit**

```bash
git add styles.css script.js
git commit -m "feat: mobile menu, back-to-top button, scroll-reveal animations"
```

---

## Task 12: README + final review

**Files:**
- Create: `README.md`

- [ ] **Step 1: Create `README.md`**

```markdown
# Сайт адвокатського об'єднання

Односторінковий статичний сайт (HTML + CSS + ванільний JS). Без збірки та бекенду.

## Як редагувати вміст
Усі тексти, контакти й посилання — у файлі `script.js`, в об'єкті `SITE_DATA` зверху.
Редагуйте лише значення (у лапках). Розмітку чіпати не потрібно.

- `brand` — назва, слоган, заголовок і підзаголовок hero.
- `stats` — три показники під hero.
- `about` — текст про об'єднання та картки переваг.
- `team` — адвокати (ім'я, посада, біо, `photo` — URL фото або "" для монограми).
- `services` — послуги (`icon` — одне зі значень: gavel, home, doc, briefcase, family, shield, scale, clock, lock).
- `testimonials` — відгуки.
- `faq` — питання й відповіді.
- `social` — посилання на Instagram / YouTube / TikTok / Telegram.
- `contacts` — телефон, email, адреса, графік, посилання Telegram і `mapEmbedUrl`.

### Як вставити Google-мапу
Google Maps → знайдіть адресу → «Поділитися» → «Вбудувати карту» → скопіюйте значення `src` з тегу `<iframe>` і вставте у `contacts.mapEmbedUrl`.

## Як переглянути
Відкрийте `index.html` подвійним кліком у браузері.

## Як опублікувати
Перетягніть папку на app.netlify.com/drop або завантажте у GitHub Pages — жодного збирання не потрібно.
```

- [ ] **Step 2: Self-review pass — open the full page and walk every section top to bottom**

Run: `open index.html`
Checklist (all must hold):
- Nav sticky, logo + menu + CTA; burger works <860px.
- Hero: tagline, title, subtitle, 2 buttons, 3 stats.
- Про нас: text + 4 advantage cards.
- Команда: 3 member cards with monograms.
- Послуги: 6 cards.
- Відгуки: 3 quotes.
- FAQ: accordion, one open at a time.
- Соцмережі: 4 link buttons (new tab).
- Контакти: info list + 3 action buttons + embedded map.
- Footer: brand, links, current-year copyright.
- Back-to-top appears on scroll; sections reveal on scroll.
- No console errors. Check at 375px, 768px, 1280px widths.

- [ ] **Step 3: Commit**

```bash
git add README.md
git commit -m "docs: add README with editing and deployment instructions"
```

---

## Self-Review (plan vs spec)

- **Spec coverage:** Static no-build approach (Task 1) ✓; Класична довіра theme (Task 1) ✓; `SITE_DATA` config (Task 1, all sections) ✓; nav + burger (Tasks 1, 11) ✓; hero + stats (Task 2) ✓; Про нас + advantages (Task 3) ✓; **Команда** cards (Task 4) ✓; Послуги (Task 5) ✓; Відгуки (Task 6) ✓; FAQ accordion (Task 7) ✓; Соцмережі buttons (Task 8) ✓; Контакти + actions + **map** (Task 9) ✓; Footer (Task 10) ✓; **back-to-top** + scroll reveal + responsive (Task 11) ✓; README (Task 12) ✓.
- **Placeholder scan:** No "TODO"/"handle later" steps; every code step contains full code.
- **Type consistency:** `SITE_DATA` keys defined in Task 1 (`brand.heroTitle`, `brand.heroSubtitle`, `stats`, `about.advantages`, `team[].photo`, `services[].icon`, `testimonials`, `faq`, `social`, `contacts.mapEmbedUrl`, `contacts.telegramUrl`) match every render function. `icon()` + `ICONS` defined in Task 3, extended in Task 8, used consistently. Element IDs in markup match `getElementById` calls.
