# task-tamagotchi

A task-completion Tamagotchi. Check off a task in the web dashboard, the signal goes out over Bluetooth Low Energy, and a little pet on an OLED screen reacts and grows — egg → child → adult — with hand-drawn, frame-by-frame sprite animations.

## How it works

```
[ web dashboard ]  --(BLE write: "TASK_DONE")-->  [ ESP32 + SSD1306 OLED ]
   add/check tasks                                   pet animates + evolves
```

1. Tasks are added and checked off in the browser-based dashboard.
2. Each completed task sends a `TASK_DONE` packet over Web Bluetooth to the ESP32.
3. The firmware advances the pet's task counter and plays a short celebration animation.
4. Once enough tasks are completed, the pet evolves to its next growth stage, each with its own hand-drawn 32x32 sprite (scaled 2x on screen).

## Hardware

- ESP32 dev board
- SSD1306 OLED, 128x64, SPI wiring
  - `SCK` → GPIO 18
  - `MOSI` → GPIO 23
  - `DC` → GPIO 4
  - `RESET` → GPIO 14
  - `CS` → GPIO 5
- Enclosure: hand-built, Y2K-inspired (clear case + polymer clay trim)

## Software

- **Firmware** (`/firmware`): Arduino sketch (`.ino`) using `Adafruit_SSD1306`, `Adafruit_GFX`, and the ESP32 `BLEDevice` stack. Advertises as `Tamagotchi_Pet` over BLE.
- **Web dashboard** (`/web`): a single HTML file using the [Web Bluetooth API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Bluetooth_API) to connect directly from the browser — no backend, no app install. Add tasks, check them off, watch the BLE packets go out in a live terminal-style log.

### Running the dashboard

Open `web/tamagotchi-task-app-dark.html` in a Bluetooth-capable browser (Chrome on desktop or Android — Web Bluetooth isn't supported in Safari or Firefox). Click "Connect to Tamagotchi" and select `Tamagotchi_Pet` from the pairing prompt.

### Flashing the firmware

Open `firmware/tamagotchi_pet.ino` in the Arduino IDE with the ESP32 board package installed. Install the `Adafruit_SSD1306` and `Adafruit_GFX` libraries via the Library Manager, then flash to your ESP32.

## Known limitations

- The BLE characteristic has no pairing/authentication — anyone in range who knows the service UUID can send a `TASK_DONE` packet. Harmless for a desk toy, but worth knowing if you build on this.
- Growth stages and animation frames are intentionally minimal (2 frames per stage) — easy to extend with more frames or stages.

## License

No license — all rights reserved by default. Reach out if you'd like to use this for something.

---

*reyaveynwrites // desk shrine systems*
