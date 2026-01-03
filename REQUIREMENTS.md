Oto profesjonalnie przygotowany dokument requirements.md, napisany z perspektywy Architekta Systemowego oraz Product Ownera, z uwzględnieniem najlepszych praktyk UI/UX.

requirements.md
Metryka Projektu

Typ projektu: Web Application (SaaS) - Timo (Nazwa Projektu)

Platforma: Przeglądarka internetowa (RWD: Desktop, Tablet, Mobile)

Główny cel: Narzędzie typu "Infinite Canvas" do projektowania, lokalizacji i generowania zrzutów ekranu (screenshots) dla Apple App Store i Google Play Store.

Architektura: Client-side heavy (SPA/PWA), Mobile First Approach.

1. Executive Summary

Celem projektu jest stworzenie platformy graficznej, która łączy swobodę narzędzi takich jak Miro/Figma z precyzyjnymi wymaganiami sklepów mobilnych. Kluczowym wyróżnikiem (USP) jest "JSON Localization Hub" – moduł pozwalający na zarządzanie treściami wszystkich ekranów poprzez jeden plik JSON, co umożliwia błyskawiczne tłumaczenie przy użyciu zewnętrznych narzędzi AI (np. ChatGPT) i automatyczną generację wariantów językowych.

2. Funkcjonalności Kluczowe (Core Features)
2.1. Infinite Canvas (Nieskończona Tablica)

FR-01.01: Aplikacja oferuje nieskończoną przestrzeń roboczą, po której użytkownik może się poruszać (panning) i przybliżać/oddalać widok (zooming).

FR-01.02: Obsługa gestów dotykowych (pinch-to-zoom, two-finger pan) dla urządzeń mobilnych oraz skrótów klawiszowych (Spacja+Drag, Ctrl+Scroll) dla desktopu.

FR-01.03: Siatka pomocnicza (Grid) i inteligentne linie przyciągania (Smart Guides) do wyrównywania elementów.

2.2. JSON Localization Hub (Serce Aplikacji)

To unikalna funkcjonalność zarządzania treścią.

FR-02.01: Panel JSON (Side Drawer): Wysuwany panel boczny zawierający edytor tekstu (code editor) z aktualnym stanem tekstów na projekcie.

FR-02.02: Dwukierunkowe wiązanie danych (Two-way binding):

Edycja tekstu na Canvasie 
→
→
 aktualizuje JSON.

Edycja JSONa 
→
→
 aktualizuje teksty na Canvasie.

FR-02.03: Struktura JSON: Plik obsługuje klucze języków (np. en-US, pl-PL, de-DE). Każdy element tekstowy ma unikalne ID.

FR-02.04: Workflow tłumaczenia AI:

Użytkownik kopiuje JSON (np. wersję angielską).

Wkleja do AI z promptem: "Przetłumacz wartości na hiszpański i niemiecki, zachowując strukturę".

Wkleja wynikowy JSON do aplikacji.

FR-02.05: Przełącznik języków (Language Switcher): Globalny przełącznik w UI, który natychmiast podmienia teksty na wszystkich screenshotach na wybrany język z załadowanego JSONa.

2.3. Edytor Graficzny i Drag & Drop

FR-03.01: Import mediów: Przeciąganie i upuszczanie (Drag & Drop) plików PNG/JPG/WEBP bezpośrednio na tablicę.

FR-03.02: Zarządzanie warstwami (Z-Index):

Możliwość ustalania kolejności elementów: "Przesuń na wierzch", "Przesuń pod spód".

Kluczowe dla układania: Tło 
→
→
 Ramka Telefonu (Mockup) 
→
→
 Screenshot aplikacji 
→
→
 Tekst.

FR-03.03: Manipulacja obiektami:

Skalowanie (zachowanie proporcji przytrzymując Shift).

Rozciąganie (Stretching) – niezależna zmiana szerokości/wysokości.

Rotacja (obracanie elementów o dowolny kąt).

FR-03.04: Edytor Tekstu:

Wybór czcionki (integracja z Google Fonts).

Style: Bold, Italic, Underline.

Kolor tekstu (Color Picker z obsługą Alpha).

Wyrównanie (lewo, środek, prawo).

FR-03.05: Tło globalne i lokalne: Możliwość zmiany koloru tła dla pojedynczego ekranu lub zastosowanie jednego stylu dla całej serii.

2.4. Store Intelligence & Templates (Logika Sklepów)

FR-04.01: Biblioteka Rozdzielczości: Predefiniowane ramki (Frames) zgodne z aktualnymi wymaganiami Apple (6.5", 5.5", 12.9" iPad) i Google (Phone, 7" Tablet, 10" Tablet).

FR-04.02: Walidacja: Ostrzeżenie, jeśli elementy wychodzą poza bezpieczny obszar (Safe Area).

2.5. Smart Scaling & Replication (Kopiowanie Serii)

FR-05.01: Master Series Clone: Możliwość stworzenia idealnej serii dla np. iPhone 14 Pro Max, a następnie opcja "Duplikuj i Dostosuj do iPada Pro".

FR-05.02: Inteligentne dopasowanie: Przy zmianie rozmiaru ramki (np. z telefonu na tablet), tło się rozszerza, a elementy centralne pozostają wyśrodkowane (zakotwiczenie relatywne - Relative Anchoring).

FR-05.03: Zachowanie ID tekstów: Skopiowane ekrany zachowują te same ID tekstów, dzięki czemu zmiana w JSON zmienia tekst zarówno na wersji telefonowej, jak i tabletowej jednocześnie.

2.6. Eksport

FR-06.01: Masowy Eksport: Przycisk "Generuj Wszystko".

FR-06.02: Struktura plików: Aplikacja pobiera plik .zip z uporządkowaną strukturą folderów:

/en-US/iPhone-6.5/screen1.png

/pl-PL/iPad-12.9/screen1.png

3. UI/UX Design Guidelines (Mobile First & RWD)

Jako ekspert UI/UX zalecam następujące podejście do interfejsu:

3.1. Koncepcja "Floating Tools" (Mobile)

Na urządzeniach mobilnych przestrzeń jest cenna. Zamiast stałych pasków bocznych (jak w Figma Desktop), zastosuj:

Contextual Bottom Sheet: Po kliknięciu w element (np. tekst), od dołu wysuwa się panel z opcjami edycji tego konkretnego elementu.

Hiding UI: Podczas przesuwania tablicy (panning), interfejs (przyciski) stają się półprzezroczyste lub znikają, aby maksymalizować widoczność projektu.

3.2. Panel JSON (Code View)

Musi posiadać walidację składni (syntax highlighting). Jeśli użytkownik zrobi literówkę w JSON (np. brak przecinka), panel musi podświetlić błąd na czerwono i zablokować zapis, aby nie "wysadzić" aplikacji.

Przycisk "Copy for AI" – jednym klikiem kopiuje strukturę gotową do promptowania.

3.3. Wizualizacja Warstw

Na mobile drag&drop warstw może być trudny.

Zalecenie: Dodać proste przyciski "Layer Up" / "Layer Down" w podręcznym menu, zamiast wymuszać skomplikowane drzewo warstw.

4. Architektura Techniczna (Sugestie)

Frontend: React.js lub Vue.js (ze względu na reaktywność).

Canvas Engine: Konva.js lub Fabric.js. To biblioteki idealne do obsługi obiektów na canvasie, skalowania i eksportu do obrazu.

State Management: Stan aplikacji (pozycje obrazków, teksty) powinien być trzymany w jednym dużym obiekcie stanu, który łatwo zserializować do zapisu projektu.

Generowanie obrazów: Wszystko powinno dziać się po stronie klienta (Client-side generation) dla prywatności i szybkości, chyba że pliki są ogromne – wtedy Serverless Function.

5. Eksperckie Usprawnienia (Co można zrobić lepiej?)

Mockup Library: Zamiast kazać użytkownikowi wgrywać własne ramki telefonów (png), wbuduj bibliotekę "Device Frames" (iPhone, Pixel, Samsung), gdzie użytkownik tylko wrzuca screenshota, a ramka nakłada się automatycznie (maskowanie).

Auto-Translate API (Premium): W wersji podstawowej użytkownik kopiuje JSON do AI. W wersji "Pro" podpinasz API OpenAI/DeepL bezpośrednio w aplikacji – jeden klik "Tłumacz na 20 języków" robi to w tle.

Style Presets: Możliwość zdefiniowania "Brand Kit" (czcionki firmowe, paleta kolorów), aby jednym klikiem zmienić styl wszystkich screenshotów, gdy zmienia się branding.

Panoramic Backgrounds: Funkcja, która pozwala "rozlać" jedno tło na 3 kolejne screenshoty (efekt carouseli na Instagramie/App Store), automatycznie tnąc grafikę.

6. Propozycje Nazw Projektu

Oto 10 propozycji nazw, które są chwytliwe, nowoczesne i oddają charakter aplikacji:

ShotFlow (Nawiązuje do screenshotów i przepływu pracy)

PolyScreen (Poly = wiele języków/wersji + Screen)

AppCanvas.io (Proste, opisowe, nawiązuje do "nieskończonej tablicy")

SnapGlobal (Szybkie robienie zrzutów + globalny zasięg tłumaczeń)

Blueprint Localize (Podkreśla aspekt planowania i tłumaczenia)

StoreBoard (Tablica do zarządzania zasobami sklepowymi)

Transnap (Translation + Snapshots)

MockupGlot (Mockup + Polyglot - wielojęzyczny)

ScreenScale (Nacisk na skalowanie na różne urządzenia)

LingoShot (Język + Zrzut ekranu)

Instrukcja dla Developera

Powyższy plik traktuj jako "Living Document". Zacznij od implementacji Fabric.js/Konva.js z prostym dodawaniem zdjęć, a następnie dobuduj warstwę reaktywności JSON. Pamiętaj – najpierw funkcjonalność na Mobile, potem skalowanie na Desktop.