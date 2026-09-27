# Basket UAE — grocery price comparison super app

Android app (Kotlin + Jetpack Compose) that puts UAE online grocery stores side by side:
**Carrefour, Spinneys, talabat mart, noon Minutes, LuLu, Kibsons and Union Coop**.

> **Prices are sample data.** The catalogue (62 products, 11 categories) uses realistic UAE base
> prices with generated per-store variation so the app can be built and tested. See *Going live* below.

## Features

- **Categories**: Fruits & Veg, Dairy & Eggs, Bakery, Meat & Fish, Pantry, Beverages, Snacks, Frozen, Household, Personal Care, Baby
- **Search** by product, brand or category
- **Emirate filter**: all 7 emirates; stores that don't deliver there drop out of the comparison
- **Price comparison**: every product shows the cheapest store (green *CHEAPEST* tag), the other stores' prices, promos (was/now) and how much you save vs the dearest store
- **Store filter**: switch stores in or out, with delivery fee, minimum order and ETA for each
- **Sort** by relevance, lowest price or biggest saving
- **Basket**: split by store, flags items that are cheaper elsewhere, minimum-order and delivery warnings
  - *Get the cheapest price for every item*: moves each item to its cheapest store
  - *Buy everything from one store*: whole-basket total per store, delivery included
- **Checkout**: review the order in-app, then *Continue in <store> app*. The store's app opens if it's installed, otherwise its website does, and the list is copied to the clipboard so you can paste it into the store's search. There's also a *Get the app* link to Play Store.

## Build

- **Android Studio**: open the folder and run the `app` configuration (JDK 17, Android SDK 35).
- **Command line**: `./gradlew assembleDebug` → `app/build/outputs/apk/debug/app-debug.apk`
- **GitHub Actions**: every push to `main` builds the APK (`.github/workflows/build-apk.yml`). Download it from the run's *Artifacts*.

## Project layout

```
app/src/main/java/ae/grocery/superapp/
  data/Models.kt           Store, Product, Offer, basket types
  data/Catalog.kt          Sample stores + catalogue (replace for live data)
  data/PriceRepository.kt  Interface to plug a live price feed into
  GroceryViewModel.kt      Filters, comparison, basket and basket optimiser
  StoreLauncher.kt         Hand-off to store app / website / Play Store
  ui/                      Compose screens (Shop, Basket, sheets, theme)
```

## Going live

1. Implement `PriceRepository` against your own backend and pass it to `GroceryViewModel`.
2. Get prices through retailer partner or affiliate feeds where you can. Check each retailer's terms before scraping.
3. If a retailer gives you cart or product deep links, add them in `StoreLauncher.checkout` so items land straight in their cart.
4. Check the store package ids in `Catalog.kt` and in `AndroidManifest.xml` `<queries>` against Play Store.
5. Replace the sample delivery fees, minimum orders and coverage with the real values.
