# 🤖 Kalki OS (v1.0 - LITE Edition)
**A Custom, Secure, and Unbound AI Operating System**

[![OS: Ubuntu Server](https://img.shields.io/badge/OS-Ubuntu%20Server-E95420?style=flat-square&logo=ubuntu&logoColor=white)]()
[![Desktop: XFCE](https://img.shields.io/badge/Desktop-XFCE-2284F2?style=flat-square&logo=xfce&logoColor=white)]()
[![Status: Blueprint](https://img.shields.io/badge/Status-Blueprint-success?style=flat-square)]()

## 1. Project Overview
**Kalki OS (Lite)** is a custom, secure, and fully-featured Linux-based Operating System built from a minimal base. 

* The core feature of Kalki OS is that it is "unbound." 
* It boots to a lightweight, efficient Graphical User Interface (GUI) while running an "Unbound AGI" as a core system service.
* The AGI has full root-level control over the OS and its hardware.
* The system is accessible via text (using Telegram or Slack) and voice (using a Wake Word).

---

## 2. Architectural Strategy: Ubuntu Server & "Lite" Philosophy

### Why Ubuntu Server?
* Ubuntu Server provides a minimal, headless base system, starting you off with only a command line. 
* This allows for "from scratch" control, where the drivers, GUI, software, and security are manually configured to suit the AGI's needs.

### The "Lite" Philosophy
* The primary constraint of this system is the less RAM. 
* A heavy, modern desktop environment would leave no resources for the AGI, so the strategy prioritizes AGI performance over GUI aesthetics. 
* By building a lean, fast, and responsive OS with a lightweight desktop environment, every megabyte of RAM saved on the GUI is dedicated to the Kalki "Brain."

---

## 3. Installation & Setup Phases

### PHASE 0: Installer Preparation
The objective is to create a bootable Ubuntu Server USB installer using a Mac host and prepare the target PC.

1. **Get Installer:** Download the latest Ubuntu Server `.iso` file.
2. **Get USB Drive:** Acquire a USB drive (8GB or more); note that this drive will be completely erased.
3. **Flash the Drive (macOS):** Use `diskutil list` to identify the drive, unmount it, and flash the ISO using the `dd` command: `sudo dd if=path/to/ubuntu-server.iso of=/dev/rdiskX bs=1m status=progress`.
4. **Target PC Setup:** Insert the USB into the target PC, enter the BIOS/Setup menu (usually F2, F10, or DEL), configure it to boot from the USB drive first, and reboot.

### PHASE 1: The Base System 
During the Ubuntu Server installation, manually partition the 500GB drive with the recommended "Bare-Metal" scheme:
* **EFI Partition:** +512M for the Bootloader.
* **Swap Partition:** +8G for Virtual RAM, which is critical for a 4GB system.
* **Root Partition (`/`):** +100G for the OS Root.
* **Home Partition (`/home`):** Remainder of the disk for personal files.

*Note: Proceed through the Ubuntu Subiquity installer to create your root user and standard user.*

### PHASE 2: The "Lite" GUI (XFCE Desktop)
The objective is to build a lightweight, fast, and efficient graphical interface to conserve RAM for the AGI. 

Run the following commands to establish the graphical interface:
* **Display Server & Drivers:** The Pentium E5300 uses Intel integrated graphics. Install the display server and generic Intel drivers.
  ```bash
  sudo apt install xorg xserver-xorg-video-intel
  ```
* **Desktop Environment:** Install XFCE, a fast and lightweight desktop.
  ```bash
  sudo apt install xfce4 xfce4-goodies thunar xfce4-terminal
  ```
* **Login Manager:** Install and enable LightDM, a lightweight display manager.
  ```bash
  sudo apt install lightdm lightdm-gtk-greeter
  sudo systemctl enable lightdm
  ```

### PHASE 3: The AGI (The "Brain" & "Hands")
This phase ports the AGI blueprint to the new Linux OS, giving it "unbound" control.

* **Install the "Brain":**
  ```bash
  sudo apt install python3 python3-pip
  pip3 install google-generativeai fastapi uvicorn python-telegram-bot httpx pywhatkit requests
  ```
* **Install the "Hands" (Linux Toolbox):**
  ```bash
  sudo apt install ffmpeg arduino-cli scrot xdotool
  ```

**The Architecture:**
* **`brain_server.py`:** Written from scratch with a new "Toolbox" of functions utilizing the installed Linux commands. 
* **AI Manual:** The system instructions are rewritten to teach the AI how to use these specific Linux commands.

### PHASE 4: The "Unbound" Integration
The objective is to make the AGI "wake up" with the OS and be ready for commands.

* **Systemd Services (Autostart):** Create `.service` files for `kalki_brain.service` and `kalki_telegram_bot.service`. Use `systemctl enable` to launch these automatically on boot in the background so the AI is alive before login.
* **Voice Client (The "Ears" & "Mouth"):** Build a `voice_client.py` script. 
  * Uses a lightweight, 100% offline library (like porcupine) to listen strictly for the wake word ("Kalki").
  * Triggers a high-power `faster-whisper` (VAD) module to listen for commands.
  * Responds using an offline Linux TTS engine like `Piper-TTS`.
* **XFCE Autostart:** Add an entry in XFCE's "Session and Startup" to run the voice client upon login.

---

## 4. The Final System
The final result is a complete, custom OS that boots to a lightweight XFCE desktop. In the background, the Kalki "Brain" and "Telegram Bot" are already running as system services. 

The Voice Client silently listens for the wake word. You can use the machine like a normal computer, or simply say a command like, *"Kalki, take a screenshot, record my screen, and send a message to Papa,"* and the AGI will utilize its root-level Linux tools to execute the commands seamlessly within its own OS. This represents a true, "unbound" AI OS built efficiently for its hardware.
```
