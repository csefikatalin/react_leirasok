# React + Typescript tananyag 2026-27

## Mi az a React? 

A React egy nyílt forráskódú JavaScript-könyvtár, amelyet a Meta (korábban Facebook) fejlesztett ki. Elsősorban interaktív webalkalmazások felhasználói felületének (UI) létrehozására használjuk.

A React legfontosabb jellemzői:

- **Komponensalapú felépítés:** A felhasználói felületet kisebb, önálló, újrafelhasználható részekre, úgynevezett komponensekre bontjuk. Ilyen lehet például egy fejléc, egy menü, egy termékkártya vagy egy űrlap.     A legújabb react verziókban függvény komponenseket használunk. Felépítése:  
    - Minden komponens JSX kódját a return kulcsszó után kell írni
    - Egyetlen tagbe a visszatérési érték, minden további taget és kódot ezek között kell elhelyezni. 

    ```
        function Komponens() {
        return (
            <div>Komponens</div>
        )
        }
        export default Komponens
```
- **JSX:** A React komponensekben HTML-hez hasonló szintaxist használhatunk, amely lehetővé teszi a felület és a JavaScript-logika együttes leírását. 
    - XML + javascript együtt. 
    - A JS vagy TS kódokat {} közé kell írni. 
    - Minden tagnek kell zárótag! <PARATLANTAG /> 
- **Deklaratív működés:** Azt írjuk le, hogyan nézzen ki a felület egy adott állapotban. A React gondoskodik a szükséges változtatások megjelenítéséről.
- **Állapotkezelés (state):** A komponensek adatokat tárolhatnak, amelyek változásakor a React automatikusan frissíti a felület megfelelő részeit.

- **Egyirányú adatáramlás:** Az adatokat a szülőkomponensek tulajdonságokon (props) keresztül adhatják át a gyermekkomponenseknek.

A React önmagában nem teljes alkalmazásfejlesztési keretrendszer. Gyakran más eszközökkel együtt használjuk:

- **React**	        Felhasználói felület létrehozása és frissítése
- **TypeScript**	Típusellenőrzés, a programozási hibák egy részének megelőzése
- **Vite**	        Fejlesztői szerver és az alkalmazás összeállítása
- **React Router**	Navigáció és útvonalkezelés
- **Tailwind CSS**	A felhasználói felület megjelenésének kialakítása

## Alapfogalmak: Komponens, JSX, Props, State

**Komponens (component):** A felhasználói felület egy önálló, újrafelhasználható része. Reactben általában egy függvény, amely JSX-elemeket ad vissza.

**JSX:** JavaScriptben vagy TypeScriptben használható, HTML-hez hasonló szintaxis. Segítségével írjuk le, hogy mit jelenítsen meg egy komponens.

**Props:** A szülőkomponenstől a gyermekkomponensnek átadott, csak olvasható adatok. A gyermek a kapott propsokat nem módosítja közvetlenül.

**State (állapot):** Egy komponens időben változó, belső adata. Az állapot módosításakor a React újrarendereli az érintett komponenst.

**useState hook:** Beépített React hook, amellyel funkcionális komponensben állapotot hozhatunk létre. Visszaadja az aktuális értéket és az értéket módosító függvényt.



### Példák: 

#### Képgaléria

**Részletes leírás:** <a href="01_componens_jsx_tsx.md"> Komponens, props, state </a>

repo: https://github.com/csefikatalin/kepgaleria_2026_2.git
pages: https://csefikatalin.github.io/kepgaleria_2026_2/

#### Tic-Tac-Toe

repo: https://github.com/csefikatalin/tictactoe_2026_state_dinamikus_palyameret_urlapelemmel.git
pages: https://csefikatalin.github.io/tictactoe_2026_state_dinamikus_palyameret_urlapelemmel/

## React oldal publikálása

**Részletes leírás:** <a href="02_react_project_publikalasa_gh-pages.md"> A gh-pages csomag használata pages linkekhez</a>

## Context használata

### Context fogalma

A Context a React egyik beépített eszköze, amely lehetővé teszi, hogy több komponens közösen használjon adatokat és függvényeket, anélkül hogy azokat minden komponensnek külön-külön, props segítségével kellene továbbadni.

A Reactben az adatokat általában a szülőkomponensektől a gyermekkomponensek felé, props segítségével adjuk át.

Ha egy adatot sok, egymásba ágyazott komponensnek kell továbbadni, az nehezen áttekinthetővé válhat. Ezt nevezzük prop drillingnek.

A Context segítségével a komponensek közvetlenül hozzáférhetnek a közösen biztosított adatokhoz.

**Részletes leírás:** <a href="03_context_alapok_todo.md"> Context használata a TODO alkalmazáson keresztül</a>

Példák: 

#### Todo alkalmazás

repo: https://github.com/csefikatalin/todo_context_provider.git
pages: https://csefikatalin.github.io/todo_context_provider/

#### Könyvesbolt - admin felület

repo:https://github.com/csefikatalin/konyvesbolt_admin_contexttel.git


#### Tilitoli példa context-tel

repo: https://github.com/csefikatalin/B_tilitoli.git

## Űrlapok használata - űrlap készítése a Könyvesbolt alkalmazáshoz

**Részletes leírás:** <a href="03_konyvesbolt_admin_contexttel.md"> Context használata a Könyvesbolt  alkalmazáson keresztül</a>

Új könyv rögzítése űrlappal
<a href="04_urlapok_konyvesbolt.md">Űrlap készítése leírás</a>

## Szűrés és rendezés készítése a Könyvesbotl alkalmazáshoz

### Rendezés készítése
<a href="05_rendezes_konyvesbolt.md">Rendezés elkészítése leírás</a>

### Szűrés lehetősége

<a href="05_szures_konyvesbolt.md">Szűrés elkészítése leírás</a>


## Adatok elérése végpontról - Fakestore webáruház

repo: https://github.com/csefikatalin/fakestore_react_typescript_tailwind

### Adatok lekérése

**Részletes leírás:** <a href="06_fakestore_webaruhaz_fetch_get.md"> FakeStore webáruház és az API végpontok - fetch GET </a>

### Adatok küldése

**Részletes leírás:** <a href="06_adatkuldes_post.md"> FakeStore webáruház és az API végpontok - fetch POST </a>

### Single Page alkalmazás készítése react router segítségével

**Részletes leírás:** <a href="07_react_router_es_publikalas.md"> react router használata és publikálás githubra</a>








