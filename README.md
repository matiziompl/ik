# 📚 Powtórka — Interfejsy Komputerów Cyfrowych (IK)
### Wojskowa Akademia Techniczna

---

## Spis treści

1. [System wejścia-wyjścia — architektura](#1-system-wejściawyjścia--architektura)
2. [Sposoby realizacji transmisji danych](#2-sposoby-realizacji-transmisji-danych)
3. [Transmisja szeregowa — rodzaje i cechy](#3-transmisja-szeregowa--rodzaje-i-cechy)
4. [Bit parzystości i nieparzystości](#4-bit-parzystości-i-nieparzystości)
5. [Klawiatura — interfejs PS/2 i obsługa programowa](#5-klawiatura--interfejs-ps2-i-obsługa-programowa)
6. [Karta graficzna — tryby i pamięć ekranu](#6-karta-graficzna--tryby-i-pamięć-ekranu)
7. [Interfejsy graficzne — VGA, DVI, HDMI, DisplayPort](#7-interfejsy-graficzne--vga-dvi-hdmi-displayport)
8. [Drukarki — mechanizmy i języki](#8-drukarki--mechanizmy-i-języki)
9. [Urządzenia wejścia — skaner, digitizer, ploter](#9-urządzenia-wejścia--skaner-digitizer-ploter)
10. [Magistrale — ISA, PCI, PCI Express](#10-magistrale--isa-pci-pci-express)
11. [Metody transferu danych — PIO i DMA](#11-metody-transferu-danych--pio-i-dma)
12. [Port równoległy LPT — SPP, EPP, ECP](#12-port-równoległy-lpt--spp-epp-ecp)
13. [USB — architektura i transmisja](#13-usb--architektura-i-transmisja)
14. [Pamięci masowe — HDD, SSD, optyczne](#14-pamięci-masowe--hdd-ssd-optyczne)
15. [Monitory — CRT, LCD, Plazma](#15-monitory--crt-lcd-plazma)

---

## 1. System wejścia-wyjścia — architektura

### 1.1 Podstawowe pojęcia

System wejścia-wyjścia (I/O) jest częścią komputera odpowiedzialną za komunikację z urządzeniami zewnętrznymi. Składa się z trzech poziomów:

```
┌─────────────────────────────────────────────┐
│                 PROCESOR (CPU)              │
└─────────────────┬───────────────────────────┘
                  │ magistrala systemowa
┌─────────────────▼───────────────────────────┐
│         MODUŁ WEJŚCIA-WYJŚCIA               │
│  (kontroler I/O / chipset)                  │
└──────┬──────────────────────┬───────────────┘
       │ interfejs             │ interfejs
┌──────▼──────┐         ┌─────▼───────┐
│  Urządzenie │         │  Urządzenie │
│  zewnętrzne │         │  zewnętrzne │
└─────────────┘         └─────────────┘
```

**Interfejs** — zespół ustalonych reguł oraz środków technicznych umożliwiających łączenie komputera z urządzeniami zewnętrznymi. Definiuje: standard sygnałów elektrycznych, protokół komunikacyjny, mechanikę złącza.

**Moduł wejścia-wyjścia** — element pośredniczący między procesorem a urządzeniami zewnętrznymi. Realizuje:
- sterowanie i taktowanie komunikacją z procesorem,
- sterowanie komunikacją z urządzeniami peryferyjnymi,
- buforowanie danych (wyrównywanie różnicy prędkości),
- wykrywanie i sygnalizowanie błędów.

**Urządzenie wejścia-wyjścia** — fizyczne urządzenie peryferyjne, które zawiera:
- **przetwornik** — zamienia sygnały fizyczne na elektryczne i odwrotnie (np. głowica odczytująca, dysza inkjet),
- **bufor** — tymczasowe przechowywanie danych,
- **układy logiczne sterowania** — lokalna logika zarządzająca pracą urządzenia.

**Adapter** — pośrednik dopasowujący standard urządzenia do magistrali komputera (np. karta graficzna, karta sieciowa).

---

### 1.2 Rozkazy wejścia-wyjścia procesora x86

Procesor x86 komunikuje się z urządzeniami przez dwa rozkazy:

```asm
IN  AL, nrp    ; odczyt (READ)  — przesłanie z portu nrp do rejestru AL
OUT nrp, AL    ; zapis  (WRITE) — przesłanie z rejestru AL do portu nrp
```

- `nrp` — numer portu = adres urządzenia zewnętrznego w przestrzeni I/O (0–FFFFh)
- Przestrzeń portów I/O jest osobna od przestrzeni adresowej pamięci RAM

Przykłady adresów portów klawiatury:
- Port **60H** — dane klawiatury (scan code, polecenia do klawiatury)
- Port **64H** — status i polecenia do kontrolera klawiatury (8042)

---

## 2. Sposoby realizacji transmisji danych

### 2.1 Transmisja równoległa

Przesyłanie **całego słowa jednocześnie** — każdy bit ma własną, dedykowaną linię danych.

```
Nadajnik                    Odbiornik
   [D7]─────────────────────[D7]
   [D6]─────────────────────[D6]
   [D5]─────────────────────[D5]
   [D4]─────────────────────[D4]
   [D3]─────────────────────[D3]
   [D2]─────────────────────[D2]
   [D1]─────────────────────[D1]
   [D0]─────────────────────[D0]
```

- Do przesłania słowa n-bitowego potrzeba **n linii danych**
- Zaletą jest duża szybkość przy krótkich odległościach
- Wadą jest koszt kabla i podatność na tzw. **skew** (różne czasy propagacji na różnych liniach) przy dużych prędkościach
- Przykład: port LPT (Centronics), starsze magistrale (ISA, PCI)

### 2.2 Transmisja szeregowa

Przesyłanie **bitów kolejno jeden po drugim** przez jedną linię danych.

```
Nadajnik                    Odbiornik
   [DATA]──────────────────[DATA]
   ([CLK])─────────────────([CLK])   ← opcjonalnie
```

- Wymaga tylko **1 linii danych** (+ opcjonalnie linia zegara)
- Wolniejsza niż równoległa dla tego samego taktu, ale umożliwia wyższe częstotliwości
- Brak problemu ze skew
- Przykłady: USB, PCIe, SATA, PS/2, RS-232

---

## 3. Transmisja szeregowa — rodzaje i cechy

### 3.1 Transmisja asynchroniczna

Brak wspólnego sygnału zegarowego. Każda ramka danych zawiera bity synchronizacji:
- **bit startu** (zawsze 0) — informuje odbiornik o początku ramki
- **bity danych** (5–9 bitów)
- **bit parzystości** (opcjonalny)
- **bit(y) stopu** (zawsze 1) — minimalny czas przerwy między ramkami

```
Linia w spoczynku: ─────────1 1 1 1 1 1 1 1 1 1 1─────────
Ramka:                       │0│D0│D1│D2│D3│D4│D5│D6│D7│P│1│
                              S                             STOP
                              T
                              A
                              R
                              T
```

Przykład: RS-232 (COM port)

### 3.2 Transmisja synchroniczna

Nadajnik i odbiornik zsynchronizowane są wspólnym sygnałem **zegarowym (CLK)**. Dane zmieniają się na jednym zboczu zegara, a są próbkowane na drugim.

```
CLK:  ─┐ ┌─┐ ┌─┐ ┌─┐ ┌─
       └─┘ └─┘ └─┘ └─┘
DATA: ──┤D0├──┤D1├──┤D2├──┤D3├──
```

- **Wymaga linii sygnału zegarowego**
- Brak bitów startu/stopu — wyższa efektywność
- Przykłady: SPI, I²C, **PS/2**, ISA, PCI

### 3.3 Transmisja izochroniczna

Specjalny rodzaj transmisji synchronicznej, w którym gwarantowane jest przesyłanie danych w **regularnych, stałych przedziałach czasu**.

- Kluczowa dla multimediów (audio, video) — zapobiega "zacięciom"
- Gwarantuje Quality of Service (QoS) w wymiarze czasowym
- Przykład: tryb izochroniczny **USB** (dla kamer, głośników)

### 3.4 Transmisja różnicowa (differential signaling)

Dane przesyłane są **parą przewodów** (DATA+ i DATA–) o przeciwnych poziomach napięcia. Odbiornik mierzy różnicę potencjałów.

```
DATA+: ─────┐   ┌─────
            └───┘
DATA–: ─────┐   ┌─────   (odwrócony względem DATA+)
```

- Bardzo odporna na zakłócenia elektromagnetyczne (EMI)
- Pozwala na wysokie prędkości transmisji
- Przykłady: USB (DATA+, DATA–), PCIe, SATA, RS-485

---

## 4. Bit parzystości i nieparzystości

Bit parzystości jest **prostą metodą wykrywania błędów** w transmisji. Dodawany jest do każdej grupy bitów danych tak, by suma wszystkich jedynek (dane + bit parzystości) spełniała pewien warunek.

### 4.1 Bit parzystości (even parity)

Bit ustawiany tak, by **łączna liczba jedynek była parzysta**.

| Dane | Liczba '1' | Bit parzystości | Łącznie '1' |
|------|-----------|-----------------|-------------|
| 0011011 | 4 (parzysta) | **0** | 4 (parzysta ✅) |
| 1010101 | 4 (parzysta) | **0** | 4 (parzysta ✅) |
| 1110000 | 3 (nieparzysta) | **1** | 4 (parzysta ✅) |

### 4.2 Bit nieparzystości (odd parity)

Bit ustawiany tak, by **łączna liczba jedynek była nieparzysta**.

| Dane | Liczba '1' | Bit nieparzystości | Łącznie '1' |
|------|-----------|-------------------|-------------|
| 0011011 | 4 (parzysta) | **1** | 5 (nieparzysta ✅) |
| 1010101 | 4 (parzysta) | **1** | 5 (nieparzysta ✅) |
| 1110000 | 3 (nieparzysta) | **0** | 3 (nieparzysta ✅) |

### 4.3 Algorytm obliczania

```
1. Policz liczbę jedynek w ciągu danych
2. Even parity: jeśli liczba '1' jest nieparzysta → bit = 1
                jeśli liczba '1' jest parzysta   → bit = 0
3. Odd parity:  jeśli liczba '1' jest parzysta   → bit = 1
                jeśli liczba '1' jest nieparzysta → bit = 0
```

> **Ważne:** Interfejs PS/2 używa bitu **nieparzystości (odd parity)**.

---

## 5. Klawiatura — interfejs PS/2 i obsługa programowa

### 5.1 Standard PS/2

PS/2 to synchroniczny, szeregowy interfejs klawiatury i myszy. Złącze mini-DIN 6-pin, ale używane są tylko 4 linie:

| Linia | Opis |
|-------|------|
| **VCC** | Zasilanie +5V |
| **GND** | Masa |
| **DATA** | Dane szeregowe (1 linia) |
| **CLOCK** | Sygnał zegarowy generowany przez klawiaturę |

> **Uwaga:** USB ma linie DATA+ i DATA– (sygnał różnicowy). PS/2 ma DATA i CLOCK (sygnał zegarowy).

### 5.2 Ramka transmisji PS/2

Każda ramka składa się z **11 bitów**:

```
Bit:   1    2–9    10       11
      [0] [D0–D7] [P]      [1]
      START  DATA  PARITY   STOP
```

- **Bit startu** = zawsze 0
- **8 bitów danych** (LSB pierwszy)
- **Bit parzystości** = nieparzystości (odd parity)
- **Bit stopu** = zawsze 1

### 5.3 Kierunek transmisji PS/2

| Kierunek | Opis |
|----------|------|
| **Klawiatura → Adapter** | Wysyłanie scan code po naciśnięciu lub zwolnieniu klawisza |
| **Adapter → Klawiatura** | Wysyłanie poleceń: włącz/wyłącz LED, ustaw autorepetycję, zresetuj |

### 5.4 Scan code

Każdy klawisz ma przypisany **scan code** (kod sprzętowy) — niezależny od systemu operacyjnego. Przy:
- **naciśnięciu** klawisza → wysyłany jest **make code**
- **zwolnieniu** klawisza → wysyłany jest **break code** (zwykle F0h + make code)

### 5.5 Autorepetycja (Typematic)

Gdy klawisz jest przytrzymany, klawiatura automatycznie powtarza wysyłanie scan code. Dwa parametry:
- **Typematic Delay** — czas opóźnienia od naciśnięcia do początku powtarzania (wyrażany w ms, np. 250/500/750/1000 ms)
- **Typematic Rate** — **szybkość powtarzania** (wyrażana w znakach na sekundę, np. 2–30 znaków/s)

### 5.6 Porty klawiatury w przestrzeni I/O

| Port | Adres | Zapis | Odczyt |
|------|-------|-------|--------|
| Dane | **60H** | Polecenie do klawiatury | Scan code od klawiatury |
| Status/Polecenia | **64H** | Polecenie do kontrolera 8042 | Bajt statusu kontrolera 8042 |

**Bajt statusu portu 64H:**
- Bit 0 (OBF — Output Buffer Full): = 1 → w porcie 60H czeka bajt do odczytu
- Bit 1 (IBF — Input Buffer Full): = 1 → kontroler zajęty, nie pisz do 60H/64H

### 5.7 Obsługa programowa — przerwanie BIOS INT 16H

BIOS dostarcza usługi obsługi klawiatury poprzez przerwanie **INT 16H**. Numer funkcji ustawia się w rejestrze **AH**.

```asm
MOV AH, 00H    ; funkcja 00H = czekaj na klawisz i odczytaj
INT 16H        ; wywołanie usługi BIOS klawiatury
; po powrocie: AH = scan code, AL = kod ASCII
```

Najważniejsze funkcje INT 16H:

| AH | Funkcja |
|----|---------|
| 00H | Czekaj na klawisz, odczytaj (blokująca) |
| 01H | Sprawdź czy klawisz dostępny (nieblokująca) |
| 02H | Odczytaj bajt stanu klawiszy modyfikujących |

**Przerwania BIOS — numery:**

| INT | Obsługuje |
|-----|-----------|
| **10H** | Karta graficzna (usługi wideo) |
| **16H** | Klawiatura |
| **21H** | DOS — usługi systemu operacyjnego |

### 5.8 Bufor klawiatury BIOS

BIOS utrzymuje w obszarze danych (BDA — BIOS Data Area) **cykliczny bufor klawiatury**:
- Rozmiar: **32 bajty** = **16 słów dwubajtowych**
- Każde słowo = **bajt scan code** + **bajt ASCII**
- Bufor wypełniany przez przerwanie IRQ1 (ISR klawiatury), opróżniany przez INT 16H

### 5.9 Obszar danych BIOS (BDA)

BDA zaczyna się od adresu logicznego **0040H:0000H** (adres fizyczny 0400H).

Ważne komórki:

| Adres logiczny | Zawartość |
|----------------|-----------|
| **0040H:0017H** | Bajt stanu klawiszy Lock i modyfikujących (Shift, Ctrl, Alt, Caps Lock, Num Lock, Scroll Lock) |
| 0040H:001AH | Wskaźnik ogona bufora klawiatury (HEAD) |
| 0040H:001CH | Wskaźnik głowy bufora klawiatury (TAIL) |

---

## 6. Karta graficzna — tryby i pamięć ekranu

### 6.1 Tryb tekstowy

W trybie tekstowym ekran jest podzielony na siatkę komórek znakowych. Standard VGA/MDA definiuje tryb **80×25×16**:

- **80** — liczba kolumn (znaków w wierszu)
- **25** — liczba wierszy
- **16** — liczba dostępnych kolorów (4-bitowa paleta)

**Organizacja pamięci ekranu w trybie tekstowym:**  
Każdy znak zajmuje **2 bajty**:

```
Bajt 1: Kod ASCII znaku (np. 'A' = 41H)
Bajt 2: Bajt atrybutu
```

**Struktura bajtu atrybutu (8 bitów):**

```
Bit:  7    6   5   4    3   2   1   0
     [BL] [R] [G] [B] [I] [R] [G] [B]
      │    └─────────┘  │  └─────────┘
      │    kolor tła    │  kolor znaku
      │    (3 bity)     │  (3 bity + intensity)
      miganie          intensity
```

Łączna pamięć dla ekranu 80×25: **80 × 25 × 2 = 4000 bajtów**

### 6.2 Tryb graficzny

W trybie graficznym każdy piksel jest adresowany osobno. Standard **320×200×256**:

- **320** — szerokość w pikselach
- **200** — wysokość w pikselach
- **256** — liczba kolorów (8-bitowa paleta)

Każdy piksel = **1 bajt** (indeks do 256-kolorowej tablicy Look-Up Table, LUT).

**Łączna pamięć:** 320 × 200 × 1 = **64 000 bajtów = 64 KB**

### 6.3 Obsługa programowa — INT 10H

BIOS dostarcza usługi wideo przez przerwanie **INT 10H**:

| AH | Funkcja |
|----|---------|
| 00H | Ustaw tryb video |
| 02H | Ustaw pozycję kursora |
| 09H | Wypisz znak z atrybutem |
| 0EH | Wypisz znak w trybie TTY |

---

## 7. Interfejsy graficzne — VGA, DVI, HDMI, DisplayPort

### 7.1 VGA (Video Graphics Array)

- Sygnał **analogowy** (R, G, B — trzy składowe napięciowe)
- Złącze: D-Sub 15-pin (DE-15)
- Przekazuje wyłącznie sygnał wideo (brak audio)
- Standard z 1987 roku, coraz rzadziej stosowany
- Podatny na zakłócenia i degradację sygnału przy długich kablach

### 7.2 DVI (Digital Visual Interface)

Standard wprowadzony w 1999 roku. Istnieje w trzech wariantach:

| Wariant | Sygnał |
|---------|--------|
| **DVI-D** | Tylko cyfrowy |
| **DVI-A** | Tylko analogowy (kompatybilny z VGA) |
| **DVI-I** | Cyfrowy + analogowy (Integrated) |

- Używa kodowania TMDS (Transition Minimized Differential Signaling)
- Brak sygnału audio

### 7.3 HDMI (High-Definition Multimedia Interface)

- Sygnał **cyfrowy** — obraz i **dźwięk** w jednym kablu
- Obsługuje HDCP (ochrona treści — High-bandwidth Digital Content Protection)
- Złącze: Type A (pełnowymiarowe), Type C (mini), Type D (micro)
- Szeroko stosowany w TV, monitorach, projektorach

### 7.4 DisplayPort (DP)

- Sygnał **cyfrowy** — obraz i dźwięk
- Opracowany przez VESA jako następca VGA/DVI
- Obsługuje Multi-Stream Transport (MST) — jeden kabel do wielu monitorów (daisy-chain)
- Wyższe przepustowości niż HDMI (szczególnie dla wysokich rozdzielczości i odświeżania)
- Złącze: pełnowymiarowe DP lub Mini DisplayPort

### 7.5 Podsumowanie

| Interfejs | Sygnał | Audio | Uwagi |
|-----------|--------|-------|-------|
| **VGA** | Analogowy | Nie | Stary standard, coraz rzadszy |
| **DVI** | Cyfrowy / Analogowy | Nie | DVI-D, DVI-A, DVI-I |
| **HDMI** | Cyfrowy | Tak | TV, konsole, monitory |
| **DisplayPort** | Cyfrowy | Tak | Komputery, multi-monitor |

---

## 8. Drukarki — mechanizmy i języki

### 8.1 Mechanizmy druku

#### Drukarka mozaikowa (igłowa, matrix)
- Głowica z matrycą igieł (9 lub 24 igły)
- Igły uderzają przez taśmę barwiącą w papier
- Druk **uderzeniowy** — jedyny typ drukarki drukujący przez kalki
- Hałaśliwa, wolna, ale tania w eksploatacji
- Zastosowania: kasy fiskalne, wydruki wielokopiowe

#### Drukarka termiczna
- Głowica grzewcza nagrzewa **termoczuły papier** (papier zmienia kolor pod wpływem ciepła)
- Lub: głowica grzewcza topi warstwę wosku/żywicy z taśmy termotransferowej na zwykły papier
- Brak tuszu/tonera
- Zastosowania: paragony, etykiety, faksy

#### Drukarka natryskowa (inkjet, atramentowa)
- Krople atramentu wyrzucane z dysz na papier
- Dwie technologie:
  - **Termiczna (bubble jet)** — grzałka tworzy parę, która wypycha kroplę (Canon, HP)
  - **Piezoelektryczna** — piezoelektryczny kryształ deformuje się i wypycha kroplę (Epson)
- Cicha, dobra jakość wydruku zdjęć
- Wysoki koszt tonera/tuszu

#### Drukarka laserowa
- Bęben światłoczuły ładowany elektrostatycznie
- Laser (lub LED) selektywnie rozładowuje bęben, tworząc utajony obraz
- Naelektryzowany toner (proszek) przyciągany do rozładowanych obszarów bębna
- Toner przenoszony na papier i utrwalany przez **grzałkę (fuser)**
- Szybka, wysokiej jakości, ekonomiczna przy dużych nakładach

#### Drukarka bębnowa (drum printer)
- Stary typ drukarki wierszowej
- Bęben z wypukłymi znakami obraca się, młotki uderzają w odpowiednim momencie
- Drukuje cały wiersz na raz

### 8.2 Języki sterowania drukarkami

#### PCL (Printer Command Language)
- Opracowany przez HP w 1984 roku
- Definiuje **zestaw sekwencji sterujących** opisujących format strony: czcionki, marginesy, grafika, pozycjonowanie
- Stosowany przez większość drukarek laserowych i atramentowych

#### PJL (Printer Job Language)
- Opracowany przez HP jako „nakładka" nad PCL i PostScriptem
- Działa na poziomie **zarządzania zadaniami** drukarki, nie zawartością strony
- Zastosowania: **współdzielenie drukarki**, przełączanie języków, raportowanie statusu, konfiguracja

---

## 9. Urządzenia wejścia — skaner, digitizer, ploter

### 9.1 Skaner

Urządzenie wejścia, które **przekształca statyczny obraz fizyczny** (dokument, zdjęcie, obiekt) **do postaci cyfrowej** (plik graficzny). Źródłem jest istniejący, fizyczny obraz.

Typy:
- **Płaski (flatbed)** — dokument układa się na szybie
- **Podajnikowy (sheet-fed)** — karty przesuwane przez mechanizm
- **Ręczny (hand-held)**
- **Bębnowy (drum)** — profesjonalne, najwyższa jakość

Parametry: rozdzielczość (DPI — dots per inch), głębia kolorów (np. 24-bit), zakres dynamiczny.

### 9.2 Digitizer (tablet graficzny)

Urządzenie wejścia umożliwiające **ręczne tworzenie rysunków** za pomocą specjalnego **rysika** na powierzchni tabletu. Rysik rejestruje pozycję i nacisk.

- Używany przez grafików, ilustratorów, architektów
- Rysik może być z bateriami lub bezprzewodowy (zasilany indukcyjnie przez tablet)
- Zastosowania: edycja zdjęć, ilustracja cyfrowa, CAD

### 9.3 Ploter (pisak XY / cutter)

Urządzenie wyjścia przeznaczone do **pracy z dużymi formatami** (A0 i większe). Może:
- **nanosić obrazy** (piórem, pisakiem, długopisem)
- **wycinać wzory** (ploter tnący/vinylowy)
- **grawerować** (grawerka CNC)
- **kreślić mapy** techniczne i geograficzne

Ploter sterowany jest współrzędnymi X i Y — stąd nazwa "pisak XY".

### 9.4 Podsumowanie

| Urządzenie | Typ | Rola |
|------------|-----|------|
| Skaner | Wejście | Digitalizacja istniejącego obrazu |
| Digitizer | Wejście | Ręczne tworzenie rysunków rysikiem |
| Ploter | Wyjście | Rysowanie/wycinanie na dużych formatach |

---

## 10. Magistrale — ISA, PCI, PCI Express

### 10.1 ISA (Industry Standard Architecture)

- Magistrala **równoległa**, współdzielona (shared bus)
- Wersje: 8-bit (8 MHz), 16-bit (8 MHz)
- Przepustowość: do 8 MB/s (16-bit)
- Synchroniczna — wszystkie urządzenia taktowane tym samym sygnałem zegarowym
- Obsługuje: IRQ, DMA, I/O ports, memory space
- Wyparta przez PCI w połowie lat 90.

### 10.2 PCI (Peripheral Component Interconnect)

- Magistrala **równoległa**, współdzielona
- Szerokość szyny: 32 lub 64 bity
- Częstotliwość: 33 lub 66 MHz
- Przepustowość: do 532 MB/s (64-bit, 66 MHz)
- Plug & Play — automatyczna konfiguracja
- Wyparta przez PCIe na początku lat 2000.

### 10.3 PCI Express (PCIe)

Fundamentalna zmiana architektury — z magistrali współdzielonej na **topologię punkt-punkt**.

#### Architektura PCIe

```
         Root Complex
        /      |      \
    Switch    EP     EP
    /   \
  EP    EP
```

Typy elementów:
- **Root Complex** — most między procesorem/RAM a siecią PCIe (w chipsecie/CPU)
- **Switch** — przełącznik zwiększający liczbę portów PCIe
- **Endpoint (EP)** — urządzenie końcowe (karta graficzna, SSD NVMe, karta sieciowa)

#### Transmisja w PCIe

- Transmisja **szeregowa, różnicowa** (para TX+/TX– i RX+/RX–)
- Każdy **lane** (tor) = jeden kanał transmisji w obie strony jednocześnie (full-duplex)
- Sloty: x1, x4, x8, x16 — liczba lane'ów
- Kodowanie: 8b/10b (PCIe 1.x, 2.x), 128b/130b (PCIe 3.x, 4.x, 5.x)

| Generacja | Prędkość/lane (1 kierunek) |
|-----------|--------------------------|
| PCIe 1.0 | 250 MB/s |
| PCIe 2.0 | 500 MB/s |
| PCIe 3.0 | ~1 GB/s |
| PCIe 4.0 | ~2 GB/s |
| PCIe 5.0 | ~4 GB/s |

---

## 11. Metody transferu danych — PIO i DMA

### 11.1 PIO (Programmed I/O)

Procesor **aktywnie uczestniczy** w każdej operacji transferu danych:

```
Schemat PIO – odczyt (czytaj):
┌────────┐   1. żądanie danych   ┌────────────┐
│  CPU   │──────────────────────►│  Moduł I/O │
│        │◄──────────────────────│            │
└────────┘   2. dane → rejestr  └────────────┘
     │
     │  3. CPU kopiuje z rejestru do pamięci
     ▼
┌────────┐
│  RAM   │
└────────┘
```

- CPU jest zajęty przez cały czas transferu (polling lub rozkazy IN/OUT)
- Prosta implementacja
- Nieefektywne przy dużych transferach (CPU marnuje czas)
- Typy: **PIO pisz** (CPU → urządzenie), **PIO czytaj** (urządzenie → CPU)

### 11.2 DMA (Direct Memory Access)

Specjalny kontroler DMA przesyła dane **bezpośrednio między pamięcią a urządzeniem**, bez angażowania procesora w każdy transfer:

```
Schemat DMA – odczyt:
┌────────┐  1. CPU inicjuje DMA  ┌────────────┐
│  CPU   │──────────────────────►│ Kontroler  │
│        │                       │    DMA     │
└────────┘                       └──────┬─────┘
                                        │ 2. DMA steruje magistralą
              ┌────────┐                │
              │  RAM   │◄───────────────┘
              └────────┘   3. transfer bezpośredni
                                        │
              ┌────────────┐            │
              │  Moduł I/O │◄───────────┘
              └────────────┘
4. Po zakończeniu: DMA zgłasza przerwanie do CPU
```

- CPU jest zwolniony podczas transferu (może wykonywać inne zadania)
- Po zakończeniu transferu DMA zgłasza **przerwanie** do CPU
- Efektywne dla dużych bloków danych (np. dyski twarde, karty dźwiękowe)
- Typy: **DMA pisz** (pamięć → urządzenie), **DMA czytaj** (urządzenie → pamięć)

---

## 12. Port równoległy LPT — SPP, EPP, ECP

Port LPT (Line PrinTer) to złącze DB-25 stosowane do podłączenia drukarek i innych urządzeń.

### 12.1 SPP (Standard Parallel Port) — Centronics

- Tryb **jednostronny** — dane płyną tylko od komputera do urządzenia
- 8 linii danych (D0–D7)
- Linie sterujące: STROBE (dane gotowe), BUSY (urządzenie zajęte), ACK (potwierdzenie)
- Przepustowość: ~150 KB/s

```
Protokół SPP (uproszczony):
Komputer:   STROBE ──────┐   ┌──────
                          └───┘
Drukarka:   BUSY   ────────────────┐
                                    └──
Drukarka:   ACK    ──────────────────┐ └─
```

### 12.2 EPP (Enhanced Parallel Port)

- Tryb **dwukierunkowy**
- Sprzętowy handshaking (poprzednio programowy) → szybszy transfer
- Oddzielne cykle dla adresów i danych
- Przepustowość: do ~2 MB/s
- Stosowany dla: skanerów, dysków zewnętrznych, CD-ROM

### 12.3 ECP (Extended Capabilities Port)

- Tryb **dwukierunkowy**
- Obsługuje **kompresję RLE** (Run-Length Encoding) sprzętowo
- Posiada bufor **FIFO** (kolejka)
- Obsługuje **DMA** — odciąża procesor
- Dwa tryby pracy:
  - **Forward** (komputer → urządzenie) — tryb zapisu
  - **Reverse** (urządzenie → komputer) — tryb odczytu
- Przepustowość: do ~4 MB/s
- Sygnał ChannelAddress (CA) odróżnia dane od informacji sterujących

---

## 13. USB — architektura i transmisja

### 13.1 Topologia USB

USB używa topologii **gwiazdy** ze wzmacniaczami (hub):

```
          Host (komputer)
               │
            [Hub]
          /    |    \
      [Hub]  [EP]  [EP]
      /   \
    [EP] [EP]
```

- Host kontroluje całą komunikację (master)
- Urządzenia (Endpoint) tylko odpowiadają na żądania hosta
- Jedno złącze USB może obsłużyć do 127 urządzeń (7-bitowy adres)

### 13.2 Linie USB

| Linia | Opis |
|-------|------|
| VBUS (+5V) | Zasilanie |
| GND | Masa |
| D+ | Dane różnicowe (dodatnia) |
| D– | Dane różnicowe (ujemna) |

### 13.3 Typy transferów USB

| Typ | Cechy | Zastosowanie |
|-----|-------|-------------|
| **Control** | Mały, gwarantowany, dwukierunkowy | Konfiguracja urządzenia |
| **Bulk** | Duże transfery, bez gwarancji czasu | Pamięci USB, drukarki |
| **Interrupt** | Regularne pollowanie przez host | Klawiatura, mysz |
| **Isochronous** | Gwarantowany czas, bez retransmisji | Audio, video |

### 13.4 Struktura transakcji USB

Każda transakcja składa się z **3 pakietów**:

```
┌─────────┐   ┌──────┐   ┌────────────┐
│  TOKEN  │ → │ DATA │ → │ HANDSHAKE  │
└─────────┘   └──────┘   └────────────┘
 Typ + adres   Właściwe    ACK/NAK/STALL
               dane
```

- **Token** — typ transakcji (IN/OUT/SETUP), adres urządzenia, numer endpointu
- **Data** — właściwy ładunek danych
- **Handshake** — potwierdzenie: ACK (sukces), NAK (brak danych/zajęty), STALL (błąd)

### 13.5 Wersje USB

| Wersja | Nazwa | Prędkość |
|--------|-------|---------|
| USB 1.1 | Full Speed | 12 Mb/s |
| USB 2.0 | High Speed | 480 Mb/s |
| USB 3.0 / 3.1 Gen 1 | SuperSpeed | 5 Gb/s |
| USB 3.1 Gen 2 | SuperSpeed+ | 10 Gb/s |
| USB 4 | — | 40 Gb/s |

---

## 14. Pamięci masowe — HDD, SSD, optyczne

### 14.1 HDD (Hard Disk Drive) — nośnik magnetyczny

- Dane zapisywane **magnetycznie** na obracających się talerzach (platters) pokrytych materiałem magnetycznym
- Głowica odczytu/zapisu unoszona jest nad powierzchnią na mikrometrowej poduszce powietrznej
- Dostęp: **losowy** (ale z opóźnieniem mechanicznym — seek time, rotational latency)
- Parametry: pojemność, prędkość obrotów (5400, 7200, 10000 RPM), cache

### 14.2 SSD (Solid State Drive) — pamięć FLASH NAND

- Dane zapisywane elektronicznie w komórkach **tranzystorów FGMOS** (Floating Gate)
- Brak ruchomych części — odporny na wstrząsy
- Dostęp: **losowy**, bardzo niskie opóźnienia (~0,1 ms vs ~5 ms HDD)
- Typy komórek NAND:
  - **SLC** (Single Level Cell) — 1 bit/komórkę — najtrwalsze, najdroższe
  - **MLC** (Multi Level Cell) — 2 bity/komórkę
  - **TLC** (Triple Level Cell) — 3 bity/komórkę
  - **QLC** (Quad Level Cell) — 4 bity/komórkę — najtańsze, najmniej trwałe
- Ograniczona liczba cykli zapisu (P/E cycles)

### 14.3 Pamięć FLASH NOR vs NAND

| Cecha | NOR Flash | NAND Flash |
|-------|-----------|------------|
| Dostęp do odczytu | Losowy (bajt po bajcie) | Stronnicowy (blokowy) |
| Prędkość zapisu | Wolna | Szybka |
| Gęstość | Niska (większe komórki) | Wysoka (mniejsze komórki) |
| Koszt | Drogi | Tani |
| Zastosowanie | BIOS/UEFI, EEPROM, firmware | SSD, pendrive, karty SD |
| Execute-in-Place (XiP) | Tak | Nie |

### 14.4 Nośniki optyczne — CD, DVD, Blu-ray

- Dane zapisywane jako **mikrowgłębienia (pits)** i **płaskie obszary (lands)** na spiralnej ścieżce
- Odczyt: wiązka lasera odbita od powierzchni — różna refleksyjność pit i land
- Zapis: laser o wyższej mocy topi warstwę barwnika (CD-R/DVD-R) lub zmienia stan fazowy warstwy (CD-RW/DVD-RW)

| Nośnik | Pojemność | Długość fali lasera |
|--------|-----------|-------------------|
| **CD** | 700 MB | 780 nm (podczerwień) |
| **DVD** | 4,7 GB (SL) | 650 nm (czerwień) |
| **Blu-ray** | 25 GB (SL) | 405 nm (fiolet) |

---

## 15. Monitory — CRT, LCD, Plazma

### 15.1 CRT (Cathode Ray Tube)

**Zasada działania:**
1. Działo elektronowe (cathode) emituje strumień elektronów
2. Cewki odchylające kierują wiązkę na właściwy punkt ekranu
3. Elektrony uderzają w fosfor na wewnętrznej powierzchni ekranu → emisja światła
4. Obraz budowany przez **skanowanie** — wiązka przesuwa się poziomo (linia po linii), następnie wraca i przesuwa się o jedną linię (flyback)

**Parametry:**
- Częstotliwość pozioma (line rate) — liczba linii na sekundę
- Częstotliwość pionowa (frame rate) — liczba klatek na sekundę (Hz)
- Rozdzielczość ograniczona przez szerokość wiązki i pasmo wzmacniaczy

**Wady:** duże, ciężkie, wysokie napięcia (kilkanaście kV), promieniowanie X

### 15.2 LCD (Liquid Crystal Display)

**Zasada działania:**
1. Źródłem światła jest **podświetlenie** (backlight) — fluorescencyjne (CCFL) lub LED
2. Światło przechodzi przez **polaryzator**
3. **Kryształy ciekłe** modulują polaryzację światła pod wpływem pola elektrycznego
4. Drugi **analizator (polaryzator)** przepuszcza lub blokuje zmodulowane światło
5. Filtry kolorów (RGB) tworzą kolor każdego subpiksela

**Typy matryc LCD:**
- **TN (Twisted Nematic)** — tanie, szybkie, słabe kąty widzenia
- **IPS (In-Plane Switching)** — dobre kąty widzenia, dobra reprodukcja kolorów
- **VA (Vertical Alignment)** — wysoki kontrast, średnie kąty widzenia

### 15.3 Plazma (PDP — Plasma Display Panel)

**Zasada działania:**
1. Ekran podzielony na miliony małych **komórek** wypełnionych mieszaniną gazów (Ne, Xe)
2. Impulsy elektryczne jonizują gaz, tworząc **plazmę** emitującą promieniowanie UV
3. Promieniowanie UV wzbudza **fosfor** (R, G, B) emitujący widzialne światło

**Cechy:**
- Doskonały kontrast (niemal idealny czarny — komórka po prostu nie świeci)
- Duże kąty widzenia
- Większe zużycie energii niż LCD
- Problem wypalania ekranu (burn-in) przy statycznych obrazach
- Produkowane głównie w dużych rozmiarach (42" i więcej)
- Technologia wycofana z rynku konsumenckiego ok. 2014–2015

---

## Zestawienie: ważne liczby i adresy

| Wartość | Znaczenie |
|---------|-----------|
| **INT 10H** | Przerwanie BIOS — usługi wideo (karta graficzna) |
| **INT 16H** | Przerwanie BIOS — usługi klawiatury |
| **INT 21H** | Przerwanie DOS — usługi systemu operacyjnego |
| **AH** | Rejestr numeru funkcji w wywołaniach INT BIOS |
| **Port 60H** | Dane klawiatury (scan code / polecenia do klawiatury) |
| **Port 64H** | Status i polecenia do kontrolera klawiatury 8042 |
| **0040H:0017H** | BDA — bajt stanu klawiszy Lock i modyfikujących |
| **2 bajty/znak** | Tryb tekstowy (bajt ASCII + bajt atrybutu) |
| **64 000 bajtów** | Tryb graficzny 320×200×256 (1 bajt/piksel) |
| **32 bajty / 16 słów** | Rozmiar bufora klawiatury BIOS |
| **11 bitów** | Rozmiar ramki PS/2 (1 start + 8 data + 1 parity + 1 stop) |

---

*Opracowanie na podstawie materiałów egzaminacyjnych — Interfejsy Komputerów Cyfrowych, WAT*
