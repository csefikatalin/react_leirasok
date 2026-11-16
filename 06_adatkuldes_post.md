# Adatküldés Reactben – POST kérés a Fake Store API felé


A webáruházban egy űrlapon új terméket rögzítünk. A megadott adatokat `fetch()` segítségével elküldjük a `https://fakestoreapi.com/products` végpontra, majd a szerver válaszát felhasználva frissítjük a Reactben megjelenített terméklistát.

A program három rétegre oszlik:

1. **`Form` komponens:** beolvassa az adatokat, és kezeli az űrlap elküldését.
2. **`ProductContext`:** elérhetővé teszi az `addProduct` függvényt, és kezeli a terméklista állapotát.
3. **`productService`:** ténylegesen elküldi a HTTP-kérést a szervernek.

Az adatok útja:

```text
Felhasználó → Form → addProduct(product) a Contextben
                          ↓
                    postProduct(product) a service-ben
                          ↓
               HTTP POST /products → API
                          ↓
                   JSON-válasz (új termék)
                          ↓
               setProducts(...) → újrarenderelés
```

## 2. Milyen adatokat küldünk? – `NewProductType`

A már lekért termékek `ProductType` típusa tartalmaz `id` és `rating` mezőt is. Az új termék űrlapján azonban ezeket nem kérjük be.

```ts
export interface ProductType {
    id: number;
    title: string;
    price: number;
    description: string;
    category: string;
    image: string;
    rating: {
        rate: number;
        count: number;
    };
}

export type NewProductType = Omit<ProductType, "id" | "rating">;
```

Az `Omit` egy meglévő típusból hagy el tulajdonságokat. Így a `NewProductType` mezői: `title`, `price`, `description`, `category`, `image`.

> Fontos: a `NewProductType` az **elküldendő adatok típusa**, míg a `ProductType` a terméklista teljes elemeinek típusa. A két típus nem ugyanaz.

## 3. Az űrlap adatai React state-ben

A `Form` komponensben egy objektumban tároljuk a mezők értékeit:

```tsx
const [product, setProduct] = useState<NewProductType>({
    title: "",
    price: 10,
    description: "",
    category: "",
    image: ""
});
```

Az inputok **vezérelt mezők**: a `value` értéke a React state-ből származik, az `onChange` pedig módosítja azt.

```tsx
<input
    id="price"
    name="price"
    type="number"
    min="0"
    step="1"
    required
    value={product.price}
    onChange={handleChange}
/>
```

A `name="price"` megegyezik az objektum `price` tulajdonságával. Ez teszi lehetővé, hogy ugyanaz a függvény több mezőt is kezeljen.

### A közös `handleChange` függvény

```tsx
function handleChange(
    e: ChangeEvent<HTMLInputElement | HTMLTextAreaElement>
) {
    const { name, value, type } = e.target;

    setProduct({
        ...product,
        [name]: type === "number" ? Number(value) : value
    });
}
```

- `e.target` az éppen módosított mező.
- `name` az objektum módosítandó tulajdonságának neve.
- `[name]` **számított tulajdonságnév**: például `name === "title"` esetén a `title` mezőt módosítja.
- `...product` megtartja a többi mező korábbi értékét.
- `Number(value)` azért kell, mert a HTML input `value` értéke még `type="number"` esetén is szöveg.

## 4. Az űrlap elküldése – `handleSubmit`

Az űrlapban:

```tsx
<form onSubmit={handleSubmit}>
    {/* beviteli mezők */}
    <button type="submit">Termék hozzáadása</button>
</form>
```

A függvény a megadott kódban:

```tsx
function handleSubmit(e: FormEvent<HTMLFormElement>) {
    e.preventDefault();
    addProduct(product);/*contextben definiált függvény*/
    setProduct({
        title: "",
        price: 10,
        description: "",
        category: "",
        image: ""
    });
}
```

**Lépések:**

1. Az `onSubmit` meghívja a `handleSubmit` függvényt.
2. Az `e.preventDefault()` megakadályozza a böngésző alapértelmezett űrlapelküldését, így az oldal nem töltődik újra.
3. Az `addProduct(product)` átadja az adatokat a Contextnek.
4. A `setProduct(...)` alaphelyzetbe állítja a mezőket.

**Figyelem:** a jelenlegi kódban az űrlap **azonnal kiürül**, még azelőtt, hogy kiderülne, sikeres volt-e a hálózati kérés. Ez nem szintaktikai hiba, de sikertelen küldésnél a felhasználó elveszítheti a beírt adatokat. Később érdemes úgy módosítani, hogy csak sikeres válasz után ürüljön ki.

## 5. A Context szerepe

A `ProductContextType` meghatározza, milyen adatokat és függvényeket érhetnek el a komponensek:

```tsx
interface ProductContextType {
    products: ProductType[];
    load: boolean;
    error: string | null;
    addProduct: (product: NewProductType) => void;
}
```

A `Form` ezt a függvényt a Contextből kapja:

```tsx
const { addProduct } = useProductContext();
```

Így az űrlapnak nem kell tudnia, milyen URL-re és milyen HTTP-beállításokkal küldjük az adatokat.

### Az `addProduct` függvény

```tsx
function addProduct(product: NewProductType) {
    postProduct(product)
        .then(newProduct => {
            setProducts(previousProducts => [
                ...previousProducts,
                newProduct
            ]);
        })
        .catch(() => {
            setError("Nem sikerült hozzáadni a terméket!");
        });
}
```

A `postProduct(product)` Promise-t ad vissza:

- **Siker esetén** a `.then(newProduct => ...)` megkapja a szerver válaszát, és hozzáfűzi a terméklistához.
- **Hiba esetén** a `.catch(...)` beállít egy hibaüzenetet.

A lista frissítése:

```tsx
setProducts(previousProducts => [
    ...previousProducts,
    newProduct
]);
```

A `previousProducts` a korábbi lista, a `...previousProducts` annak elemeit másolja az új tömbbe, a `newProduct` pedig az új elem. A React az állapotváltozás után újrarendereli a listát megjelenítő komponenseket.

## 6. A service réteg – a tényleges POST kérés

A `productService.ts` fájlban:

```ts
import type { NewProductType, ProductType } from "../types/ProductType";

const API_URL = "https://fakestoreapi.com/products";

export function postProduct(product: NewProductType): Promise<ProductType> {
    return fetch(API_URL, {
        method: "POST",
        headers: {
            "Content-Type": "application/json"
        },
        body: JSON.stringify(product)
    }).then(response => {
        if (!response.ok) {
            throw new Error("Failed to create product");
        }
        return response.json();
    });
}
```

### A `fetch` második paramétere

A `fetch(API_URL, { ... })` második paramétere egy beállításokat tartalmazó objektum:

| Kódrészlet | Feladata |
|---|---|
| `method: "POST"` | Jelzi, hogy adatot küldünk a szervernek. |
| `headers` | A kérés fejlécadatait tartalmazza. |
| `"Content-Type": "application/json"` | Közli, hogy a kérés törzse JSON-formátumú. |
| `body: JSON.stringify(product)` | A JavaScript-objektumot JSON-szöveggé alakítja, és a kérés törzsébe teszi. |

### Miért kell `JSON.stringify`?

JavaScript-objektum:

```ts
const product = {
    title: "Fejhallgató",
    price: 25,
    description: "Vezeték nélküli fejhallgató",
    category: "electronics",
    image: "https://example.com/headphones.jpg"
};
```

A `JSON.stringify(product)` eredménye egy JSON-formátumú **szöveg**, amely elküldhető a HTTP-kérés törzsében.

### Mit csinál a `.then(response => ...)`?

```ts
.then(response => {
    if (!response.ok) {
        throw new Error("Failed to create product");
    }
    return response.json();
});
```

A `response.ok` általában a 200–299 közötti HTTP-státuszkódok esetén igaz. A `fetch` önmagában nem dob hibát például egy HTTP 404 vagy 500 válasz miatt, ezért ellenőrizzük külön.

A `response.json()` a válasz JSON-tartalmát JavaScript-adattá alakítja; ez szintén aszinkron művelet.

## 7. GET és POST összehasonlítása

| Szempont | GET | POST |
|---|---|---|
| Cél | Adatok lekérése | Adatok elküldése / létrehozás kezdeményezése |
| Példa | `getProducts()` | `postProduct(product)` |
| `method` | Alapértelmezésben `GET` | Kifejezetten `"POST"` |
| `body` | Ebben a példában nincs | `JSON.stringify(product)` |
| Eredmény | Termékek tömbje | A szerver válasza az új termékről |


## 8. Fontos megjegyzések a konkrét projekthez

### A Fake Store API nem ment tartósan

A Fake Store API létrehozási végpontja **szimulálja** a termék létrehozását. A válasz alapján a React állapotába felvett termék megjelenhet, de az oldal újratöltésekor az újra lekért terméklistában nem feltétlenül lesz ott.

### Eltérés a `ProductType` és a POST-válasz között

A service-ben a visszatérési típus `Promise<ProductType>`, de a küldött `NewProductType` nem tartalmaz `rating` mezőt. A teszt-API válasza sem garantáltan tartalmaz minden olyan mezőt, amelyet a `ProductType` megkövetel. **A TypeScript-típusmegadás nem alakítja át és nem egészíti ki a tényleges JSON-választ.**

Ezért a `setProducts` után a termékkártyákban csak olyan mezőket szabad feltétel nélkül használni, amelyek ténylegesen rendelkezésre állnak; a hiányzó `rating` adatokat külön kell kezelni, vagy a típust kell a valós válaszhoz igazítani.

### Hibaüzenet és űrlapürítés

A Context `error` állapota jelenleg a betöltési és a mentési hibákat is tárolja. Ha a terméklistát megjelenítő komponens `if (error)` esetén kizárólag hibaüzenetet jelenít meg, akkor egy sikertelen POST után a teljes lista eltűnhet a képernyőről. Később célszerű külön `saveError` állapotot használni.


