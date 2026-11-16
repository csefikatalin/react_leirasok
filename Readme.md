# React + TypeScript tananyag 2026–27

## Mi az a React?

A React egy nyílt forráskódú JavaScript-könyvtár, amelyet a Meta (korábban Facebook) fejlesztett ki. Elsősorban interaktív webalkalmazások felhasználói felületének (UI) létrehozására használjuk.

A React legfontosabb jellemzői:

- **Komponensalapú felépítés:** A felhasználói felületet kisebb, önálló, újrafelhasználható részekre, úgynevezett komponensekre bontjuk. Ilyen lehet például egy fejléc, egy menü, egy termékkártya vagy egy űrlap. A modern React-alkalmazásokban jellemzően függvénykomponenseket használunk.

  A függvénykomponensek felépítése:
  - A megjelenítendő JSX-kódot általában a `return` utasítás után írjuk.
  - A visszaadott JSX-nek egyetlen gyökéreleme lehet. Ha több elemet szeretnénk egymás mellé helyezni, közös szülőelembe vagy fragmentbe (`<>...</>`) foglaljuk őket.

  ```tsx
  function Komponens() {
    return (
      <div>Komponens</div>
    );
  }

  export default Komponens;
  ```

- **JSX:** A React-komponensekben HTML-hez hasonló szintaxist használhatunk, amely lehetővé teszi a felület és a JavaScript-logika együttes leírását.
  - A JSX a JavaScript szintaxisának XML-szerű kiterjesztése.
  - JavaScript- vagy TypeScript-kifejezéseket kapcsos zárójelek (`{}`) közé írhatunk.
  - Minden JSX-elemet le kell zárni. Az üres elemeket önlezáró formában írjuk, például: `<img />`.
- **Deklaratív működés:** Azt írjuk le, hogyan nézzen ki a felület egy adott állapotban. A React gondoskodik a szükséges változtatások megjelenítéséről.
- **Állapotkezelés (state):** A komponensek adatokat tárolhatnak, amelyek változásakor a React frissíti a felület megfelelő részeit.
- **Egyirányú adatáramlás:** Az adatokat a szülőkomponensek tulajdonságokon (`props`) keresztül adhatják át a gyermekkomponenseknek.

A React önmagában nem teljes alkalmazásfejlesztési keretrendszer. Gyakran más eszközökkel együtt használjuk:

| Technológia | Feladata |
|---|---|
| **React** | Felhasználói felület létrehozása és frissítése |
| **TypeScript** | Típusellenőrzés, a programozási hibák egy részének megelőzése |
| **Vite** | Fejlesztői szerver és az alkalmazás összeállítása |
| **React Router** | Navigáció és útvonalkezelés |
| **Tailwind CSS** | A felhasználói felület megjelenésének kialakítása |

## Alapfogalmak: komponens, JSX, props, state

**Komponens (component):** A felhasználói felület egy önálló, újrafelhasználható része. Reactben általában egy függvény, amely JSX-elemeket ad vissza.

**JSX:** JavaScriptben vagy TypeScriptben használható, HTML-hez hasonló szintaxis. Segítségével írjuk le, hogy mit jelenítsen meg egy komponens.

**Props:** A szülőkomponenstől a gyermekkomponensnek átadott, csak olvasható adatok. A gyermek a kapott propsokat nem módosítja közvetlenül.

**State (állapot):** Egy komponens időben változó adata. Az állapot módosításakor a React újrarendereli az érintett komponenst.

**useState hook:** Beépített React-hook, amellyel függvénykomponensben állapotot hozhatunk létre. Visszaadja az aktuális értéket és az értéket módosító függvényt.

### Példák

#### Képgaléria

**Részletes leírás:** [Komponens, props, state](01_componens_jsx_tsx.md)

- Repo: [Képgaléria – GitHub](https://github.com/csefikatalin/kepgaleria_2026_2.git)
- Pages: [Képgaléria – élő oldal](https://csefikatalin.github.io/kepgaleria_2026_2/)

#### Tic-Tac-Toe

- Repo: [Tic-Tac-Toe – GitHub](https://github.com/csefikatalin/tictactoe_2026_state_dinamikus_palyameret_urlapelemmel.git)
- Pages: [Tic-Tac-Toe – élő oldal](https://csefikatalin.github.io/tictactoe_2026_state_dinamikus_palyameret_urlapelemmel/)

## React-alkalmazás publikálása

**Részletes leírás:** [A gh-pages csomag használata GitHub Pages-publikáláshoz](02_react_project_publikalasa_gh-pages.md)

## Context használata

### A Context fogalma

A Context a React egyik beépített eszköze, amely lehetővé teszi, hogy több komponens közösen használjon adatokat és függvényeket, anélkül hogy azokat minden komponensnek külön-külön, `props` segítségével kellene továbbadni.

A Reactben az adatokat általában a szülőkomponensektől a gyermekkomponensek felé, `props` segítségével adjuk át.

Ha egy adatot sok, egymásba ágyazott komponensen keresztül kell továbbadni, az nehezen áttekinthetővé válhat. Ezt nevezzük *prop drillingnek*.

A Context segítségével a komponensek közvetlenül hozzáférhetnek a közösen biztosított adatokhoz.

**Részletes leírás:** [Context használata a TODO-alkalmazáson keresztül](03_context_alapok_todo.md)

### Példák

#### TODO-alkalmazás

- Repo: [TODO – GitHub](https://github.com/csefikatalin/todo_context_provider.git)
- Pages: [TODO – élő oldal](https://csefikatalin.github.io/todo_context_provider/)

#### Könyvesbolt – adminfelület

- Repo: [Könyvesbolt – GitHub](https://github.com/csefikatalin/konyvesbolt_admin_contexttel.git)

#### Tilitoli példa Contexttel

- Repo: [Tilitoli – GitHub](https://github.com/csefikatalin/B_tilitoli.git)

## Űrlapok használata – űrlap készítése a Könyvesbolt alkalmazáshoz

A Reactben az űrlapok segítségével adatokat kérhetünk be a felhasználótól, például regisztrációhoz, kereséshez vagy új termék rögzítéséhez.

A hagyományos HTML-űrlapokban a beviteli mezők értékét alapvetően a böngésző kezeli. Reactben azonban az űrlapmezők értékét a komponens állapotában (state) tároljuk.

### Kontrollált komponens (Controlled Component)

Kontrollált komponensnek nevezzük azt az űrlapelemet, amelynek értékét a React állapota határozza meg.

Ez azt jelenti, hogy:
- A beviteli mező aktuális értékét a useState segítségével tároljuk.
- A mező value attribútumát összekapcsoljuk az állapottal.
- Az onChange esemény hatására frissítjük az állapotot.
- Az állapot változásakor a React frissíti a beviteli mező megjelenített értékét.

**Részletes leírás:** [Context használata a Könyvesbolt alkalmazáson keresztül](03_konyvesbolt_admin_contexttel.md)

Új könyv rögzítése űrlappal:

[Űrlap készítése – leírás](04_urlapok_konyvesbolt.md)

## Szűrés és rendezés készítése a Könyvesbolt alkalmazáshoz

### Rendezés készítése

[Rendezés elkészítése – leírás](05_rendezes_konyvesbolt.md)

### Szűrés lehetősége

[Szűrés elkészítése – leírás](05_szures_konyvesbolt.md)

## Adatok elérése API-végpontról – Fake Store webáruház

A modern webalkalmazások gyakran külső adatforrásokkal kommunikálnak. Például egy webáruház lekérheti a termékeket egy szerverről, új terméket rögzíthet, módosíthatja vagy törölheti a meglévő adatokat.

A React-alkalmazásokban erre használhatjuk a JavaScript beépített Fetch API-ját.

### REST API és végpontok

A REST (Representational State Transfer) egy architekturális stílus, amely meghatározza, hogyan kommunikálhatnak egymással a kliens- és szerveralkalmazások HTTP-n keresztül.

A REST alapelveit követő alkalmazásokat **RESTful** alkalmazásoknak nevezzük.

A végpont (endpoint) egy meghatározott URL, amelyen keresztül a kliens elérheti a szerver valamely erőforrását, például a termékeket.

Például a Fake Store API esetében:

https://fakestoreapi.com/products

### Aszinkron hívások

A szerverrel történő kommunikáció időt vesz igénybe. A válasz megérkezése függhet a hálózat sebességétől és a szerver terheltségétől.

Ezért a Fetch API aszinkron módon működik: a programnak nem kell megállnia és várakoznia a szerver válaszára, hanem közben más feladatokat is végezhet.

A **fetch()** egy **Promise** objektumot ad vissza, amely egy később rendelkezésre álló eredményt képvisel.

A Promise kezelésére használhatjuk:
- .then() – Akkor fut le, amikor az előző aszinkron művelet sikeresen teljesült.
- .catch() – Az aszinkron művelet során keletkező hibák kezelésére szolgál.
- .finally() – A művelet lezárásakor fut le, akár sikeres volt, akár hibával végződött.

### Fetch használata Reactben - useEffect

Reactben az adatlekérést gyakran a useEffect() hookban indítjuk el, az eredményt pedig a useState() segítségével tároljuk.

Nagyobb alkalmazásokban célszerű a szerverkommunikációt külön service rétegbe szervezni, és az adatokat szükség esetén Context segítségével megosztani a komponensek között.

Repo: [Fake Store webáruház – GitHub](https://github.com/csefikatalin/fakestore_react_typescript_tailwind)

### Adatok lekérése

**Részletes leírás:** [Fake Store webáruház és az API-végpontok – fetch GET](06_fakestore_webaruhaz_fetch_get.md)

### Adatok küldése

**Részletes leírás:** [Fake Store webáruház és az API-végpontok – fetch POST](06_adatkuldes_post.md)

### Single-page alkalmazás készítése React Router segítségével

**Részletes leírás:** [React Router használata és publikálás GitHub Pagesre](07_react_router_es_publikalas.md)
