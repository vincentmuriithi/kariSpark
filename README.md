# kariSpark ⚡


![Version](https://img.shields.io/badge/version-1.0.0-blue)
[![License](https://img.shields.io/badge/license-Apache--2.0-green)](LICENSE)

**kariSpark** is a command-line interface (CLI) tool for downloading, uploading, and transferring programs between microcontrollers.  
It is designed to make working with embedded devices simple and efficient.  

Currently, kariSpark supports **AVR-based MCUs** (such as ATmega328P, ATmega2560, etc.), with plans for broader MCU family support in the future.

---

## ✨ Features
- 📥 **Download** programs from your MCU to your computer.  
- 📤 **Upload** programs to your MCU (single or multiple boards in parallel).  
- 🔄 **Transfer** programs directly between two MCUs.  
- ✅ **Verify** uploaded programs against a reference hex file.  
- 🔍 **Detect** connected boards and available ports.  
- 🛠️ **Doctor** mode to diagnose your environment.  
- ⚙️ **Init** command to generate a ready-to-use `kari.toml` config file.  

---

## 🚀 Installation

Download the latest release from the [GitHub Releases](https://github.com/vincentmuriithi/kariSpark/releases).  
Add it to your system PATH to use `kariSpark` globally.

## 🔨 Usage
### kari doctor
Used to inspect your environment and install required dependencies to get you started on **kariSpark**.
```bash
kari doctor
```

### kari init
Used to generate **kari.toml** config file for use with **kariSpark**.
```bash
kari init
```

### kari detect
Used to detect the connected boards in your pc.
```bash
kari detect
```

### kari upload
Used to upload programs to connected boards. Requires the port to be specified when flashing to single board but also supports parallel flashing for uploading multiple boards.
For single board you use:
```bash
kari upload port <options>
```
Example:
```bash
kari upload COM3 --board uno --programmer arduino --dir . 
```
For multiple board uploading also known as parallel flashing is as:
```bash
kari upload --parallel <options>
```
In parallel flashing port is not needed and if you pass it it will be ignored. 
This command usage is as follows:
```bash
Arguments:
[PORT]
Options:
  -b, --board <BOARD>
  -p, --programmer <PROGRAMMER>
  -d, --dir <DIR>
      --parallel
  -h, --help                     Print help
  ```

  ### kari download
  Used to download programs to connected boards. Requires the port to be specified when downloading the program with an exception only when using usbasp as the programmer.
  It also allows for one to specify a directory where the downloaded program will be saved. If not saved it will be saved in the temp directory.
  
  It's signature is as follows:
  ```bash
  kari download port <options>
  ```
  Example:
  ```bash
  kari /dev/ttyACM0 -b mega2560 --programmer arduino  -d "./downloaded_program"
  ```
  This command usage is as follows:
  ```bash
  Arguments:
  [PORT]

Options:
  -b, --board <BOARD>
  -d, --dir <DIR>
  -p, --programmer <PROGRAMMER>
  -h, --help                     Print help
  ```