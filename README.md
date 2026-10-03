# Sync Up!

A handheld rhythm game I made for the ECE319K video game project at UT Austin. The game runs on an MSPM0G3507 microcontroller and uses an ST7735 display, pushbuttons, a slide potentiometer, and a 5-bit DAC for audio.

Move the selector to the correct arrow lane and press the hit button when an incoming arrow reaches the target. Each successful hit adds to your score.

The in-game credits list Anish Agarwal and Venthan Dinesh.

## Gameplay

- Four arrow lanes: left, up, down, and right.
- Slide-pot control for moving between lanes.
- Two scripted levels with different arrow patterns and note mappings.
- Musical tones generated through a 5-bit DAC.
- English and French language options.
- An on-screen score and a game-over screen.

Choose a language at startup, then select Play or open Options to change the language or level. Level 1 is selected by default.

## Controls

| Input | Action |
| --- | --- |
| Slide potentiometer | Select an arrow lane during gameplay |
| PA24 button (`UP` in the code) | Hit the selected note; choose the `>>` menu option |
| PA25 button (`LFT` in the code) | Choose the `<<` menu option; clear arrows in the hit window without adding points |
| Any recognized directional button | Return to the menus from the game-over screen |

A hit scores when the selected lane matches an incoming arrow within the target window. Pressing the hit button also plays the tone assigned to that lane.

## Hardware

| Part | Use |
| --- | --- |
| TI MSPM0G3507 LaunchPad | Runs the game and handles peripheral I/O |
| ST7735 LCD | Menus, arrow sprites, and score display |
| Slide potentiometer | Analog lane selection through the ADC |
| Pushbuttons | Menu selection and gameplay input |
| 5-bit resistor DAC on PB0–PB4 | Audio output to the speaker circuit |

The switch driver configures PA24–PA27 as directional inputs. The exact LCD and ADC wiring depends on the ECE319K support drivers, which are referenced by the source but are not included in this repository.

## Code layout

| File | Purpose |
| --- | --- |
| `Lab9Main.c` / `Lab9Main.h` | Entry point, initialization, input sampling, and main loop |
| `Game.c` / `Game.h` | Lane selection, hit detection, levels, menu navigation, and note selection |
| `Graphics.c` / `Graphics.h` | Sprite drawing, menus, and score rendering |
| `images.h` | Bitmap assets |
| `Switch.c` / `Switch.h` | Button configuration and input reads |
| `Sound.c` / `Sound.h` | Waveform tables and SysTick-driven audio |
| `DAC5.c` | 5-bit DAC initialization and output |
| `LED.c` / `LED.h` | LED control helpers |

The processor is configured for 80 MHz. TimerG12 samples inputs at 30 Hz, while the main loop handles game logic and LCD drawing. Arrow movement also uses delays in the graphics routines. SysTick outputs waveform samples to the DAC.

## Building and running

This repository contains the game source files. It needs the original ECE319K MSPM0 project environment and support libraries to build.

1. Open the ECE319K Lab 9 project for the MSPM0G3507 in the course development environment.
2. Add these game files to the project, using `Lab9Main.c` as the application entry point.
3. Make the referenced course drivers available, including `ST7735`, `Clock`, `LaunchPad`, `TExaS`, `Timer`, `ADC1`, and `DAC5`, along with `SmallFont.h` and `sounds/sounds.h`.
4. Check the include paths and connect the display, buttons, potentiometer, and DAC circuit to match the drivers.
5. Build and flash the program to the LaunchPad, then choose a language and start playing.

The archive does not include the IDE project, startup code, linker configuration, or complete course driver folder. It cannot be built as a standalone desktop program.
