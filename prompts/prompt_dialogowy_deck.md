# PROMPT: Interaktywna prezentacja — głos, płeć i emocje postaci filmowych

---

## Jak używać tego promptu

1. Otwórz Claude (claude.ai) lub Gemini.
2. Skopiuj cały blok poniżej jako wiadomość do modelu.
3. Dołącz do tej samej wiadomości plik `bundle.json` wygenerowany w notatniku `analiza_dialogow_03_synteza.ipynb`.
4. Wyślij i poczekaj na wygenerowanie pliku HTML.
5. Zapisz odpowiedź jako `[tytuł_filmu]_prezentacja.html` i otwórz w przeglądarce.

> Jeśli model nie dokończy pliku, poproś: „Kontynuuj od miejsca, gdzie się zatrzymałeś."

---

## PROMPT (kopiuj całość poniżej)

---

```
Jesteś ekspertem w zakresie Social Network Analysis, humanistyki cyfrowej oraz tworzenia interaktywnych wizualizacji danych w czystym HTML/CSS/JavaScript z użyciem biblioteki D3.js v7.

Użytkownik dostarczy Ci plik bundle.json zawierający dane z analizy dialogowej scenariusza filmowego. Struktura pliku:
- top_speakers: lista postaci z liczbą słów, płcią i procentem łącznej mowy
- speech_by_gender: łączna mowa wg płci (K/M/?)
- emotion_arc: łuk emocjonalny — lista par (scena, wygładzona walencja)
- emotion_by_gender: rozkład 6 emocji (radość/smutek/złość/strach/zaskoczenie/neutralna) dla K i M (w procentach)
- network.nodes: węzły sieci z atrybutami (gender, word_count, scene_count, dominant_emotion, avg_valence)
- network.edges: krawędzie z wagą (weight = liczba wspólnych scen)

Twoim zadaniem jest wygenerowanie JEDNEGO kompletnego pliku HTML — interaktywnej prezentacji złożonej z sześciu slajdów.

---

## KROK 1 — IDENTYFIKACJA DZIEŁA I ESTETYKI

Na podstawie nazw węzłów w network.nodes zidentyfikuj film. Zastosuj te same zasady estetyczne co dla dashboardu SNA (przykłady poniżej — traktuj jako wzorzec myślenia, nie listę zamkniętą):

| Dzieło | Estetyka |
|---|---|
| Pulp Fiction | Noir, brudna żółć i czerwień krwi, czarne tło, Bebas Neue + Courier |
| Fight Club | Industrialny brud, czerwień i beż na czarnym, Bold i Mono |
| The Godfather | Włoski chiaroscuro, sepia i ciemne złoto, Cinzel |
| Blade Runner | Cyberpunkowy neon (róż/cyjan) na czerni, Orbitron |
| Her | Ciepły pastel (łososiowy, kremowy), futurystyczna minimalistyczna typografia |
| Roma (Fellini) | Neorealizm, ziarnistość, szarość i biel, elegancki włoski serif |

**Zasady estetyczne (OBOWIĄZKOWE):**
- Co najmniej 3 kolory dominujące + 1 akcentowy ze świata wizualnego dzieła
- 2 fonty z Google Fonts: display (nagłówki) i body (dane, opisy) — żadnych Inter, Arial, Roboto
- Ciemny lub jasny motyw odpowiadający tonowi dzieła
- Subtelny efekt tła (gradient, noise texture, wzór geometryczny)
- CSS zmienne (`--var`) dla całej palety

**Kolory płci (DOPASUJ DO PALETY DZIEŁA — nie używaj generycznego różu i niebieskiego):**
Wybierz dwa kolory z palety dzieła i przypisz je do K i M. Użyj tych kolorów konsekwentnie we wszystkich czterech wykresach i w sieci.

---

## KROK 2 — UKŁAD PREZENTACJI

Zbuduj **jeden plik HTML** z nawigacją między slajdami. Wymagania:

**Nawigacja:**
- Przyciski `◀` i `▶` (strzałki) widoczne na każdym slajdzie
- Obsługa klawiszy: strzałka w lewo / strzałka w prawo
- Wskaźnik postępu: numer slajdu / łączna liczba slajdów (np. „3 / 6")
- Płynne przejście między slajdami (CSS transition, opacity lub slide)

**Każdy slajd zajmuje pełny ekran (100vw × 100vh).**

---

## KROK 3 — ZAWARTOŚĆ SLAJDÓW

### Slajd 1 — TYTUŁ

- Tytuł: wykryty film (duży font display, z estetyką dzieła)
- Podtytuł: „Głos · Płeć · Emocje — Analiza Dialogowa Scenariusza"
- Podpis na dole: „Humanistyka Cyfrowa · Character Network Analysis"
- Estetyka: najefektowniejszy wizualnie slajd

### Slajd 2 — KTO MÓWI?

**Wykres: udział w dialogu × płeć**

Dane: `top_speakers` (max 15 postaci) + `speech_by_gender`

- Poziomy wykres słupkowy D3: oś Y = postać, oś X = liczba słów (lub procent)
- Kolor słupka = płeć postaci (kolory z palety dzieła)
- Sortowanie malejące wg liczby słów
- Pod wykresem: podsumowanie „X% dialogu wypowiadają postacie kobiece / Y% — postacie męskie"
- Etykiety wartości na końcu słupków

Tytuł slajdu + krótki (2-3 zdania) komentarz metodologiczny na dole: czym jest udział w mowie, czym nie jest.

### Slajd 3 — ŁUK EMOCJONALNY

**Wykres: trajektoria walencji wzdłuż scen**

Dane: `emotion_arc`

- Wykres liniowy D3: oś X = numer sceny, oś Y = wygładzona walencja (zakres −1 do +1)
- Pozioma linia zerowa (wyraźna, inna grubość lub styl)
- Wypełnienie obszaru pod/nad linią zerową: inny kolor dla obszarów pozytywnych i negatywnych (przezroczyste, ~30% opacity)
- Tooltip przy hover: numer sceny + wartość walencji
- Tytuł slajdu + pytanie retoryczne: „Jaki kształt emocjonalny ma ta historia?"

Krótki komentarz na dole: skąd pochodzi pojęcie „kształtu historii" (Reagan i in. 2016, Vonnegut).

### Slajd 4 — EMOCJE × PŁEĆ

**Wykres: rozkład emocji według płci**

Dane: `emotion_by_gender`

- Grupowany lub ułożony wykres słupkowy D3 z 6 kategoriami emocji
- Dwie grupy: K i M (kolory z palety dzieła)
- Wartości procentowe (oś Y = 0–100%)
- Legenda emocji z ikonami lub kolorowymi cropkami
- Tooltip przy hover: procent + bezwzględna liczba kwestii

Tytuł slajdu + krótkie zastrzeżenie metodologiczne na dole: emocja przypisana przez model to interpretacja tekstu, nie subiektywne odczucie postaci.

### Slajd 5 — SIEĆ POSTACI

**Interaktywna sieć D3 Force-Directed**

Dane: `network.nodes` + `network.edges`

- `d3.forceSimulation` z siłami: link (distance ∝ 1/weight), charge (odpychanie), center, collision
- Rozmiar węzła ∝ `word_count` (skalowanie min–max, węzeł z 0 słowami = mały ale widoczny)
- Kolor węzła = płeć (te same kolory co we wszystkich poprzednich slajdach)
- Grubość krawędzi ∝ waga (min 0.5px, max 4px)
- Etykiety postaci: zawsze widoczne dla top 10 wg word_count; dla pozostałych tylko po hover
- Zoom + pan (d3.zoom)
- Drag węzłów
- Po kliknięciu węzła: przyciemnij niepowiązane, pokaż kartę postaci (nazwa, płeć, word_count, dominant_emotion, avg_valence, top 3 sąsiadów)
- Tooltip hover: nazwa + top 3 metryki

Tytuł slajdu + legenda: kolor = płeć, rozmiar = liczba słów.

### Slajd 6 — INTERPRETACJA

**Humanistyczna analiza wyników**

Napisz interpretację jako sformatowany HTML zawierający:

**6.1 Głos i reprezentacja**
- Który procent mowy należy do kobiet / mężczyzn?
- Interpretacja: czy film „koncentruje głos" w jednej płci? Co to znaczy dla narracji?

**6.2 Kształt emocjonalny**
- Opisz łuk emocjonalny: gdzie są doliny i szczyty?
- Czy kształt odpowiada archetypicznym strukturom (tragedia, bohater, odrodzenie)?

**6.3 Emocje a płeć**
- Czy rozkład emocji różni się między K i M?
- Czy film reprodukuje stereotypy emocjonalne (złość → mężczyźni, smutek → kobiety) czy im przeczy?

**6.4 Sieć a głos**
- Czy postać centralnie osadzona w sieci (wiele połączeń) to też ta, która mówi najwięcej?
- Jeśli nie — co mówi ta rozbieżność o strukturze narracyjnej?

**6.5 Propozycje dalszych analiz**
3–4 konkretne pytania badawcze specyficzne dla tego dzieła.

---

## KROK 4 — WYMAGANIA TECHNICZNE

- **Jeden plik HTML** — cały CSS, JS i dane inline
- **D3.js v7** ładowane z: `https://cdnjs.cloudflare.com/ajax/libs/d3/7.8.5/d3.min.js`
- **Google Fonts** z `@import` w CSS — fonty muszą pasować do estetyki dzieła
- Dane z bundle.json zakoduj jako stałe JS na początku skryptu
- Responsywność: wykresy reagują na zmianę rozmiaru okna
- **NIE używaj** localStorage, sessionStorage, zewnętrznych API, frameworków (React/Vue)
- **NIE używaj** generycznych estetyk: gradient fioletowy na białym, Inter/Roboto, błękitny pastel

---

## KROK 5 — KOLEJNOŚĆ WYKONANIA

1. Sparsuj bundle.json → przygotuj stałe JS dla każdego wykresu
2. Zidentyfikuj dzieło → wybierz estetykę → zapisz kolory i fonty jako CSS zmienne
3. Zbuduj strukturę HTML (6 div.slide) z nawigacją
4. Zaimplementuj slajd po slajdzie: 1 (tytuł), 2 (głos), 3 (łuk), 4 (emocje), 5 (sieć), 6 (interpretacja)
5. Dodaj obsługę klawiatury i nawigację
6. Napisz interpretację humanistyczną (slajd 6) na podstawie faktycznych danych z bundle.json
7. Sprawdź, czy wszystkie interakcje działają (tooltip, klik węzła, nawigacja)

---

## PRZYKŁAD — co powinien zawierać wygenerowany plik

Dla Pulp Fiction: ciemne tło (#0d0d0d), żółć (#f39c12) dla mężczyzn i czerwień (#c0392b) dla kobiet, Bebas Neue na nagłówkach, Courier Prime na danych; łuk emocjonalny pokazujący wyraźną dolinę w środkowej części (sceny z Marcellusem i Butchem), zdominowany głos męski (Jules, Vincent, Butch); sieć z węzłami Jules i Vincent jako największymi.

---

Wygeneruj TYLKO jeden kompletny plik HTML. Nie wyjaśniaj kodu. Nie dodawaj niczego poza plikiem HTML.
```

---

## Uwagi dla użytkownika

**Najlepsze wyniki uzyskasz gdy:**
- Plik `bundle.json` pochodzi z notatnika `analiza_dialogow_03_synteza.ipynb` i zawiera wszystkie wymagane klucze
- Używasz modelu z dużym oknem kontekstowym — wygenerowany HTML może mieć 700–1000 linii
- Podajesz tytuł filmu ręcznie, jeśli model go nie rozpoznaje: „Graf pochodzi z [tytuł]. Estetykę dobierz do tego dzieła."

**Jeśli plik HTML jest obcięty:**
Poproś model: „Kontynuuj generowanie od miejsca, gdzie się zatrzymałeś." Możesz też poprosić o slajdy w częściach: „Najpierw slajdy 1–3", potem „Dokończ slajdy 4–6".

**Jeśli chcesz wymusić konkretną estetykę:**
Dodaj do wiadomości: „Użyj estetyki: [Twój opis]"

**Testowane z:**
- Claude Sonnet 4 / Opus 4
- GPT-4o
- Gemini 1.5 Pro / 2.0 Flash

---

_Prompt zaprojektowany jako uzupełnienie modułu SNA w ramach kursu z humanistyki cyfrowej. Łączy analizę sieciową z analizą dialogową i oceną emocji przez modele językowe._
