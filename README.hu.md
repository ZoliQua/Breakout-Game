# 🏓 Breakout

[English](README.md) · [Deutsch](README.de.md) · **Magyar**

Egy igényes, típusbiztos Breakout / Arkanoid-lite **React 18**, **Vite** és **TypeScript** alapokon — kézzel tervezett falak, többütéses és acéltéglák, bónuszok és többlabdás játék, éles, skálázható SVG-ként, játékmotor nélkül — billentyűzettel, egérrel és érintéssel.
Önállóan fut, **és** a **[Zoltan's Arcade](https://github.com/ZoliQua/Arcade-Game-Center)** tizenöt játékának egyike — [játssz vele a portálon](https://dulzoltan.hu/arcade/#/play/breakout).

[![CI](https://github.com/ZoliQua/Breakout-Game/actions/workflows/ci.yml/badge.svg)](https://github.com/ZoliQua/Breakout-Game/actions/workflows/ci.yml)
![license](https://img.shields.io/badge/license-MIT-green)
![node](https://img.shields.io/badge/node-%E2%89%A520-339933?logo=node.js&logoColor=white)
![react](https://img.shields.io/badge/React-18-61dafb?logo=react&logoColor=white)
![vite](https://img.shields.io/badge/Vite-5-646cff?logo=vite&logoColor=white)
![typescript](https://img.shields.io/badge/TypeScript-5-3178c6?logo=typescript&logoColor=white)

---

## 📑 Tartalomjegyzék

- [A Zoltan's Arcade család](#-a-zoltans-arcade-család)
- [Funkciók](#-funkciók)
- [Gyorsindítás](#-gyorsindítás)
- [Irányítás](#%EF%B8%8F-irányítás)
- [Téglák, szintek és bónuszok](#-téglák-szintek-és-bónuszok)
- [Pontozás és statisztika](#-pontozás-és-statisztika)
- [Automatikus mentés és folytatás](#-automatikus-mentés-és-folytatás)
- [Beágyazás az Arcade-Game-Centerbe](#-beágyazás-az-arcade-game-centerbe)
- [Hogyan működik](#-hogyan-működik)
- [Szkriptek](#-szkriptek)
- [Felépítés](#%EF%B8%8F-felépítés)
- [Közreműködés](#-közreműködés)
- [Licenc](#%EF%B8%8F-licenc)

## 🕹️ A Zoltan's Arcade család

Minden játék önálló repo, amely a portálba is beleillik. Közös a dizájnrendszerük, a pontozási modelljük és a mentésformátumuk, a beépített [`src/arcade/`](./src/arcade) készleten keresztül.

| Játék | Repo | Műfaj |
| --- | --- | --- |
| 🏠 **Arcade portál** | [Arcade-Game-Center](https://github.com/ZoliQua/Arcade-Game-Center) | Minden játék egy helyen, élő demók, statisztika, ranglista |
| 🧱 Tetris | [Tetris-Game](https://github.com/ZoliQua/Tetris-Game) | Arcade |
| 🐍 Snake | [Snake-Game](https://github.com/ZoliQua/Snake-Game) | Reflex |
| 🔢 2048 | [2048-Game](https://github.com/ZoliQua/2048-Game) | Kirakós |
| 🟡 Pac-Man | [Pac-Man-Game](https://github.com/ZoliQua/Pac-Man-Game) | Labirintus |
| ⚫ Gomoku | [Gomoku-Game](https://github.com/ZoliQua/Gomoku-Game) | Stratégia |
| 🦷 Tic-Tac-Toe | [Tic-Tac-Toe-Game](https://github.com/ZoliQua/Tic-Tac-Toe-Game) | Párbaj |
| 🏓 **Breakout** (ez a repo) | [Breakout-Game](https://github.com/ZoliQua/Breakout-Game) | Ütős |
| 👾 Space Invaders | [Space-Invaders-Game](https://github.com/ZoliQua/Space-Invaders-Game) | Lövöldözős |
| 🔴 Négy egy sorban | [Connect-Four-Game](https://github.com/ZoliQua/Connect-Four-Game) | Párbaj |
| 🔩 Nut Sort | [Nut-Sort-Game](https://github.com/ZoliQua/Nut-Sort-Game) | Rendezős |
| 🃏 Texas Hold'em | [Texas-Holdem-Game](https://github.com/ZoliQua/Texas-Holdem-Game) | Kártya |
| 💣 Minesweeper | [Minesweeper-Game](https://github.com/ZoliQua/Minesweeper-Game) | Logika |
| ♠️ Solitaire | [Solitaire-Game](https://github.com/ZoliQua/Solitaire-Game) | Kártya |
| 🎴 Ulti | [Ulti-Game](https://github.com/ZoliQua/Ulti-Game) | Kártya |
| 🂡 Snapszer | [Snapszer-Game](https://github.com/ZoliQua/Snapszer-Game) | Kártya |

## ✨ Funkciók

- 🧱 **Hat kézzel tervezett fal** (Klasszikus, Piramis, Sakktábla, Erőd, Űrlény, Bástya), amelyek végtelenül ismétlődnek, szintenként gyorsabb labdával
- 💪 **1, 2 és 3 ütéses téglák**, plusz **törhetetlen acél**, sor szerint színezve, sérüléskor látható repedésekkel
- 🎯 **Szögvezérlés az ütővel** — az ütő és a labda találkozási pontja szabja meg a pattanás szögét (legfeljebb 60°)
- 🎁 **Öt bónusz** — széles ütő, több labda, lassú labda, ragadós ütő és extra élet; az időzített hatások az oldalpanelen számolnak vissza
- 🧮 **Szilárd fizika** — részlépésekben, tengelyenként szétválasztott mozgás: a labda sosem megy át téglán vagy az ütőn, az oldalról érkező ütközések pedig helyesen verődnek vissza
- 🎲 **Determinisztikus** — fix 120 Hz-es szimuláció, a játékállapotban tárolt kezdőértékes véletlenszám-generátorral (azonos seed + azonos bemenet = azonos játék)
- 🖱️ **Billentyűzet, egér és érintés** — nyilak / A D, mozgasd a mutatót a pálya fölött, vagy húzd az ujjadat
- 📖 **Szabályok párbeszédablak** a pattanási szöget bemutató ábrával (megnyitáskor a játék szünetel)
- 🏁 **Idő előtti befejezés** — szándékosan lezárhatsz egy játékot, és a mostani pontszámod felkerül a ranglistára
- 📱 **Érintéses játék** — a pálya bármely pontján húzva mozog az ütő, koppintásra kilő a labda
- ⛶ **Teljes képernyő** gomb (CSS-es teljes képernyős mód iPhone-on)
- 🔊 **Szintetizált hangeffektek** (Web Audio, a beállításokban kapcsolható, megjegyzi)
- 💾 **Automatikus mentés és folytatás** — zárd be a lapot egy labdamenet közepén, és pontosan onnan folytathatod; a lap elrejtése szünetelteti a játékot
- 🏆 **Egységes arcade statisztika** — rekord, játékok száma, összidő, megosztva a portállal
- 🌗 **Sötét / világos téma**, minden arcade játékkal közösen (nyomd meg a **T** billentyűt)
- 🌍 **Három nyelv** — angol, magyar és német; váltás a felső sávban vagy a beállításokban (linkátadáshoz `?lang=en|hu|de` is); a számok a nyelv csoportosítását követik
- 🦷 **Fogas mód** — „Fogas mód” / „Dental mode” / „Zahnmodus” kapcsoló a beállításokban, alapból kikapcsolva (vagy `?dental=1`), az egész arcade-ban közös: az ütő fogkefe, a labda fogkrém golyó, a téglák pedig fogkő és lepedék, külön kefe-, krém- és szondajelekkel; csak megjelenés
- 🤖 **Figyelemfelkeltő demó** (`<BreakoutDemo />`) — a valódi motor önműködő ütővel, a portál nyitóoldalán
- 🧪 **Tesztelt** — motor-, autopilot- és mentésellenőrző egységtesztek, valamint komponenstesztek Vitest + Testing Library segítségével

## 🚀 Gyorsindítás

**Node ≥ 20** és npm kell hozzá.

```bash
git clone https://github.com/ZoliQua/Breakout-Game.git
cd Breakout-Game
npm install
npm run dev          # http://localhost:5173
```

Éles build: `npm run build && npm run preview`. A build relatív elérési utakat használ, így a `dist/` bármely almappából kiszolgálható.

## ⌨️ Irányítás

| Bemenet | Művelet |
| --- | --- |
| ⬅️ ➡️ / **A D** | Az ütő mozgatása |
| 🖱️ Egér / 👆 ujjhúzás a pályán | Az ütő követi a mutatót |
| **Szóköz** / **Enter** · kattintás · koppintás | Indítás · labda kilövése · folytatás |
| **P** / **Esc** | Szünet / folytatás |
| **R** | Újrakezdés |
| **T** | Sötét / világos téma váltása (önálló oldal) |

**Mobilon:** húzd az ujjad a pálya bármely pontján, és az ütő követi; koppints a pályára a labda kilövéséhez. A **Szabályok**, **Befejezés** és **Teljes képernyő** gombok a Játék panelen vannak. iPhone-on add hozzá az oldalt a Főképernyőhöz, hogy böngészősávok nélkül lásd.

Ha az ütő **közepével** találod el a labdát, szinte egyenesen felfelé megy; a **szélek** legfeljebb 60°-os szögben küldik el.

## 🧱 Téglák, szintek és bónuszok

| Tégla | Ütés | Megjegyzés |
| --- | --- | --- |
| Sima | 1 | A szín a sort mutatja (felül piros → alul ibolya) |
| Páncélozott | 2 | Vékony fehér keret; ütéskor elhalványul és megreped |
| Megerősített | 3 | Szaggatott fehér keret |
| Acél | ∞ | Szürke; sosem törik el, és nem számít bele a fal letisztításába |

Ha minden törhető téglát leszedtél, jár a szintbónusz, és jön a következő fal. A hatodik fal után a ciklus elölről kezdődik.
A labda alapsebessége **230 px/s + 18 px/s szintenként** (legfeljebb 410), és minden ütőérintés 1,5%-kal növeli, a szint alapsebességének 1,25-szereséig.

Minden eltört tégla **14%** eséllyel (kezdőértékes véletlenszám-generátor) kapszulát ejt — kapd el az ütővel:

| Kapszula | Bónusz | Hatás |
| --- | --- | --- |
| 🟦 **W** | Széles ütő | 68 → 104 px 15 másodpercig |
| 🟪 **M** | Több labda | Minden játékban lévő labda háromfelé válik (legfeljebb 8) |
| 🟩 **S** | Lassú labda | Minden labda 70%-os sebességgel 12 másodpercig |
| 🟧 **C** | Ragadós ütő | Az ütő 15 másodpercig elkapja a labdát; told ki, vagy 2,5 s után magától elenged |
| 🟥 **+** | Extra élet | +1 élet (legfeljebb 6) |

Ha az utolsó játékban lévő labdát elveszted, egy életet veszítesz; egy elveszett labda vagy egy új fal minden aktív bónuszt és hulló kapszulát töröl. **3 élettel** indulsz.

## 🎯 Pontozás és statisztika

| Esemény | Pont |
| --- | --- |
| Tégla eltörése az *r*. sorban (felül = 1) | sor értéke × az ütésszám × szint |
| Bónusz elkapása | 50 × szint |
| Fal letisztítása | 500 × szint |

A sorok értéke felülről: **80, 70, 60, 50, 40, 30, 20, 10, 10**. Példa: egy 2 ütéses tégla a legfelső sorban a 3. szinten 80 × 2 × 3 = **480** pont. Az acél nem ér semmit.

A Breakout pontvadászat: a szintek sosem fogynak el, így a játék csak az utolsó élet elvesztésekor ér véget (kimenetel: `over`), vagy ha te fejezed be a **Befejezés** gombbal (kimenetel: `finished`, a mostani pontszám kerül rangsorolásra).
Ha egy játék véget ér, a készlet pontosan egyszer rögzíti a `localStorage`-ban az `arcade.stats.breakout` kulcs alatt
(`best`, `plays`, `wins`, `totalScore`, `totalSeconds`, `lastScore`, `lastPlayedAt`).

## 💾 Automatikus mentés és folytatás

Amíg egy játék folyamatban van, a **teljes** játékállapot — a téglák hátralévő ütéseikkel, minden labda, az ütő, a hulló kapszulák, az aktív bónuszok időzítői, a szint, az életek, a pontszám és a véletlenszám-generátor seedje — nagyjából másodpercenként mentődik, valamint amikor az oldal elrejtődik vagy a játék megszűnik
(kulcs: `arcade.session.breakout`, verziózott, betöltéskor a `src/logic/persist.ts` szigorúan ellenőrzi). Újratöltéskor a **Üdv újra** kártya jelenik meg a szüneteltetett, visszaállított játékkal.
Az *Automatikusan mentve* jelvény melletti **Elvetés** eldobja a mentést. A befejezett játékok mentése automatikusan törlődik. A hangbeállítás külön, a `breakout.sound` kulcs alatt tárolódik.

## 🧩 Beágyazás az Arcade-Game-Centerbe

```tsx
import { BreakoutGame, BreakoutDemo } from '@arcade/breakout';

<BreakoutGame
  embedded                                   // az oldalcím elrejtése; a portál mutatja
  onGameOver={(score, info) => submitScore('breakout', score)}   // info.finished: a Befejezés gombbal idő előtt lezárva
  onStatusChange={({ status, score, seconds }) => updateHeader(status, seconds)}
/>
<BreakoutDemo active={cardIsVisible} />       // figyelemfelkeltő demó a nyitóoldalra
```

Mindkét komponens a `src/arcade/types.ts` közös `ArcadeGameProps` / `ArcadeDemoProps` szerződését követi.

## 🧠 Hogyan működik

Állapotgép: `idle → running ↔ paused → gameOver → (újrakezdés) → running`. Az adogatás, a labda elvesztése és a szint teljesítése
mind a `running` állapoton belül történik (a labda egyszerűen az ütőn pihen az indításig), így a játékidő és a készlet állapota egyszerű marad.

Egyetlen tiszta függvény, a `step(state, dtMs, input, events?)` birtokolja a szimulációt:

1. A bónuszok időzítőinek visszaszámlálása és az ütő mozgatása (billentyűzettel 470 px/s, a mutató követése 2400 px/s-ra korlátozva).
2. Az ütőn pihenő labdák vele mozognak; az indítás (vagy a ragadás időkorlátja) az ütő szögében elküldi őket.
3. Minden szabad labda **legfeljebb 2 px-es részlépésekben** mozog, **előbb x, majd y** szerint feloldva: ha az x mentén mozogva téglával fedi egymást, az oldalütközés (`vx` visszaverése), az y mentén felső/alsó ütközés (`vy` visszaverése). A téglák 38 × 14 px-esek, így semmi nem maradhat ki.
4. A lefelé haladó labda, amely az ütő felső lapjához ér, `rel × 60°`-kal pattan el a függőlegestől; a felső lap alatt lévő labda lecsúszik az oldalán.
5. A letört téglák a kezdőértékes Mulberry32 generátorból (`state.seed`) kapszulát sorsolhatnak; a kapszulák hullanak, és érintésre hatnak.
6. Nincs több törhető tégla → szintbónusz és következő fal; nincs több labda → életet (vagy a játékot) veszíted.

A komponens a `step`-et **fix 120 Hz-en** futtatja egy `requestAnimationFrame` akkumulátorból (a képkockák 50 ms-ra korlátozottak, így egy akadás sosem ugrasztja meg a labdát), és egyetlen mérvadó állapot-referenciát tart, amelyet megoszt a bemeneti kezelőkkel, hogy két képkocka között lenyomott szünet sose vesszen el. A pálya egy 480 × 360-as SVG `viewBox`, a tárolóhoz méretezve; minden szín az arcade CSS-tokenjeiből jön, így mindkét téma működik a stílusok újraolvasása nélkül.

A **demó autopilot** (`logic/autopilot.ts`) megjósolja, hol keresztezi a következő hulló labda az ütő vonalát (az útját az oldalfalakról visszahajtva), és billentyűzetsebességgel odairányít. A célzási hibája a játék seedjéből származik, így determinisztikus, mégis emberi: többnyire közel a középhez, néha egy tiszta mellényúlás — a demó végigmegy a játék végén, és magától újraindul.

## 🧪 Szkriptek

| Szkript | Cél |
| --- | --- |
| `npm run dev` | Vite fejlesztői szerver |
| `npm run build` | Típusellenőrzés (`tsc -b`) és éles build |
| `npm run preview` | Az éles build kiszolgálása |
| `npm run typecheck` | `tsc -b` |
| `npm run lint` | ESLint |
| `npm test` | Vitest (logikai és komponenstesztek) |
| `npm run check` | lint → typecheck → test → build (amit a CI futtat) |

## 🏗️ Felépítés

```
src/
  arcade/              beépített Zoltan's Arcade készlet — itt ne szerkeszd (lásd a README-jét)
  logic/               tiszta játékszabályok (React nélkül)
    types.ts constants.ts rng.ts
    levels.ts          a hat falelrendezés + buildBricks
    game.ts            createInitialState + step (fizika, téglák, bónuszok, életek, szintek)
    autopilot.ts       leérkezés-előrejelzés + demóvezérlő
    persist.ts         a mentett játékok szigorú futásidejű ellenőrzése
  hooks/               useFrameLoop (rAF), useBreakoutKeys, useSound (Web Audio)
  components/          Playfield (SVG, teljes + mini), PowerUpList, SoundToggle, palette
  demo/BreakoutDemo.tsx  figyelemfelkeltő demó a nyitóoldalra
  rules.tsx            a szabályok ablak tartalma és a pattanási ábra
  BreakoutGame.tsx     a beágyazható játék (ArcadeGameProps)
  App.tsx              önálló oldal: <ArcadeShell><BreakoutGame/></ArcadeShell>
  main.tsx             belépési pont; egyszer tölti be a közös arcade.css-t
  index.ts             csomagbelépő az Arcade-Game-Center számára
```

## 🤝 Közreműködés

Lásd a [CONTRIBUTING.md](./CONTRIBUTING.md), a [Magatartási kódex](./CODE_OF_CONDUCT.md) és a [SECURITY.md](./SECURITY.md) fájlt.
A közreműködők a [CONTRIBUTORS.md](./CONTRIBUTORS.md) fájlban szerepelnek; a változások a [CHANGELOG.md](./CHANGELOG.md) fájlban.

## ⚖️ Licenc

[MIT](./LICENSE) © 2026 Zoltán Dul

---

<p align="center">Szeretettel készítette Zoltán Dul · a <a href="https://github.com/ZoliQua/Arcade-Game-Center">Zoltan's Arcade</a> része</p>
