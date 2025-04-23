OLED Simon Game on Arduino

This is an enhanced version of the classic **Simon memory game**, built with an **Arduino**, an **SSD1306 OLED display**, and a **buzzer**. The game challenges players to remember and repeat increasingly long sequences of lights and sounds.

🎮 Features

- OLED display for level and game state
- Adjustable game speed with level progression
- Sound feedback using buzzer:
  - Start tone
  - Button press tone
  - Game over melody
- Randomly generated sequences up to 99 levels
- Easy start via pushbutton
- Input via 4 buttons mapped to analog pins

🧠 How It Works

- Press the **start button** (connected to pin `6`) to begin the game.
- Watch the **sequence of lights and sounds**.
- Repeat the sequence using your input buttons (A0 to A3).
- Correct inputs increase the level and speed up the game.
- Wrong inputs trigger a game over tone and reset the game.

🧰 Hardware Requirements

- Arduino (Uno/Nano/Pro Mini, etc.)
- SSD1306 OLED display (I2C)
- 4 push buttons (connected to A0–A3)
- 4 LEDs (connected to D2–D5)
- 1 Start button (connected to D6)
- Buzzer (connected to D12)
- 220Ω resistors for LEDs
- Pull-down resistors for buttons (or internal pull-ups)

🔌 Pin Configuration

| Component      | Pin         |
|----------------|-------------|
| LED 1–4        | D2–D5       |
| Buttons 1–4    | A0–A3       |
| Start Button   | D6          |
| Buzzer         | D12         |
| OLED (I2C)     | A4 (SDA), A5 (SCL) |

> The game uses a pull-down setup, so LOW input indicates button press.

## 🛠️ Libraries Required

Install the following libraries via the Arduino Library Manager:

- `Adafruit GFX`
- `Adafruit SSD1306`

🚀 Getting Started

1. Connect all components as described above.
2. Upload the sketch to your Arduino.
3. Press the start button to begin.
4. Match the LED and sound sequences as the levels progress!

📂 Code Overview

Main features:
- `generate_sequence()` creates a full game sequence.
- `show_sequence()` displays and plays the sequence.
- `get_sequence()` reads player input and checks correctness.
- `wrong_sequence()` and `right_sequence()` handle success or failure.
- OLED is used to display the current level or reset screen.

---

Enjoy your retro memory workout with modern visuals and sounds!
