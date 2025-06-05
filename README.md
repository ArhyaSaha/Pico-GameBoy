<div align="center">

# 👾 Pico-GameBoy

A fork of [`youmaketech/pico-gb`](https://github.com/youmaketech/pico-gb)  
Now with **full-screen scaling** for better display utilization on ILI9225 screens.  
Precompiled `.uf2` releases available for quick flashing.

</div>

---

### 🕹️ About This Project

This is a personal fork of the Game Boy emulator for the Raspberry Pi Pico. The key enhancement in this version is the **implementation of screen scaling** to make better use of display resolutions of the ILI9225 176\*220 display.

---

### 📈 Why Scaling Was Added

| Reason                           | Description                                                                                                                                                                                                      |
| -------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 🖥️ Full-Screen Experience        | The original Game Boy resolution is 160×144 pixels, which was preserved in `youmaketech/pico-gb`. On larger or higher-resolution screens, this resulted in significant empty space, especially around the edges. |
| 📐 Better Use of Display Space   | The empty borders felt like wasted potential. Scaling was introduced to **maximize screen usage** and eliminate unused areas, making the emulator feel more immersive and complete.                              |
| 👀 Improved Visibility           | Scaling up the image makes the graphics easier to see on small displays, improving readability and comfort during gameplay.                                                                                      |
| 🔧 Optimized for ILI9225 Display | The scaling fills more of the ILI9225’s 176×220 screen, using the available resolution more effectively. While this version is tailored for ILI9225, future updates could expand support for other displays.     |

---

### 🧠 Scaling Algorithm Logic

#### The Scaling Challenge

- Game Boy: 160×144 pixels
- Your LCD: 220×176 pixels
- Scale factors: 220/160 = 1.375 (horizontal), 176/144 ≈ 1.222 (vertical)

#### Horizontal Scaling (1.375x) -

For each LCD pixel position `x` (0 to 219), we need to find which Game Boy pixel to use:

```
gb_x = (x * 160) / 220
```

##### Example mapping:

- LCD pixel 0 → GB pixel (0×160)/220 = 0
- LCD pixel 1 → GB pixel (1×160)/220 = 0 (same pixel repeated)
- LCD pixel 2 → GB pixel (2×160)/220 = 1
- LCD pixel 3 → GB pixel (3×160)/220 = 2

This creates a pattern where some Game Boy pixels are shown once, others twice.

#### Vertical Scaling (1.222x) -

The key insight is: _every GameBoy line maps to a range of LCD lines_.

```c
uint8_t start_lcd_line = (line * 176) / 144;
uint8_t end_lcd_line = ((line + 1) * 176) / 144;
```

##### How line repetition is decided:

| GB Line | start_lcd_line | end_lcd_line  | LCD Lines | Repetitions |
| ------- | -------------- | ------------- | --------- | ----------- |
| 0       | (0×176)/144=0  | (1×176)/144=1 | 0         | 1 time      |
| 1       | (1×176)/144=1  | (2×176)/144=2 | 1         | 1 time      |
| 2       | (2×176)/144=2  | (3×176)/144=3 | 2         | 1 time      |
| 3       | (3×176)/144=3  | (4×176)/144=4 | 3         | 1 time      |
| 4       | (4×176)/144=4  | (5×176)/144=6 | 4,5       | 2 times     |
| 5       | (5×176)/144=6  | (6×176)/144=7 | 6         | 1 time      |

##### Visual Example

```
Game Boy (144 lines)    LCD (176 lines)
Line 0    ──────────►   Line 0
Line 1    ──────────►   Line 1
Line 2    ──────────►   Line 2
Line 3    ──────────►   Line 3
Line 4    ──────────►   Line 4
          ──────────►   Line 5 (Line 4 repeated)
Line 5    ──────────►   Line 6
Line 6    ──────────►   Line 7
Line 7    ──────────►   Line 8
Line 8    ──────────►   Line 9
          ──────────►   Line 10 (Line 8 repeated)
...
```

---

### 🚀 Installation / Flashing

Download the latest `.uf2` release from the [Releases](https://github.com/ArhyaSaha/Pico-GameBoy/releases) section and flash it to your Pico:

1. Hold the **BOOTSEL** button on the Pico and plug it into your computer.
2. It should appear as a USB drive.
3. Drag and drop the downloaded `.uf2` file into the drive.
4. Pico will reboot and run the emulator.

---

### 📥 Releases

- [Download the Latest `.uf2`](https://github.com/ArhyaSaha/Pico-GameBoy/releases/latest)

> Releases are precompiled and ready to flash. No build setup needed.

---

### 💡 Future Plans

- Better Menu Design.
- Pull Requests are welcome.

Let's Make This The BEST OPEN-SOURCE MAKE YOUR OWN GAME BOY!

---

### 📄 License

This project follows the license from the original [`youmaketech/pico-gb`](https://github.com/youmaketech/pico-gb) repository.
