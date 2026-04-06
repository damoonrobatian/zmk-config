# ZMK Firmware Setup — Ferris Sweep (Cradio)

This documents the steps taken to set up ZMK firmware for a Ferris Sweep split keyboard with a ProMicro NRF52840 controller.

## Hardware

- **Keyboard:** Ferris Sweep (Cradio)
- **Controller:** ProMicro NRF52840
- **Firmware:** ZMK v0.3

---

## 1. Prerequisites

### Install Git

```bash
git --version
```

If not installed, get it from https://git-scm.com/install/

### Install uv (Python package manager)

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

Close and reopen the terminal, then verify:

```bash
uv --version
```

### Install ZMK CLI

```bash
uv tool install zmk
```

Verify:

```bash
zmk --version
```

---

## 2. Create the zmk-config Repository

Navigate to where you want the config folder to live (e.g., ~/Dropbox), then run:

```bash
cd <path/to/where/you/want/install/local/git/repo>
zmk init
```

Answer the prompts as follows:

- **Create or clone?** → Select **"Create a new ZMK config repo"**
- A browser window will open → log into GitHub if needed → click the green **"Create repository"** button
- Once created, click the green **"<> Code"** button → copy the HTTPS URL → paste it back in the terminal
- **Directory name?** → Press Enter to accept the default `zmk-config`
- **Add a keyboard now?** → Type `y` and press Enter
- **Select a keyboard:** → Type `cradio` to search → select **cradio (Cradio/Sweep)**
- **Select a controller:** → Select **nice_nano_v2**

---

## 3. Edit the Keymap

Open the keymap file:

```bash
zmk code cradio
```

The keymap is located at `config/cradio.keymap`. Edit it to customize your layout.

### Layer Overview

The default keymap has 4 layers:

| Layer | How to activate | Contents |
|-------|----------------|----------|
| 0 — Base | Always active | QWERTY + home row mods |
| 1 — Numbers & Nav | Hold right thumb key (Backspace) | Numbers 1–9, 0 + arrow keys, Home, End, PgUp, PgDn |
| 2 — Symbols | Hold left thumb key (Tab) | Brackets, punctuation, special characters |
| 3 — Bluetooth | Hold both thumb keys together | Bluetooth profiles, clear pairing, reset, bootloader |

### Home Row Mods

The home row keys double as modifiers when held:

| Key | Tap | Hold |
|-----|-----|------|
| A | A | Left Shift |
| S | S | Left Alt |
| D | D | Left Ctrl |
| F | F | Left GUI |
| J | J | Right GUI |
| K | K | Right Ctrl |
| L | L | Right Alt |
| ' | ' | Right Shift |

---

## 4. Build the Firmware

Commit and push changes to GitHub to trigger a build:

```bash
zmk cd
git add .
git commit -m "your message here"
git push
```

Then open the Actions tab in your GitHub repo, or run:

```bash
zmk download
```

Wait for the build to complete (green checkmark), then download the `firmware.zip` artifact. Extract it — you will find two files:

- `cradio_left-nice_nano_v2-zmk.uf2`
- `cradio_right-nice_nano_v2-zmk.uf2`

---

## 5. Flash the Firmware

### Enter Bootloader Mode

1. Plug the keyboard half into your computer via USB
2. Turn it on
3. **Double-tap the RESET button** on the PCB (labeled "RESET")
4. A drive called **NICENANO** will appear on your computer

### Flash Each Half

1. Drag and drop `cradio_left-nice_nano_v2-zmk.uf2` onto the NICENANO drive
2. The drive will disappear automatically — flashing is complete
3. Repeat for the right half using `cradio_right-nice_nano_v2-zmk.uf2`

> **Note:** For subsequent flashes, only the left (central) half needs to be reflashed if you are only updating the keymap.

---

## 6. Pairing the Keyboard Halves

The two halves pair automatically over Bluetooth when both are turned on. No manual pairing is needed.

---

## 7. Connecting via Bluetooth

1. Make sure both halves are turned on
2. Open Bluetooth settings on your computer
3. The keyboard will appear as **cradio**
4. Click to connect

### Clearing Bluetooth Pairing

If you need to clear the pairing (e.g. to connect to a new device):

1. Activate Layer 3 by holding both thumb keys together
2. Press the **V** key (BT_CLR) on the left half
3. Scan for the keyboard again on your device
