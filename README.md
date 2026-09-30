# electro-doom

DOOM na Raspberry Pi Pico z wyświetlaczem TFT 320×240 (ILI9341): gotowy obraz pamięci flash dla przykładu
„DOOM na Raspberry Pi Pico” w [Electro Notebook](https://geraldserafin.github.io/electro/).

## Szybko: pobierz gotowy plik

**[doom.bin](https://github.com/geraldserafin/electro-doom/releases/download/doom/doom.bin)** (~2 MB,
budowany automatycznie przez GitHub Actions z tego repo).

1. Otwórz w notatniku lekcję **Przykłady → DOOM na Raspberry Pi Pico**.
2. Upuść `doom.bin` na płytkę Pico (albo zaznacz ją i kliknij **Wgraj plik .uf2 / .bin**).
3. Uruchom symulację (⚡), kliknij schemat i graj z klawiatury:

| klawisz | co robi |
|---|---|
| **Enter** | menu i wybór (New Game) |
| **strzałki** | ruch |
| **Ctrl** | strzał |
| **spacja** | drzwi, przełączniki |
| **Esc** | menu |

## Zbuduj sam

```sh
git clone https://github.com/geraldserafin/electro-doom && cd electro-doom
./build.sh            # → doom.bin
```

Potrzebny jest `git` oraz jedno z dwóch:

- **nix**: skrypt sam weźmie kompilator, cmake itd.;
- **toolchain w PATH**: `arm-none-eabi-gcc` (13), `cmake`, `make`, kompilator C dla hosta, `python3`.
  Na Debianie/Ubuntu: `sudo apt install gcc-arm-none-eabi cmake build-essential python3`.
  Na macOS: `brew install --cask gcc-arm-embedded && brew install cmake`.

Skrypt pobiera źródła w przypiętych wersjach, nakłada łatkę i składa obraz: program od adresu 0, a od
`0x46000` skompresowany WAD. Budowa trwa kilka minut. `KEEP=1 ./build.sh` zostawia katalog roboczy.

## Co jest w środku

- [kilograham/rp2040-doom](https://github.com/kilograham/rp2040-doom): Chocolate Doom przerobiony tak, żeby
  zmieścił się w RP2040 (264 KB RAM, 2 MB flash), razem z `doom1.whx`, czyli skompresowanym shareware'owym
  DOOM1.WAD;
- [pondahai/rp2040-doom-ili9341](https://github.com/pondahai/rp2040-doom-ili9341): wyświetlacz ILI9341 na SPI
  zamiast VGA i osiem przycisków;
- [`pico-doom.patch`](pico-doom.patch): zmiany pod emulator. Klatki idą przez DMA do SPI tylko wtedy, gdy
  są gotowe, nie ma dźwięku ani efektu „melt”, a rdzenie czekają w uśpieniu (WFE).

Podłączenie: TFT SCK GP18, MOSI GP19, CS GP17, DC GP20, RESET GP21, podświetlenie GP22. Przyciski
(do GND, z podciąganiem): ↑ GP9, ↓ GP5, ← GP8, → GP6, strzał GP3, użyj GP2, Enter GP4, menu GP28.

## Licencje

Kod Dooma to GNU GPL v2 (Chocolate Doom), a kod RP2040 kilograhama to BSD-3. Łatka z tego repo jest na GPL v2
([LICENSE](LICENSE)). DOOM1.WAD w wersji shareware jest © id Software, które pozwala rozpowszechniać go
w niezmienionej formie.

---

*English:* Doom for a Raspberry Pi Pico with an ILI9341 display, as a flash image for
[Electro Notebook](https://geraldserafin.github.io/electro/)'s emulator. Download
[doom.bin](https://github.com/geraldserafin/electro-doom/releases/download/doom/doom.bin) from Releases,
or build it: `./build.sh` (needs git and nix, or arm-none-eabi-gcc 13 + cmake).
