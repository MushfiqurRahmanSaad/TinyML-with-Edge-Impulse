# TinyML-with-Edge-Impulse
This repository will go through the basics of setting up your TinyML device with Edge Impulse. It includes instructions on installation for relevant CLI, troubleshooting and an easy guide to allow more people to experience TinyML. This repository is built in partnership with Wireless Research @ UKM
---
# 🚀 Edge Impulse on Linux with Arduino Nano 33 BLE Sense

A complete guide to installing the **Edge Impulse CLI** on Linux, installing and configuring the **Arduino CLI**, flashing your **Arduino Nano 33 BLE Sense**, selecting the correct serial port, and solving all common issues—including environment and dependency problems.

---

## 📦 Prerequisites

- Linux system (tested on Ubuntu 20.04, 22.04, Debian, Raspbian)
- **Python 3**
- **Node.js** (v16+ required, **v22 LTS recommended**)
- **npm**
- **git**
- **Arduino Nano 33 BLE Sense** (or Rev2)
- Data-capable USB cable (not charge-only)

---

## 🔧 1. Install Edge Impulse CLI

### Option A: Using NodeSource (preferred for correct Node version)
```bash
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -
sudo apt-get install -y nodejs
npm install -g edge-impulse-cli --force
````

### Option B: Using Install Script

```bash
git clone https://github.com/edgeimpulse/ei-install-scripts.git
cd ei-install-scripts
. ./install-linux.sh
```

### If Install Fails

* Clean npm cache and force install:

  ```bash
  npm cache clean --force
  npm install -g --force edge-impulse-cli
  ```
* Use a custom npm global path (to avoid `sudo`):

  ```bash
  mkdir ~/.npm-global
  npm config set prefix '~/.npm-global'
  echo 'export PATH=~/.npm-global/bin:$PATH' >> ~/.profile
  source ~/.profile
  npm install -g edge-impulse-cli
  ```

---

## 🔨 2. Install Arduino CLI (without `apt`)

```bash
curl -fsSL https://raw.githubusercontent.com/arduino/arduino-cli/master/install.sh | sh
sudo mv bin/arduino-cli /usr/local/bin/
arduino-cli version
```

Then install board support:

```bash
arduino-cli core update-index
arduino-cli core install arduino:mbed_nano
```

Confirm your board:

```bash
arduino-cli board list
```

Example output:

```
Port         Type              Board Name
/dev/ttyACM0 Serial Port (USB) Arduino Nano 33 BLE Sense
```

---

## 📲 3. Connecting and Flashing Arduino

1. Connect via USB and **double-tap** RESET to enter bootloader (LED pulses).
2. Run Edge Impulse daemon:

   ```bash
   edge-impulse-daemon
   ```
3. Or manually flash:

   ```bash
   cd ~/Downloads/ei-arduino-project
   arduino-cli compile --fqbn arduino:mbed_nano:nano33ble .
   arduino-cli upload -p /dev/ttyACM0 --fqbn arduino:mbed_nano:nano33ble .
   ```

⚠️ Ensure the `.ino` file name matches the project folder name to avoid mismatch errors.

---

## 🔍 4. Choosing the Correct Serial Port

List connected devices:

```bash
ls /dev/ttyACM*
```

Unplug/replug your board to confirm which one appears. Usually it’s `/dev/ttyACM0`.

When prompted by `edge-impulse-daemon`, pick the correct port.

---

## ⚠️ 5. Troubleshooting

### A. Edge Impulse CLI Installation

| Problem                            | Fix                                                                                    |
| ---------------------------------- | -------------------------------------------------------------------------------------- |
| `gyp ERR! EACCES` or build errors  | Use `npm cache clean --force` then `npm install -g --force edge-impulse-cli`.          |
| CLI not found after install        | Add npm global bin to PATH: `export PATH=$PATH:$(npm bin -g)`.                         |
| `node: command not found`          | Create a symlink: `sudo ln -s /usr/bin/nodejs /usr/bin/node`.                          |
| "Different Node.js version" errors | Reinstall CLI: `npm uninstall -g edge-impulse-cli && npm install -g edge-impulse-cli`. |
| Corporate network blocks install   | Try an older version: `npm install -g edge-impulse-cli@1.14.10`.                       |

---

### B. Arduino CLI & Flashing

* **Board not found**: Double-tap reset button to activate bootloader.
* **Wrong or missing board**: Run

  ```bash
  arduino-cli core update-index
  arduino-cli core install arduino:mbed_nano
  ```
* **Sketch folder mismatch**: `.ino` filename must match folder name.
* **Multiple `.bin` artifacts**: Delete extras before flashing.

---

### C. Serial Port & Permissions

If you get `Permission denied` on `/dev/ttyACM0`:

```bash
sudo usermod -a -G dialout $USER
newgrp dialout
```

---

### D. Environment Variables

* CLI not in PATH? Add to `~/.bashrc`:

  ```bash
  echo 'export PATH=$PATH:$(npm bin -g)' >> ~/.bashrc
  source ~/.bashrc
  ```
* Arduino CLI not found? Ensure `/usr/local/bin` is in PATH.

---

### E. Node.js & Python Conflicts

* Stick to Node v16–22 LTS.
* If Python conflicts occur during CLI install, create a virtualenv:

  ```bash
  python3 -m venv ei-env
  source ei-env/bin/activate
  ```
* Install inside that isolated environment.

---

### F. Hardware Issues

* Use a **data-capable USB cable** (not charge-only).
* For Nano 33 BLE Rev2 microphone issues, ensure the `PDM` library is installed.
* Run `lsusb` to verify device is recognized at USB level.

---

## ✅ Quick Reference

| Task                     | Command                                      |                                                 |
| ------------------------ | -------------------------------------------- | ----------------------------------------------- |
| Install Node.js          | \`curl ...                                   | sudo bash -`<br>`sudo apt install nodejs\`      |
| Install Edge Impulse CLI | `npm install -g edge-impulse-cli --force`    |                                                 |
| Install Arduino CLI      | \`curl ...                                   | sh && sudo mv bin/arduino-cli /usr/local/bin/\` |
| Install board support    | `arduino-cli core install arduino:mbed_nano` |                                                 |
| Flash project            | `arduino-cli compile/upload`                 |                                                 |
| Run daemon               | `edge-impulse-daemon`                        |                                                 |
| Fix port permissions     | `sudo usermod -a -G dialout $USER`           |                                                 |

---


✅ This is fully formatted Markdown, you can paste it **as-is** into your `README.md`.  

Do you want me to also add a **“Quick Start (TL;DR)”** section at the top (like 5 commands to get everything working) for users who just want minimal steps without reading the whole guide?
```
