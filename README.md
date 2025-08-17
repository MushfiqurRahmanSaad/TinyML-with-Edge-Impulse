# TinyML-with-Edge-Impulse
# This repository will go through the basics of setting up your TinyML device with Edge Impulse. It includes instructions on installation for relevant CLI, troubleshooting and an easy guide to allow more people to experience TinyML. This repository is built in partnership with Wireless Research @ UKM

---

---

# Edge Impulse on Linux — Beginner-Friendly Guide

This guide walks you through **installing Edge Impulse CLI on Linux** and connecting it to the **Arduino Nano 33 BLE Sense**. It includes **step-by-step setup**, **flashing instructions**, and a **troubleshooting section** for the most common issues beginners face.

---

## 📦 1. Prerequisites

Before installing, make sure your system is up-to-date and has the basics installed. Open your terminal and run:

```bash
# Update and upgrade your system
sudo apt update && sudo apt upgrade -y
```

### ✅ Check if you have the following installed:

* **Git**

```bash
git --version
```

* **Python (>= 3.8)**

```bash
python3 --version
```

If not installed:

```bash
sudo apt install python3 python3-pip -y
```

* **Node.js (>= 18.x recommended)**

```bash
node -v
```

If missing:

```bash
# Install Node.js 18.x LTS
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install -y nodejs
```

* **npm**

```bash
npm -v
```

* **USB permissions (for Arduino)**

```bash
lsusb
```

If your Arduino shows up, you’re good. If not, check cables/ports.

---

## 🚀 2. Install Edge Impulse CLI

Run:

```bash
npm install -g edge-impulse-cli
```

Verify:

```bash
edge-impulse-daemon --version
```

If you see a version number, installation worked!

---

## 🔌 3. Connect Arduino Nano 33 BLE Sense

1. Plug in your Arduino via USB.
2. Check available ports:

```bash
ls /dev/ttyACM*
```

Typical output: `/dev/ttyACM0`
(If you have multiple, unplug/replug Arduino and see which one disappears/appears.)

3. Flash the Arduino with Edge Impulse firmware:

```bash
edge-impulse-flash --clean
```

👉 **Important:** Run this from your **Arduino sketch folder** or the folder containing your `.ino` / firmware files.
If you’re unsure, navigate to your project folder with:

```bash
cd ~/Arduino
```

---

## ⚡ 4. Running Edge Impulse Daemon

Once flashed, run:

```bash
edge-impulse-daemon
```

Log in with your Edge Impulse credentials and select your project. Your Arduino should now appear as a device in the Edge Impulse Studio.

---

## 🛠 5. Troubleshooting (Common Issues & Fixes)

### ❌ Issue: `edge-impulse-daemon: command not found`

* Cause: npm global path not added to environment variables.
* Fix:

```bash
export PATH=$PATH:$(npm bin -g)
```

Add this permanently by editing `~/.bashrc`:

```bash
echo 'export PATH=$PATH:$(npm bin -g)' >> ~/.bashrc
source ~/.bashrc
```

---

### ❌ Issue: Arduino port not showing

* Check if Arduino is connected:

```bash
lsusb
ls /dev/ttyACM*
```

* If missing, try another cable (use **data cable**, not charge-only).
* Add user to dialout group for USB access:

```bash
sudo usermod -a -G dialout $USER
newgrp dialout
```

---

### ❌ Issue: Wrong Arduino CLI version

Edge Impulse works best with **Arduino CLI v0.35.x**.
Do **NOT** install with `sudo apt install arduino-cli`. Instead:

```bash
curl -fsSL https://raw.githubusercontent.com/arduino/arduino-cli/master/install.sh | sh
sudo mv bin/arduino-cli /usr/local/bin/
```

Check:

```bash
arduino-cli version
```

---

### ❌ Issue: "Permission denied" when flashing

* Run:

```bash
sudo chmod a+rw /dev/ttyACM0
```

(Replace `/dev/ttyACM0` with your port.)

---

### ❌ Issue: Multiple Node.js versions causing conflicts

* Remove old versions:

```bash
sudo apt remove nodejs npm -y
```

* Reinstall only the recommended version (Node.js 18 LTS).

---

### ❌ Issue: "No Arduino sketch found"

* Make sure you are in the correct folder where `.ino` or firmware files are stored.
* Example:

```bash
cd ~/Arduino/MyProject
```

---

### ❌ Issue: Python missing / incompatible

* Edge Impulse CLI requires Python 3.
  Check:

```bash
python3 --version
```

If missing:

```bash
sudo apt install python3 python3-pip -y
```

---

### ❌ Issue: Environment variable not loaded

* If `edge-impulse-daemon` only works with `sudo`, your PATH is not set correctly.
  Fix:

```bash
echo 'export PATH=$PATH:$(npm bin -g)' >> ~/.bashrc
source ~/.bashrc
```

---

## 🎯 6. Tips for Beginners

* To see **real-time logs** from Arduino:

```bash
screen /dev/ttyACM0 115200
```

(Press `Ctrl+A` then `K` to exit.)

* If unsure about your Arduino port:

```bash
dmesg | grep tty
```

Look for lines like:

```
[   34.123456] cdc_acm 1-1.2:1.0: ttyACM0: USB ACM device
```

---

## ✅ 7. Next Steps

Now that your Arduino Nano 33 BLE Sense is connected, you can:

1. Collect data in Edge Impulse Studio.
2. Train a model.
3. Deploy the model back to your device with:

   ```bash
   edge-impulse-run-impulse
   ```

---

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

```
