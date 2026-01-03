
## Metryka Projektu: Timo (Nazwa Robocza)
*   **Typ projektu:** Web Application (SaaS) / Single Page Application (SPA).
*   **Model:** Client-Side Rendering (CSR) – przetwarzanie grafiki w przeglądarce.
*   **Platformy:** Desktop (Chrome/Safari/Edge), Tablet, Mobile (Mobile First Design).
*   **Główny stos technologiczny (sugerowany):** React/Vue, TypeScript, Konva.js/Fabric.js (Canvas engine), Zustand (State Management), Tailwind CSS.

---

## 1. Executive Summary
**Timo** to narzędzie graficzne typu "Infinite Canvas", które rewolucjonizuje proces tworzenia screenshotów do App Store i Google Play.
Głównym problemem, który rozwiązuje aplikacja, jest zarządzanie tłumaczeniami na grafikach. Zamiast ręcznie edytować setki plików, **Timo** centralizuje treści w jednym pliku JSON. Użytkownik projektuje raz, tłumaczy JSON przy pomocy AI (np. ChatGPT), a aplikacja automatycznie generuje warianty graficzne dla każdego języka i urządzenia.

---

## 2. Product Backlog (Epics, Stories, Tasks)

### EPIC 1: Core Canvas Engine (Silnik Graficzny i Nawigacja)
**Cel Biznesowy:** Zapewnienie płynnego, profesjonalnego środowiska pracy ("Infinite Board"), które działa równie dobrze na 27-calowym monitorze, jak i na iPadzie.

#### STORY 1.1: Przestrzeń Robocza i Gesty (Touch & Desktop)
**Opis:** Użytkownik musi mieć możliwość swobodnego poruszania się po nieskończonej tablicy.
**Acceptance Criteria (AC):**
1.  Aplikacja ładuje pełnoekranowy element Canvas.
2.  **Desktop:** Spacja + Drag przesuwa widok (Pan). Scroll myszy przybliża/oddala (Zoom).
3.  **Mobile:** Przesunięcie dwoma palcami przesuwa widok. Gest "szczypania" (Pinch) skaluje widok.
4.  Wyświetlanie siatki (Grid), która pomaga w orientacji.

*   **Task 1.1.1 [FRONT] Setup Sceny (Konva/Fabric):** Inicjalizacja biblioteki graficznej. Obsługa responsywności (`window.resize`), aby canvas zawsze zajmował 100% viewportu.
*   **Task 1.1.2 [LOGIC] Implementacja Zoom & Pan Matrix:** Oprogramowanie wektorów przesunięcia i skali. Zoom musi centrować się na kursorze/palcu użytkownika.
*   **Task 1.1.3 [UI] Infinite Grid:** Renderowanie linii siatki z logiką LOD (Level of Detail) – siatka zagęszcza się lub rzednie w zależności od poziomu zoomu.

---

### EPIC 2: Object Manipulation & Layout (Edycja Wizualna)
**Cel Biznesowy:** Umożliwienie tworzenia zaawansowanych kompozycji (warstwy, makiety telefonów) bez znajomości profesjonalnych narzędzi graficznych.

#### STORY 2.1: Import Mediów i Zarządzanie Warstwami (Z-Index)
**Opis:** Użytkownik chce nałożyć ramkę telefonu na screenshota. Musi kontrolować kolejność wyświetlania.
**AC:**
1.  Drag & Drop plików (PNG/JPG) bezpośrednio na tablicę.
2.  Menu kontekstowe z opcjami: "Przesuń na wierzch", "Przesuń pod spód", "Warstwa w górę/dół".
3.  Obiekty zachowują swoją pozycję w stosie po ponownym załadowaniu projektu.

*   **Task 2.1.1 [FRONT] Drag & Drop Handler:** Przechwycenie eventu `drop`, odczyt `File API`, konwersja na obiekt obrazu i dodanie do Canvasu.
*   **Task 2.1.2 [LOGIC] Layer Management API:** Implementacja metod zmieniających indeks tablicy obiektów w renderze.

#### STORY 2.2: Transformacje: Skalowanie vs Rozciąganie
**Opis:** Użytkownik chce skalować zdjęcia proporcjonalnie, ale tła chce móc rozciągać dowolnie.
**AC:**
1.  Domyślne skalowanie za rogi (Transformers) zachowuje proporcje obrazu.
2.  Przytrzymanie klawisza (np. Shift) lub dedykowany tryb "Stretch" pozwala na niezależną zmianę szerokości i wysokości.
3.  Możliwość rotacji elementów.

*   **Task 2.2.1 [FRONT] Implementacja Transformerów:** Wizualizacja uchwytów (anchors) wokół aktywnego obiektu.
*   **Task 2.2.2 [LOGIC] Aspect Ratio Toggle:** Logika pozwalająca na wyłączenie blokady proporcji dla konkretnych obiektów.

#### STORY 2.3: Globalne Stylowanie i Tekst
**Opis:** Użytkownik chce sformatować tekst oraz zmienić tło dla całej serii ekranów jednym klikiem.
**AC:**
1.  **Tekst:** Wybór czcionki, pogrubienie, pochylenie, kolor, wielkość.
2.  **Globalne Tło:** Zmiana koloru tła (Color Picker) w jednym ekranie daje możliwość zastosowania tego koloru do wszystkich zdefiniowanych ekranów w projekcie.

*   **Task 2.3.1 [UI] Text Editor Toolbar:** Pływający panel (lub pasek nad klawiaturą na mobile) z opcjami formatowania.
*   **Task 2.3.2 [LOGIC] Global Property Propagation:** Funkcja, która znajduje wszystkie obiekty typu "Background" i aktualizuje ich kolor na podstawie wyboru użytkownika.

---

### EPIC 3: JSON Localization Hub (Serce Systemu - USP)
**Cel Biznesowy:** Automatyzacja lokalizacji. Umożliwienie współpracy z AI w celu błyskawicznego tłumaczenia.

#### STORY 3.1: Panel JSON i Dwukierunkowe Wiązanie (Two-way Binding)
**Opis:** Użytkownik chce widzieć i edytować teksty w panelu bocznym. Zmiany muszą być widoczne natychmiast na grafice.
**AC:**
1.  Przycisk w UI "Pokaż/Ukryj JSON".
2.  Panel (Drawer) zawiera edytor kodu z aktualnymi tekstami.
3.  Edycja tekstu na Canvasie -> Aktualizuje JSON.
4.  Edycja JSONa -> Aktualizuje tekst na Canvasie.
5.  Każdy tekst ma unikalne ID.

*   **Task 3.1.1 [ARCH] State Management Architecture:** Zaprojektowanie store'a (np. w Zustand), który oddziela warstwę graficzną od warstwy danych tekstowych.
*   **Task 3.1.2 [UI] JSON Editor Component:** Implementacja edytora (np. Monaco Editor lub prosty `textarea` z walidacją) w wysuwanym panelu bocznym.
*   **Task 3.1.3 [LOGIC] Reactive Subscription:** Mechanizm nasłuchujący zmian w modelu danych i wymuszający przerysowanie konkretnych węzłów tekstowych na Canvasie.

#### STORY 3.2: Workflow Tłumaczeń AI i Przełącznik Języków
**Opis:** Użytkownik chce skopiować JSON, przetłumaczyć go w AI i wgrać z powrotem jako nowy język.
**AC:**
1.  Przycisk "Kopiuj JSON" (formatuje dane do schowka).
2.  Możliwość wklejenia JSONa z nowym kluczem językowym (np. `es-ES`).
3.  Globalny przełącznik języka (Language Switcher) w UI.
4.  Zmiana języka podmienia treści we wszystkich ramkach jednocześnie.

*   **Task 3.2.1 [LOGIC] JSON Import & Validation:** Parser sprawdzający, czy wklejony JSON ma poprawną strukturę. Obsługa błędów (np. "Błąd składni w linii 5").
*   **Task 3.2.2 [UX] Language Switcher Logic:** Funkcja iterująca po wszystkich obiektach tekstowych i podmieniająca ich właściwość `text` na podstawie wybranego klucza językowego ze słownika.

---

### EPIC 4: Store Intelligence & Smart Templates (Wiedza Domenowa)
**Cel Biznesowy:** Zgodność plików z wymaganiami sklepów. Przyspieszenie pracy poprzez inteligentne skalowanie.

#### STORY 4.1: Szablony Rozdzielczości (Device Frames)
**Opis:** Użytkownik wybiera urządzenie z listy, a aplikacja tworzy ramkę o idealnych wymiarach.
**AC:**
1.  Baza predefiniowanych wymiarów: iPhone 6.5", 5.5", iPad Pro, Android Phone/Tablet.
2.  Ramki działają jak maski (Clipping) – to co wystaje, jest ucinane.

*   **Task 4.1.1 [CONFIG] Resolution Database:** Plik stałych z wymiarami zgodnymi z aktualnymi wytycznymi Apple i Google.
*   **Task 4.1.2 [FRONT] Clipping Groups:** Implementacja grup z funkcją przycinania (clipPath), reprezentujących pojedynczy screenshot.

#### STORY 4.2: Inteligentne Klonowanie Serii (Smart Clone)
**Opis:** Użytkownik stworzył serię 5 ekranów na iPhone. Chce je skopiować na iPada.
**AC:**
1.  Funkcja "Duplikuj serię na [Inne Urządzenie]".
2.  Aplikacja tworzy nowe ramki, kopiuje grafikę i teksty.
3.  **Kluczowe:** Skopiowane teksty zachowują te same ID co oryginały. Dzięki temu tłumaczenie w JSON działa na obie wersje urządzeń jednocześnie!
4.  Elementy są pozycjonowane relatywnie (np. środek ekranu pozostaje środkiem).

*   **Task 4.2.1 [LOGIC] Deep Clone with Ref Preservation:** Algorytm klonujący obiekty graficzne, ale zachowujący powiązania logiczne (Text IDs) z modelem danych.
*   **Task 4.2.2 [LOGIC] Relative Positioning Layout:** Przeliczanie pozycji X/Y elementów na podstawie procentowych wartości względem nowej szerokości/wysokości ramki.

---

### EPIC 5: Export & Finalization (Dostarczenie Wartości)
**Cel Biznesowy:** Użytkownik otrzymuje gotową paczkę plików do uploadu.

#### STORY 5.1: Masowy Eksport (Batch Generation)
**Opis:** Pobranie wszystkich wersji językowych i rozmiarowych jednym kliknięciem.
**AC:**
1.  Aplikacja generuje plik ZIP.
2.  Struktura folderów: `/Język/Urządzenie/screenshot.png`.
3.  Proces odbywa się w tle z paskiem postępu.

*   **Task 5.1.1 [FRONT] Render Loop:** Skrypt automatyzujący proces: Ustaw Język A -> Renderuj do Blob -> Ustaw Język B -> Renderuj do Blob.
*   **Task 5.1.2 [LIB] ZIP Packaging:** Wykorzystanie biblioteki `JSZip` do spakowania wygenerowanych blobów i wymuszenie pobierania pliku.

---

## 3. Wytyczne UI/UX (Mobile First)

1.  **Interfejs Kontekstowy:** Unikaj stałych pasków bocznych zajmujących miejsce na małych ekranach. Menu edycji powinno pojawiać się jako "Bottom Sheet" (panel wysuwany z dołu) po kliknięciu w element.
2.  **Obsługa "Fat Finger":** Elementy interaktywne (uchwyty skalowania, przyciski zamykania) muszą mieć minimalny obszar dotyku 44x44px.
3.  **JSON Drawer:** Panel z kodem na urządzeniach mobilnych powinien zajmować 90-100% ekranu, aby edycja była wygodna, z łatwym przyciskiem "X" do powrotu do widoku graficznego.
4.  **Tryb Focus:** Podczas przesuwania tablicy (panning), interfejs UI powinien stać się półprzezroczysty (opacity 0.3), aby nie zasłaniać projektu.

---

## 4. Struktura Danych JSON (Przykład Implementacji)

```json
{
  "projectSettings": {
    "version": "1.0",
    "globalBackgroundColor": "#FFFFFF"
  },
  "translations": {
    "en-US": {
      "id_headline_1": "Fastest App",
      "id_sub_1": "Download now"
    },
    "pl-PL": {
      "id_headline_1": "Najszybsza Apka",
      "id_sub_1": "Pobierz teraz"
    },
    "de-DE": {
      "id_headline_1": "Schnellste App",
      "id_sub_1": "Jetzt herunterladen"
    }
  },
  "canvasObjects": [
    // Obiekty graficzne trzymają tylko ID tekstu, nie sam tekst
    {
      "type": "text",
      "x": 100,
      "y": 200,
      "textId": "id_headline_1", 
      "style": { "fontSize": 40, "color": "black" }
    }
  ]
}
```


## 5. Rekomendacje Architektoniczne (Eksperckie)
Wydajność: Przy dużej liczbie ekranów (np. 5 języków x 3 rozmiary x 5 ekranów = 75 artboardów) renderowanie wszystkiego w DOM zabije przeglądarkę. Zastosuj Canvas Virtualization – renderuj dokładnie tylko to, co jest w widoku (viewport), reszta może być uproszczona lub ukryta.
Serverless Export (Opcjonalnie): Jeśli generowanie ZIP po stronie klienta będzie zbyt wolne na słabych telefonach, rozważ przeniesienie renderingu (Headless Browser) do funkcji Serverless (AWS Lambda / Vercel), która zwróci gotowy link do ZIPa.
Font Fallbacks: Przy tłumaczeniach na języki azjatyckie lub arabskie, wybrane czcionki mogą nie mieć znaków. System musi automatycznie wykrywać brak znaków i proponować font zastępczy (fallback font), aby uniknąć "prostokątów" zamiast liter.