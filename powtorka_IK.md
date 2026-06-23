# 📚 Powtórka — Interfejsy Komputerów Cyfrowych (IK)
### Wojskowa Akademia Techniczna | Opracowanie na podstawie egzamin1.md, egzamin2.md, egzamin3.md

---

> **WAŻNE:** Każda poprawna odpowiedź oznaczona jest symbolem ✅. Pytania pogrupowane tematycznie dla łatwiejszego zapamiętania.

---

## Spis treści

1. [Architektura systemu wejścia-wyjścia](#1-architektura-systemu-wejściawyjścia)
2. [Transmisja danych — podstawy](#2-transmisja-danych--podstawy)
3. [Klawiatura i interfejs PS/2](#3-klawiatura-i-interfejs-ps2)
4. [Karta graficzna i tryby video](#4-karta-graficzna-i-tryby-video)
5. [Drukarki i mechanizmy druku](#5-drukarki-i-mechanizmy-druku)
6. [Urządzenia wejścia — skaner, digitizer, ploter](#6-urządzenia-wejścia--skaner-digitizer-ploter)
7. [Interfejsy graficzne (VGA, DVI, HDMI, DP)](#7-interfejsy-graficzne-vga-dvi-hdmi-dp)
8. [Magistrale systemowe — ISA, PCI, PCI Express](#8-magistrale-systemowe--isa-pci-pci-express)
9. [Transmisja PIO i DMA](#9-transmisja-pio-i-dma)
10. [Port równoległy LPT — SPP, EPP, ECP](#10-port-równoległy-lpt--spp-epp-ecp)
11. [USB](#11-usb)
12. [Pamięci masowe — HDD, SSD, CD, DVD](#12-pamięci-masowe--hdd-ssd-cd-dvd)
13. [Monitory — CRT, LCD, Plazma](#13-monitory--crt-lcd-plazma)
14. [Bit parzystości / nieparzystości](#14-bit-parzystości--nieparzystości)
15. [Szybka tabela odpowiedzi](#15-szybka-tabela-odpowiedzi)

---

## 1. Architektura systemu wejścia-wyjścia

### Czym jest interfejs?

**Pytanie:** Zespół ustalonych reguł oraz środków technicznych łączenia komputera z urządzeniami zewnętrznymi to:
- a. urządzenie wejścia-wyjścia
- b. adapter
- c. moduł wejścia-wyjścia
- **d. interfejs ✅**

> **Wyjaśnienie:** Interfejs to zbiór reguł (protokołów) + środków sprzętowych umożliwiających połączenie komputera z urządzeniem zewnętrznym. Definiuje sposób komunikacji — sygnały, timing, format danych.

---

### Czym jest moduł wejścia-wyjścia?

**Pytanie:** Element odpowiedzialny za sterowanie wieloma urządzeniami zewnętrznymi. Realizuje następujące funkcje:
- sterowanie i taktowanie komunikacją z procesorem,
- sterowanie komunikacją z urządzeniami,
- buforowanie danych,
- wykrywanie błędów.

- a. interfejs
- b. urządzenie wejścia-wyjścia
- **c. moduł wejścia-wyjścia ✅**

> **Wyjaśnienie:** Moduł W/W (I/O module) to pośrednik między procesorem a urządzeniami. Buforuje dane, zarządza przerwaniami, wykrywa błędy, taktuje transmisję.

---

### Bloki funkcjonalne urządzenia wejścia-wyjścia

**Pytanie:** Element o blokach funkcjonalnych: układy logiczne sterowania, bufor, przetwornik — to:
- a. moduł wejścia-wyjścia
- **b. urządzenie wejścia-wyjścia ✅**
- c. interfejs

> **Wyjaśnienie:** Urządzenie W/W zawiera: przetwornik (np. głowicę HDD, głowicę drukarki), bufor (pamięć tymczasowa) i układy sterowania (logika lokalna).

---

### Rozkazy IN i OUT

**Pytanie:** Rozkaz `IN AL, nrp` oznacza:
- **a. rozkaz typu czytaj, przesłanie z portu nrp (adres urządzenia zewnętrznego) do rejestru AL procesora ✅**
- b. rozkaz typu czytaj, przesłanie z rejestru AL do portu nrp
- c. rozkaz typu pisz, przesłanie z portu nrp do rejestru AL
- d. rozkaz typu pisz, przesłanie z rejestru AL do portu nrp

**Pytanie:** Rozkaz `OUT nrp, AL` oznacza:
- **a. rozkaz typu pisz, przesłanie rejestru AL procesora do portu nrp oznaczającego adres urządzenia zewnętrznego ✅**

> **Mnemonika:**
> - `IN`  = wejście do procesora (czytaj z urządzenia → AL)
> - `OUT` = wyjście z procesora (pisz z AL → urządzenia)

---

## 2. Transmisja danych — podstawy

### Transmisja równoległa

**Pytanie:** Transmisja równoległa polega na przesyłaniu porcji informacji wieloma liniami danych. Liczebność linii danych zależy od przyjętej długości słowa:
- **Prawda ✅**

**Pytanie:** Do realizacji transmisji równoległej wymagane jest połączenie urządzeń tyloma liniami danych ile jest bitów w przesłanym słowie (np. 8, 16):
- **Prawda ✅**

> **Wyjaśnienie:** W transmisji równoległej każdy bit słowa ma własną linię danych. Dla 8-bit = 8 linii, dla 16-bit = 16 linii. Przykład: stary port LPT (Centronics).

---

### Transmisja szeregowa synchroniczna

**Definicja:** Transmisja szeregowa **synchroniczna** to transmisja, w której:
- **✅ występuje linia sygnału zegarowego** — dane synchronizowane są zewnętrznym sygnałem CLK

> Przykłady: SPI, I²C, PS/2

---

### Transmisja szeregowa izochroniczna

**Pytanie:** Transmisja szeregowa **izochroniczna** to transmisja, w której:
- a. występuje linia sygnału zegarowego
- **b. gwarantuje przesyłanie informacji w regularnych przedziałach czasu ✅**
- c. występuje linia sygnału zegarowego i linia danych
- d. występuje linia danych

> **Wyjaśnienie:** Izochroniczna = dane przesyłane w stałych, regularnych interwałach (np. audio/video w USB). Gwarantuje QoS czasowy.

---

## 3. Klawiatura i interfejs PS/2

### Linie interfejsów

**Pytanie:** Linie VCC (+5V), GND, DATA+, DATA– ma interfejs:
- a. PS/2
- **b. USB ✅**
- c. RS-232

> **Wyjaśnienie:**
> - PS/2 ma linie: VCC (+5V), GND, **DATA**, **CLOCK** (4 linie, sygnał zegarowy)
> - USB ma linie: VCC (+5V), GND, **DATA+**, **DATA–** (sygnał różnicowy)

---

### Przerwania BIOS

**Pytanie:** Przerwanie (usługa systemu BIOS) dla **klawiatury** ma numer:
- **a. 16H ✅**
- b. 21H
- c. 10H

**Pytanie:** Numer przerwania BIOS do **karty graficznej**:
- **a. 10H ✅**
- b. 16H
- c. 21H

> **Mnemonika:**
> - INT **10H** = karta graficzna (wideo)
> - INT **16H** = klawiatura
> - INT **21H** = DOS (system operacyjny, usługi systemowe)

---

### Rejestr funkcji BIOS

**Pytanie:** Numer funkcji dla przerwania BIOS klawiatury ustawia się w rejestrze:
- **a. AH ✅**
- b. AL
- c. AX

---

### Znaczenie 00H w programie BIOS

**Pytanie:**
```asm
MOV AH, 00H
INT 16H
```
`00H` oznacza:
- **a. numer funkcji ✅**
- b. argument funkcji
- c. numer przerwania

> **Wyjaśnienie:** Konwencja BIOS: numer funkcji w **AH**, argument/wynik w **AL**.
> Funkcja 00H INT 16H = czekaj na wciśnięcie klawisza i zwróć scan code (AH) + ASCII (AL).

---

### Porty klawiatury AT/PS2

| Port | Adres | Opis |
|------|-------|------|
| Dane klawiatury | **60H** | Odczyt scan code / wysyłanie poleceń do klawiatury |
| Status/Polecenia | **64H** | Odczyt statusu kontrolera / wysyłanie poleceń do kontrolera |

**Pytanie:** W rozkazie `OUT 60H, AL` — `60H` oznacza:
- a. adres portu statusu klawiatury
- **b. adres portu danych klawiatury ✅**
- c. daną lub rozkaz przesyłany do klawiatury

**Pytanie:** W rozkazie `OUT 64H, AL` — `64H` oznacza:
- **a. adres portu statusu klawiatury ✅**
- b. adres portu danych klawiatury
- c. daną lub rozkaz przesyłany do klawiatury

**Pytanie:** W rozkazie `IN AL, 64H` — `AL` oznacza:
- **odczytany status klawiatury ✅**

---

### Bufor klawiatury

**Pytanie:** Bufor klawiatury zawiera obszar o rozmiarze 32 bajtów, złożony z 16 2-bajtowych słów. Każde słowo zawiera:
- a. szybkość autorepetycji
- **b. kod ASCII klawisza ✅**
- **c. scan code klawisza ✅**
- d. czas opóźnienia powtarzania klawisza

> **Wyjaśnienie:** Każde słowo (2 bajty) w buforze klawiatury zawiera: **bajt scan code** + **bajt ASCII**. Bufor cykliczny (ring buffer) przechowuje 16 wciśnięć klawiszy.

---

### Adres pamięci stanu klawiszy blokujących (BDA)

**Pytanie:** Stany trybów Num Lock, Caps Lock, Scroll Lock jest zapisany w bajcie zapisanym w pamięci o adresie logicznym:
- **a. 0040H:0017H ✅**
- b. 0040H:0018H
- c. 0040H:001CH
- d. 0040H:001AH

> **Wyjaśnienie:** Obszar danych BIOS (BIOS Data Area, BDA) zaczyna się od segmentu `0040H`. Pod offsetem `0017H` = bajt stanu klawiszy modyfikujących (Shift, Ctrl, Alt, Caps Lock, Num Lock, Scroll Lock).

---

### Autorepetycja

**Pytanie:** Autorepetycja oznacza czas opóźnienia, po jakim będą pojawiały się kolejne znaki na ekranie klawisza, który został naciśnięty i nie został jeszcze zwolniony:
- **Fałsz ✅**

> **Wyjaśnienie:** Autorepetycja (Typematic Rate) = **szybkość powtarzania znaku** (ilość znaków/sekundę). Czas opóźnienia (Typematic Delay) to osobny parametr wyrażany w ms.

**Pytanie:** Szybkość autorepetycji podaje się w ilości znaków na sekundę:
- **Prawda ✅**

---

## 4. Karta graficzna i tryby video

### Tryb tekstowy 80×25×16

**Pytanie:** Tryb 80×25×16 oznacza, że ekran ma wymiary:
- a. znaki w 80 kolorach, 25 kolumn, 16 wierszy
- b. 80 wierszy, 25 kolumn, znaki w 16 kolorach
- **c. 80 kolumn, 25 wierszy, znaki w 16 kolorach ✅**

**Pytanie:** W trybie tekstowym 80×25×16 do przechowywania znaku w pamięci ekranu potrzeba:
- **a. dwóch bajtów pamięci ✅**
- b. jednego bajtu pamięci
- c. czterech bajtów pamięci

> **Wyjaśnienie:** Każdy znak w trybie tekstowym zajmuje **2 bajty**:
> - Bajt 1: **kod ASCII** znaku
> - Bajt 2: **bajt atrybutu** (kolor znaku + kolor tła)
>
> Łącznie dla 80×25 ekranu: 80 × 25 × 2 = **4000 bajtów**

---

### Bajt atrybutu znaku

**Pytanie:** W trybie tekstowym 80×25×16, bajt atrybutu znaku zawiera:
- **a. kolor znaku ✅**
- b. scan code znaku
- c. kod ASCII znaku
- **d. kolor tła znaku ✅**

> **Struktura bajtu atrybutu (8 bitów):**
> ```
> Bit:  7    6 5 4    3 2 1 0
>       BL   R G B    I R G B
>       ^    ^^^^^^^  ^^^^^^^
>       miganie  kolor tła  kolor znaku (z intensity)
> ```

---

### Tryb graficzny 320×200×256

**Pytanie:** W trybie graficznym 320×200×256, do zapisania jednego ekranu w pamięci potrzeba:
- **a. 320 × 200 bajtów ✅**
- b. 4 × 320 × 200 bajtów
- c. 320 × 200 × 256 bajtów

> **Wyjaśnienie:** Tryb 320×200×256 = 320 kolumn × 200 wierszy × 256 kolorów.
> Każdy piksel = **1 bajt** (indeks do 256-kolorowej palety).
> Łącznie: **320 × 200 = 64 000 bajtów = 64 KB**

---

## 5. Drukarki i mechanizmy druku

### Typy mechanizmów druku

| Mechanizm | Zasada działania |
|-----------|-----------------|
| **Mozaikowy (igłowy)** | Igły uderzają przez taśmę barwiącą w papier. Druk uderzeniowy (impact). |
| **Termiczny** | Głowica grzeje termoczuły papier lub taśmę termotransferową. |
| **Natryskowy (inkjet)** | Krople atramentu wyrzucane na papier termicznie lub piezoelektrycznie. |
| **Laserowy** | Bęben ładowany elektrostatycznie, toner przyciągany i utrwalany ciepłem. |
| **Bębnowy** | Stare drukarki linijkowe z obracającym się bębnem z wypukłymi literami. |

---

### Języki sterowania drukarkami

**Pytanie:** Zestaw zdefiniowanych sekwencji sterujących drukarek **określa** język:
- a. PLC  b. LPC  c. PJL
- **d. PCL ✅**

**Pytanie:** Zestaw zdefiniowanych sekwencji sterujących drukarek służący między innymi do **współdzielenia drukarki**:
- a. PCL  b. LPL
- **c. PJL ✅**
- d. PLC

> **Wyjaśnienie:**
> - **PCL** (Printer Command Language) — język HP opisu strony, definiuje jak drukować tekst i grafikę
> - **PJL** (Printer Job Language) — zarządza kolejką zadań drukarki, współdzieleniem, ustawieniami między zadaniami

---

## 6. Urządzenia wejścia — skaner, digitizer, ploter

### Podsumowanie urządzeń

| Urządzenie | Funkcja | Typ |
|------------|---------|-----|
| **Skaner** | Digitalizuje istniejący obraz (zdjęcie, dokument) do postaci cyfrowej | Wejście |
| **Digitizer / Tablet graficzny** | Ręczne rysowanie rysikiem na płytce | Wejście |
| **Ploter (pisak XY)** | Rysowanie/grawerowanie na dużych formatach, kreślenie map | Wyjście |

---

**Pytanie:** Skaner to urządzenie, które:
- a. służy do pracy z dużymi płaskimi powierzchniami, mogące nanosić obrazy, wycinać wzory...
- b. umożliwia ręczne rysowanie rysunków za pomocą rysika
- **c. umożliwia przetworzenie statycznego obrazu rzeczywistego obiektu do postaci cyfrowej, w celu dalszej obróbki komputerowej ✅**

**Pytanie:** Digitizer to urządzenie, które:
- **umożliwia użytkownikowi ręczne rysowanie rysunków, pisma, animacji i grafiki za pomocą rysika, w podobny sposób jak osoba rysująca obrazy za pomocą ołówka i papieru ✅**

**Pytanie:** Ploter (pisak XY) to urządzenie, które:
- a. umożliwia ręczne rysowanie za pomocą rysika
- **b. służy do pracy z dużymi płaskimi powierzchniami, mogące nanosić obrazy, wycinać wzory, grawerować, kreślić mapy ✅**
- c. umożliwia przetworzenie statycznego obrazu do postaci cyfrowej

---

## 7. Interfejsy graficzne (VGA, DVI, HDMI, DP)

### Interfejsy analogowe vs cyfrowe

**Pytanie:** Wskaż interfejsy **analogowe**:
- **VGA ✅**

**Pytanie:** Wskaż interfejsy **cyfrowe**:
- **DP ✅**, **DVI ✅** (DVI-D lub DVI-I część cyfrowa), **HDMI ✅**

### Tabela interfejsów graficznych

| Interfejs | Typ sygnału | Uwagi |
|-----------|-------------|-------|
| **VGA** | Analogowy | D-Sub 15-pin, tylko obraz |
| **DVI** | Cyfrowy / Analogowy / Oba | DVI-D (cyfr.), DVI-A (analog.), DVI-I (oba) |
| **HDMI** | Cyfrowy | Obraz + dźwięk, HDCP, szeroko stosowany |
| **DP (DisplayPort)** | Cyfrowy | Obraz + dźwięk, HBR, Multi-Stream Transport |

---

## 8. Magistrale systemowe — ISA, PCI, PCI Express

### Rodzaje magistral

| Magistrala | Typ transmisji | Uwagi |
|------------|----------------|-------|
| **ISA** | Równoległa | Stara magistrala 8/16-bit, współdzielona szyna |
| **PCI** | Równoległa | 32/64-bit, Plug & Play, współdzielona |
| **PCI Express** | Szeregowa różnicowa | Lane'y (x1, x4, x8, x16), punkt-punkt |

---

### PCI Express — charakterystyka

**Pytanie:** Urządzenie podłączone do gniazd PCI Express:
- **a. komunikuje się za pośrednictwem jednej lub więcej par linii różnicowych, z których każda realizuje transmisję szeregową ✅**
- b. ... każda realizuje transmisję równoległą
- c. za pomocą linii: adresowej, danych, sterowania

> **Wyjaśnienie:** PCIe używa **transmisji szeregowej różnicowej**. Każdy "lane" (tor) składa się z pary TX+/TX– i RX+/RX–. Wiele lane'ów (x1, x4, x8, x16) daje wyższą przepustowość.

---

### Typy urządzeń PCIe

**Pytanie:** Typy urządzeń: Root Complex, Switch, Endpoint występują w:
- a. ISA
- **b. PCI Express ✅**
- c. PCI

> **Wyjaśnienie:**
> - **Root Complex** — most między CPU/RAM a siecią PCIe
> - **Switch** — przełącznik (rozgałęziacz) PCIe
> - **Endpoint** — docelowe urządzenie (karta graficzna, SSD NVMe, etc.)

---

## 9. Transmisja PIO i DMA

### PIO (Programmed I/O)

**Definicja:** Procesor aktywnie uczestniczy w każdej operacji transferu — czyta/zapisuje przez porty I/O.

| Typ | Opis |
|-----|------|
| **PIO pisz** | Procesor wysyła dane (z rejestrów) do urządzenia |
| **PIO czytaj** | Procesor odczytuje dane z urządzenia do rejestrów |

### DMA (Direct Memory Access)

**Definicja:** Kontroler DMA przesyła dane bezpośrednio między pamięcią a urządzeniem — **bez angażowania procesora** w każdy transfer (procesor inicjuje, DMA przesyła, na koniec przerwanie).

| Typ | Opis |
|-----|------|
| **DMA pisz** | Dane z pamięci → urządzenie |
| **DMA czytaj** | Dane z urządzenia → pamięć |

---

## 10. Port równoległy LPT — SPP, EPP, ECP

### Tryby transmisji LPT

| Tryb | Pełna nazwa | Kierunek | Charakterystyka |
|------|-------------|----------|-----------------|
| **SPP** | Standard Parallel Port | Jednostronny (komp → urząd.) | Klasyczny Centronics, 8 linii danych, wolny |
| **EPP** | Enhanced Parallel Port | Dwukierunkowy | Szybsza wersja, sprzętowy handshaking |
| **ECP** | Extended Capabilities Port | Dwukierunkowy | Kompresja RLE, DMA, FIFO, tryb forward/reverse |

> Pytania egzaminacyjne pokazują oscylogramy sygnałów i pytają o rozpoznanie trybu:
> - SPP: sygnały STROBE, BUSY, ACK; jednostronny
> - EPP: szybki handshake, cykle danych i adresów
> - ECP: sygnał ChannelAddress (CA), oddzielne tryby zapisu i odczytu

---

## 11. USB

### Pakiety USB

**Pytanie:** Standard transmisji, w którym występują pakiety: Token, Data, Handshake:
- a. PS/2
- **b. USB ✅**
- c. RS-232

> **Wyjaśnienie:** USB komunikuje się za pomocą **transakcji** składających się z 3 faz:
> 1. **Token** — określa typ transakcji (IN/OUT/SETUP) i adres/endpoint urządzenia
> 2. **Data** — przenosi właściwe dane
> 3. **Handshake** — potwierdzenie (ACK = sukces / NAK = brak danych / STALL = błąd)

### Linie USB

VCC (+5V), GND, **DATA+**, **DATA–** — sygnał różnicowy, odporny na zakłócenia.

---

## 12. Pamięci masowe — HDD, SSD, CD, DVD

### Nośniki pamięci — tabela

| Urządzenie | Typ nośnika |
|------------|-------------|
| **HDD** | Magnetyczny ✅ |
| **SSD** | FLASH typu NAND ✅ |
| **CD / DVD / Blu-ray** | Optyczny ✅ |
| **EEPROM / BIOS/UEFI** | FLASH typu NOR ✅ |
| **Pendrive / karta SD** | FLASH typu NAND |
| **RAM** | Półprzewodnikowy (ulotny) |

---

**W dyskach SSD:** FLASH typu **NAND** ✅  
**W HDD:** nośnik **magnetyczny** ✅  
**W EEPROM:** FLASH typu **NOR** ✅

> **Wyjaśnienie różnicy NAND vs NOR:**
> - **NAND Flash** — duże gęstości, sekwencyjny dostęp, tanie → SSD, pendrive, karty SD
> - **NOR Flash** — losowy dostęp do odczytu (Execute-in-Place), wolniejszy zapis → BIOS/UEFI, firmware mikrokontrolerów

---

## 13. Monitory — CRT, LCD, Plazma

### Zasada działania ekranów

| Typ | Zasada działania |
|-----|-----------------|
| **CRT** | Działo elektronowe; wiązka elektronów skanuje ekran pokryty fosforem linia po linii |
| **LCD** | Ciekłe kryształy modulują przepuszczanie podświetlenia (backlight) |
| **Plazmowy** | Komórki z gazem (plazma) emitują UV wzbudzające fosfor, który świeci |

> Pytania egzaminacyjne pokazują schemat budowy/zasady działania i pytają o rozpoznanie typu.

---

## 14. Bit parzystości / nieparzystości

### Obliczanie bitu nieparzystości

**Pytanie:** Bit nieparzystości dla ciągu **0011011** ma wartość:
- **a. jeden ✅**
- b. minus jeden
- c. zero

> **Obliczenie krok po kroku:**
> 1. Ciąg: `0 0 1 1 0 1 1`
> 2. Liczba jedynek = 1+1+1+1 = **4** (liczba parzysta)
> 3. Bit **nieparzystości (odd parity)** ustawiamy tak, by łączna liczba jedynek była **nieparzysta**
> 4. 4 jedynki → dodajemy bit = **1** → łącznie 5 jedynek (nieparzysta ✅)

---

### Definicja bitów parzystości

| Typ bitu | Zasada |
|----------|--------|
| **Parzystości (even parity)** | Bit = 1 gdy liczba jedynek w danych jest nieparzysta (uzupełniamy do parzystej) |
| **Nieparzystości (odd parity)** | Bit = 1 gdy liczba jedynek w danych jest parzysta (uzupełniamy do nieparzystej) |

> **Uwaga:** Interfejs PS/2 używa bitu **nieparzystości** (odd parity) w każdej ramce.

---

## 15. Szybka tabela odpowiedzi

| Pytanie | Poprawna odpowiedź |
|---------|-------------------|
| INT BIOS klawiatury | **16H** |
| INT BIOS karty graficznej | **10H** |
| INT DOS | **21H** |
| Rejestr numeru funkcji BIOS | **AH** |
| Port danych klawiatury | **60H** |
| Port statusu/poleceń klawiatury | **64H** |
| Adres stanu Lock klawiszy w BDA | **0040H:0017H** |
| Bufor klawiatury — rozmiar | **32 bajty (16 × 2-bajtowych słów)** |
| Bufor klawiatury — zawartość słowa | **ASCII + scan code** |
| Tryb tekstowy 80×25×16 — bajty/znak | **2 bajty** |
| Tryb graficzny 320×200×256 — bajty/ekran | **64 000 bajtów (320×200)** |
| Linie PS/2 | VCC (+5V), GND, DATA, CLOCK |
| Linie USB | VCC (+5V), GND, DATA+, DATA– |
| Interfejs analogowy | **VGA** |
| Interfejsy cyfrowe | **DVI, HDMI, DP (DisplayPort)** |
| PCIe — typ transmisji | **Szeregowa różnicowa** |
| PCIe — urządzenia | Root Complex, Switch, Endpoint |
| USB — pakiety | **Token, Data, Handshake** |
| SSD — pamięć | **FLASH NAND** |
| HDD — nośnik | **Magnetyczny** |
| EEPROM — nośnik | **FLASH NOR** |
| PCL — zastosowanie | Sekwencje sterujące drukarki |
| PJL — zastosowanie | Współdzielenie drukarki, zarządzanie zadaniami |
| Skaner | Digitalizacja istniejącego obrazu |
| Digitizer (tablet graficzny) | Rysowanie rysikiem |
| Ploter (pisak XY) | Rysowanie/grawerowanie na dużych formatach |
| `IN AL, nrp` | **Czytaj** (port nrp → AL) |
| `OUT nrp, AL` | **Pisz** (AL → port nrp) |
| Transmisja izochroniczna | Regularne, stałe przedziały czasu |
| Transmisja synchroniczna | Linia sygnału zegarowego |
| Bit nieparzystości dla 0011011 | **1** (4 jedynki → parzyste → dodaj 1) |
| Autorepetycja — jednostka | znaki/sekundę |
| Bajt atrybutu — zawiera | kolor znaku + kolor tła |
| Moduł W/W — funkcje | sterowanie, buforowanie, wykrywanie błędów |
| Interfejs — definicja | Reguły + środki techniczne łączenia z urządzeniami |
| Urządzenie W/W — bloki | Przetwornik + bufor + logika sterowania |

---

## Notatki dodatkowe

### Hierarchia systemu I/O

```
Procesor (CPU)
    |
    v
Moduł wejścia-wyjścia (I/O Module)
    |
    |-- Interfejs <--> Urządzenie zewnętrzne
    |
    v
Magistrala systemowa (ISA / PCI / PCIe)
```

### Porty klawiatury AT/PS2 — szczegóły

```
Port 60H — DANE klawiatury:
  Zapis:  wysyłanie poleceń DO klawiatury (np. ustaw LED)
  Odczyt: odbieranie scan code OD klawiatury

Port 64H — STATUS / POLECENIA:
  Zapis:  wysyłanie poleceń do kontrolera 8042
  Odczyt: bajt statusu kontrolera:
    bit 0 (OBF): Output Buffer Full — dane gotowe do odczytu z 60H
    bit 1 (IBF): Input Buffer Full — kontroler zajęty, nie pisz do 60H/64H
```

### Transmisja PS/2 — ramka

```
11 bitów:
[0][D0][D1][D2][D3][D4][D5][D6][D7][P][1]
 ^                                   ^   ^
 START                           PARITY STOP
 (zawsze 0)              (nieparzystość) (zawsze 1)
```

### Obliczanie bitu parzystości — algorytm

```
1. Policz liczbę jedynek w bajtach danych
2. Even parity:  bit = (suma jedynek nieparzysta) ? 1 : 0
3. Odd parity:   bit = (suma jedynek parzysta)    ? 1 : 0

Przykład: 0011011 → jedynki = 4 (parzysta)
  Even parity bit = 0  (4+0 = 4, parzysta ✅)
  Odd parity bit  = 1  (4+1 = 5, nieparzysta ✅)
```

---

*Opracowanie na podstawie materiałów egzaminacyjnych WAT — Interfejsy Komputerów Cyfrowych*  
*Pliki źródłowe: egzamin1.md, egzamin2.md, egzamin3.md*
