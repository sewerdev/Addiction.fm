# [Addiction.fm](https://sewerdev.github.io/Addiction.fm/)

[Русский](#русский)

<p align="center">
  <img src="https://github.com/user-attachments/assets/0e1c4752-30f1-4c75-a174-8396a07390bc" alt="image" width="100%">
</p>

**Mobile version:**

<p align="left">
  <img src="https://github.com/user-attachments/assets/72c2653e-4ca4-4e9e-8e8c-ba00ada297b9" alt="image" width="45%">
  <img src="https://github.com/user-attachments/assets/b9c78e26-c095-43c0-8128-979a68d58972" alt="image" width="45%">
</p>

**Generative radio. One button - sound built for your mental state.**

Addiction.fm is a browser-based generative audio app with no playlists, no accounts, and no ads. Pick a state - get a unique sound that never repeats.

---

## 🔥 What's new in 2.2

**The player bar stopped moving.** The break name now sits in a fixed-width plate that covers ~90% of the bank, with the full name in a tooltip. The 🎲 button keeps its slot reserved while the dice is switched on, so changing presets no longer nudges the sliders by a pixel, and long labels truncate instead of colliding. The whole row still holds together down to 633 px.

**Background playback got tougher.** The scheduler no longer trusts `document.hidden` - it measures how late its own ticks actually arrive and grows the lookahead to match real throttling. Coming back from a hard freeze ducks the master for half a second instead of firing every queued note at once, and an `AudioContext` state-change handler re-primes the grid when iOS wakes the context without telling anyone.

**The propeller has three speeds now.** DRIVE, FOCUS and CALM spin at their own rate and ease between them instead of snapping to a stop.

**Fixes**
- The ⏺ rec button no longer sticks on "⏹ stop" after a recording finishes - a local variable was shadowing the translation function, so the label-revert timer never got armed.
- The last used preset survives a reload; the page used to always come back to FOCUS.
- Decoded breaks release their base64 source: about a third of the bank's memory is no longer held for nothing.
- Dead code removed, and the player is now drawn by a single render pass instead of four functions writing the same nodes.

---

## 🔥 What's new in 2.1

[Addiction.fm is now hosted!](https://sewerdev.github.io/Addiction.fm/)

- **Background**: added deep “ocean floor” hum (brown noise, –6 dB/oct), seamless loop, 480 Hz filter.
- **Reliability**: scheduler looks 1.6 s ahead - tab switching won't break the audio stream.
- **Interface**: system language (ru/en/es) on first launch, long track names truncated with tooltip, dark theme with no white flash.
- **Fixes**: audio filter leak patched, held keys no longer duplicate commands, bank counter now shows the real number (268 breaks).

The biggest update since launch: the mobile interface was rebuilt from scratch, the drum bank grew five and a half times, and you can now run the radio without ever opening the tab.

### System-wide media controls

The radio is no longer trapped inside a browser tab. It shows up wherever a normal music player does, with its own artwork:

- **Android** - a card in the notification shade and on the lock screen: play/pause, track skipping, and a dedicated "another break" button.
- **Windows** - the Chrome and Edge media panel (the one behind the address-bar icon, which also feeds the system media controls), plus hardware media keys on keyboards and headsets.
- **iOS/macOS** - lock screen and Control Center.
- With a **sleep timer** armed, the system card draws a real countdown, so you can see what's left without unlocking the phone.

This turned out to be the fiddliest part of the release: the operating system has no idea Web Audio exists, so the card is carried by a separate inaudible `<audio>` element. Chrome and Edge only promote tracks longer than 5 seconds into the system controls, and a track of pure zeroes gets written off as "no audio at all" - so the carrier runs for 30 seconds and holds a square wave one least-significant bit tall: quieter than the noise floor of any signal chain, yet unmistakably real audio to the OS.

### 268 real breaks instead of 49

The drum bank went from 49 to **268 live loops** - real breaks, not synthesis. The large packs live in separate files and are only fetched once DRIVE is playing, so the page still opens instantly.

### Every visit sounds new

Track `#1` used to sound the same forever. Now the **bank is reshuffled on every visit**: the same track number today and tomorrow lands on completely different drums. Within a single session the numbering stays deterministic, so going back to `‹‹ #N ››` still returns the same track.

### Redesigned mobile player

The phone player was rebuilt: a sticky panel at the bottom edge, a CSS-grid layout instead of wrapping rows, thumb-sized tap targets, and dedicated breakpoints for narrow screens. Sliders, timer, and track controls no longer fall apart at 360 pixels wide.

### Small things you can see and hear

- **mp3 codec delay compensation** - decoders leave up to 25 ms of silence at the head of a file, which made the slicing land just barely late. The real start of the loop is now detected from the signal, so hits sit exactly on the grid.
- **The break name** heads the player in DRIVE and sits on the second line of the system card.
- **Custom icon** - the app artwork doubles as the tab favicon, the home-screen icon, and the system player cover.
- **Bank counter** in the About dialog shows how many breaks are loaded.
- **FOCUS was rewritten** around pure synthesis - the `genres.js` file from 1.1 is gone.

---

## Why this exists

Most music services require you to choose. Choosing is a distraction.
Addiction.fm removes the choice entirely: one button, one state, one stream of sound. Runs in the browser, no install required, no login needed.

---

## States

| State | Genre | For |
|-------|-------|-----|
| 🔴 **DRIVE** | Amen-break · Jungle · ~180 BPM | Energy, task focus, movement |
| 🔵 **FOCUS** | Deep ambient | Sub-bass and dark pads only - no drums |
| 🟢 **CALM** | Slow drone + rain | Relaxation, sleep, recovery |

---

## How it works

Sound is **generated in real time** via the Web Audio API - this is not streaming, not a playlist. Every press creates a unique combination:

- **DRIVE** - a break from a 268-loop bank sliced into 16th notes with DJ jumps, stutters and rolls, soft piano on top
- **FOCUS** - sub-bass synthesized in the chosen key, dark pads through a delay chain
- **CALM** - drone from detuned oscillators, filtered noise, slow bells

---

## Why Addiction.fm

- **Runs everywhere** - Chrome, Firefox, Safari, Android, iOS, Windows. Open and play.
- **Controlled from the OS** - Android shade, Windows media controls, lock screen, media keys
- **Works offline** - download the files and run locally from any folder
- **No accounts** - no tracking, no ads, no recommendation algorithms
- **Always different** - the generative engine creates a new version on every press
- **268 breaks** - the bank is reshuffled on every visit
- **Deterministic tracks** - the `‹‹ #N ››` counter locks a specific track by seed number within a session
- **WAV recording** - the ⏺️ rec button captures the track in real time and downloads it as `.wav`
- **Propeller** - spins at the tempo of the current state, coasts down on pause

---

## Files

| File | What's inside | Required |
|------|---------------|----------|
| `index.html` | the whole app: engine, interface, artwork | yes |
| `breaks.js` | base bank, 49 breaks | yes |
| `breaks2.js` | main pack, 219 breaks | optional, but far more fun |
| `breaks3.js` | pack top-up | optional |

Without `breaks2.js` and `breaks3.js` the radio runs on the base bank; without `breaks.js` it falls back to synthesized drums. You can delete them for fun, but the songs won't sound very good.

---

## Shortcuts

| Key | Action |
|-----|--------|
| `Space` | pause |
| `1` `2` `3` | state |
| `V` / `B` | previous / next track |
| `X` | another break - in DRIVE, with the 🎲 switch on |

---

Requirements: **modern browser** (Chrome 90+, Firefox 88+, Safari 15+). Nothing else.

---

## Tech stack

- **Web Audio API** - synthesis, slicing, mixing, real-time recording
- **Media Session API** - system media controls, artwork, sleep-timer countdown
- **OfflineAudioContext** - full track and stem rendering without artifacts
- **ScriptProcessorNode** - PCM capture for WAV export
- **Pure HTML/JS** - zero dependencies, zero frameworks, single file

---

## Author

Made by **Sewerbox** / [sewerdev](https://github.com/sewerdev)

Telegram: [@VestronVulture](https://t.me/VestronVulture)

*Addiction.fm is not a playlist. It's a state of mind.*

---

# Русский

# [Addiction.fm](http://addiction.fm/)

**[English Page](https://github.com/sewerdev/Addiction.fm)**

<img width="1918" height="889" alt="image" src="https://github.com/user-attachments/assets/34c4e750-e09f-4de1-b251-ba7719a08c4a" />

**Мобильная версия:**

<img width="280" height="600" alt="image" src="https://github.com/user-attachments/assets/61ead776-63ff-4483-bc96-e37a83de79f7" /><img width="280" height="600" alt="image" src="https://github.com/user-attachments/assets/255de5c5-1ffb-4a90-a7cd-198badc76b3a" />

**Генеративное радио. Одна кнопка - и звук создаётся под твоё состояние.**

[Addiction.fm](http://addiction.fm/) - это браузерное генеративное аудио-приложение без плейлистов, аккаунтов и рекламы. Выбери состояние - получи уникальный звук, который никогда не повторится.

---

## 🔥 Что нового в 2.2

**Плеер перестал ездить.** Имя брейка живёт в плашке фиксированной ширины - она закрывает ~90% банка, полное имя лежит в подсказке. Когда кубик включён, его слот остаётся занятым, поэтому смена пресета больше не сдвигает ползунки на пиксель, а длинные подписи обрезаются, а не наезжают друг на друга. Ряд целиком держится вплоть до 633 px.

**Фон стал жёстче.** Планировщик больше не верит флагу `document.hidden` - он меряет, насколько реально опаздывают его тики, и растягивает упреждение под фактический троттлинг. Возврат из жёсткой заморозки на полсекунды приглушает мастер вместо того, чтобы выстрелить всеми запланированными нотами разом, а `AudioContext` сам перепланировывает сетку, когда iOS разбудит контекст и никому об этом не скажет.

**У пропеллера три скорости.** РАЗГОН, ФОКУС и ПОКОЙ крутятся каждая в своём темпе и плавно перетекают друг в друга, а не обрываются щелчком.

**Исправления**
- Кнопка ⏺ rec больше не залипает на «⏹ стоп» после записи - локальная переменная затеняла функцию перевода, из-за чего таймер возврата надписи просто не ставился.
- Последний выбранный пресет переживает перезагрузку; раньше страница всегда возвращалась на ФОКУС.
- Расшифрованный брейк освобождает свою base64-строку: примерно треть банка больше не висит в памяти впустую.
- Мёртвый код убран, а плеер теперь рисуется одним проходом, а не четырьмя функциями, пишущими в одни и те же узлы.

---

## 🔥 Что нового в 2.1

[Addiction.fm теперь на хосте!](https://sewerdev.github.io/Addiction.fm/)

- **Фон**: добавлен глубокий гул «дна океана» (коричневый шум, –6 дБ/окт), бесшовная петля, фильтр 480 Гц.
- **Надёжность**: планировщик смотрит на 1,6 с вперёд - переключение вкладок не рвёт звук.
- **Интерфейс**: язык системы (ru/en/es) при первом запуске, длинные имена треков сокращаются с подсказкой, тёмная тема без белой вспышки.
- **Исправления**: утечка аудиофильтров, зажатые клавиши не дублируют команды, счётчик банка показывает реальное число (268 брейков).

Самое крупное обновление с момента запуска: переписан мобильный интерфейс, банк ударных вырос в пять с половиной раз, а управлять радио теперь можно, не открывая вкладку.

### Системный пункт управления

Радио больше не заперто во вкладке браузера. Оно появляется там же, где обычный музыкальный плеер, со своей обложкой:

- **Android** - карточка в шторке уведомлений и на экране блокировки: play/pause, переключение треков, отдельная кнопка «другой брейк».
- **Windows** - панель мультимедиа Chrome и Edge (та, что открывается по иконке в адресной строке и попадает в системный пункт управления), плюс аппаратные медиаклавиши на клавиатуре и наушниках.
- **iOS/macOS** - экран блокировки и Пункт управления.
- Если заведён **таймер сна**, в системной карточке рисуется реальный обратный отсчёт - видно, сколько осталось, не разблокируя телефон.

Технически это оказалось самой муторной частью: Web Audio для операционной системы не существует, поэтому карточку «несёт» отдельный неслышимый элемент `<audio>`. Chrome и Edge берут в системные контролы только дорожку длиннее 5 секунд, а на дорожке из чистых нулей система решает, что звука нет вообще - поэтому носитель длится 30 секунд и содержит меандр амплитудой в один младший бит: тише, чем шум любого тракта, но для системы это настоящий звук.

### 268 настоящих брейков вместо 49

Банк ударных вырос с 49 до **268 живых петель** - реальные брейки, а не синтез. Большие паки лежат отдельными файлами и подтягиваются только когда включён РАЗГОН, так что страница по-прежнему открывается мгновенно.

### Каждый заход - новый звук

Раньше трек `#1` всегда звучал одинаково. Теперь **порядок банка перетасовывается заново при каждом заходе на сайт**: тот же номер трека сегодня и завтра - совершенно разные ударные. Внутри одного сеанса номера остаются детерминированными, так что вернуться на `‹‹ #N ››` и услышать то же самое по-прежнему можно.

### Редизайн мобильного плеера

Плеер на телефоне собран заново: липкая панель у нижней кромки экрана, раскладка на CSS-сетке вместо переносов, крупные зоны нажатия под большой палец, отдельные брейкпойнты для узких экранов. Ползунки, таймер и переключение треков больше не разъезжаются на 360 пикселях ширины.

### Мелочи, которые слышно и видно

- **Компенсация задержки mp3-кодека** - декодер оставляет в начале файла до 25 мс тишины, из-за чего нарезка едва заметно запаздывала. Теперь реальное начало петли ищется по сигналу, и доли попадают ровно в сетку.
- **Имя брейка** - заголовок плеера в РАЗГОНЕ и вторая строка системной карточки.
- **Своя иконка** - обложка приложения работает как фавикон вкладки, иконка на домашнем экране и обложка в системном плеере.
- **Счётчик банка** в окне «О приложении» показывает, сколько брейков подгружено.
- **ФОКУС переписан** на чистый синтез - файл `genres.js` из версии 1.1 больше не нужен.

---

## Зачем это существует

Большинство музыкальных сервисов требуют выбора. Выбор - это отвлечение.
[Addiction.fm](https://sewerdev.github.io/Addiction.fm/) убирает выбор полностью: одна кнопка, одно состояние, один поток звука. Работает в браузере, не требует установки, не просит логин.

---

## Состояния

| Состояние | Жанр | Для чего |
|-----------|------|----------|
| 🔴 **РАЗГОН** | Amen-break · Jungle · ~180 BPM | Энергия, фокус на задаче, движение |
| 🔵 **ФОКУС** | Глубокий эмбиент | Только суббасс и тёмные пэды - никаких ударных |
| 🟢 **ПОКОЙ** | Медленный дрон + дождь | Расслабление, сон, восстановление |

---

## Как это работает

Звук **генерируется в реальном времени** через Web Audio API - это не стриминг и не плейлист. Каждое нажатие создаёт уникальную комбинацию:

- **РАЗГОН** - брейк из банка на 268 петель нарезается по 16-м долям с DJ-прыжками, статтерами и роллами, мягкое пианино поверх
- **ФОКУС** - суббасс синтезируется в заданной тональности, тёмные пэды через delay-цепочку
- **ПОКОЙ** - дрон из расстроенных осцилляторов, фильтрованный шум, медленные колокольчики

---

## Преимущества

- **Работает везде** - Chrome, Firefox, Safari, Android, iOS, Windows. Открыть и играть.
- **Управление из системы** - шторка Android, пункт управления Windows, экран блокировки, медиаклавиши
- **Без интернета** - скачай файлы и запусти локально из любой папки
- **Нет аккаунтов** - нет трекинга, нет рекламы, нет алгоритмов рекомендаций
- **Каждый раз разный** - генеративный движок создаёт новую версию при каждом нажатии
- **268 брейков** - банк перетасовывается при каждом заходе
- **Детерминированные треки** - счётчик `‹‹ #N ››` фиксирует конкретный трек по seed-номеру внутри сеанса
- **Запись в WAV** - кнопка ⏺️ rec захватывает трек в реальном времени и скачивает как `.wav`
- **Пропеллер** - крутится в темпе текущего состояния, плавно тормозит на паузе

---

## Файлы

| Файл | Что внутри | Обязателен |
|------|------------|------------|
| `index.html` | приложение целиком: движок, интерфейс, обложка | да |
| `breaks.js` | базовый банк, 49 брейков | да |
| `breaks2.js` | основной пак, 219 брейков | нет, но с ним интереснее |
| `breaks3.js` | добор пака | нет |

Без `breaks2.js` и `breaks3.js` радио работает на базовом банке, без `breaks.js` - на синтезированных ударных. Вы можете по приколу их удалить, но звучать песни будут не очень 

---

## Горячие клавиши

| Клавиша | Действие |
|---------|----------|
| `Space` | пауза |
| `1` `2` `3` | состояние |
| `V` / `B` | трек назад / вперёд |
| `X` | другой брейк - в РАЗГОНЕ, при включённом кубике 🎲 |

---

Требования: **современный браузер** (Chrome 90+, Firefox 88+, Safari 15+). Больше ничего.

---

## Технологии

- **Web Audio API** - синтез, нарезка, микширование, запись в реальном времени
- **Media Session API** - системный пункт управления, обложка, обратный отсчёт таймера
- **OfflineAudioContext** - рендер полных треков и стемов без артефактов
- **ScriptProcessorNode** - захват PCM для экспорта в WAV
- **Чистый HTML/JS** - ноль зависимостей, ноль фреймворков, один файл

---

## Автор

Сделано **Sewerbox** / [sewerdev](https://github.com/sewerdev)

Telegram: [@VestronVulture](https://t.me/VestronVulture)

---

*[Addiction.fm](http://addiction.fm/) - это не плейлист. Это состояние.*
