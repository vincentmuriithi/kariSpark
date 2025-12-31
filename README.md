# kariSpark ⚡


![Version](https://img.shields.io/badge/version-1.0.0-blue)
[![License](https://img.shields.io/badge/license-Apache--2.0-green)](LICENSE)

**kariSpark** is a cross-platform desktop and CLI tool for downloading, uploading, and transferring programs between microcontrollers.  
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

### 🖥️ Desktop App (GUI)
Download the latest installer for your operating system from the  
**[GitHub Releases](https://github.com/vincentmuriithi/kariSpark/releases)** page.

| Platform | Installer Type | Suitable For | Download |
|----------|----------------|--------------|---------------|
| **<img src="https://www.citypng.com/public/uploads/preview/windows-10-logo-icon-free-png-735811696612207vhxwa5iwgf.png" width=16> Windows** | `.msi` | All Windows 10/11 |[Download](https://github.com/vincentmuriithi/kariSpark/releases/latest/download/kariSpark.msi)
| **🐧 Linux** | `.AppImage` | Most distros (portable, no install) |[Download](https://github.com/vincentmuriithi/kariSpark/releases/download/v1.0.0-gui/kariSpark_1.0.0_amd64.AppImage)|
| 🐧 **Linux** | `.deb` | Ubuntu / Debian / Mint |[Download](https://github.com/vincentmuriithi/kariSpark/releases/download/v1.0.0-gui/kariSpark_1.0.0_amd64.deb)|
| **🐧 Linux** | `.rpm` | Fedora / RHEL / CentOS / openSUSE |[Download](https://github.com/vincentmuriithi/kariSpark/releases/download/v1.0.0-gui/kariSpark-1.0.0-1.x86_64.rpm)|

#### <img src="https://www.citypng.com/public/uploads/preview/windows-10-logo-icon-free-png-735811696612207vhxwa5iwgf.png" width=16> Windows
Download the `.msi`, run it, and launch **kariSpark** from the start menu.

#### 🐧 Linux – AppImage (portable)
```bash
chmod +x kariSpark_*.AppImage
./kariSpark_*.AppImage
```

#### 🐧 Linux – Debian/Ubuntu (.deb)
```bash
sudo dpkg -i kariSpark_*.deb
```

#### 🐧 Linux – RPM-based (.rpm)
```bash
sudo rpm -i kariSpark-*.rpm
```

---

### 💻 CLI Version
You can still use kariSpark as a command-line tool on **Windows and Linux**.

#### <img src="https://www.citypng.com/public/uploads/preview/windows-10-logo-icon-free-png-735811696612207vhxwa5iwgf.png" width=16> Windows
1. Download the CLI binary from **[GitHub Releases](https://github.com/vincentmuriithi/kariSpark/releases)**
2. Add the binary to your system `PATH` to use it globally

#### 🐧 Linux
**Option A – Debian/Ubuntu (.deb)**
```bash
sudo dpkg -i kariSpark_CLI.deb
```
**Option B – Manual binary**
```bash
# Download the CLI binary from GitHub Releases
sudo mv karispark /usr/local/bin/
```

Check installation:
```bash
karispark --version
```

## 🛠 Command Reference
```bash
# Used for generating a kari.toml config file
karispark init

# Used for detecting the available connected boards
karispark detect

# Used to upload the specified program
karispark upload port <options>

# Used to download a program from the specified board
karispark download port <options>

# Used to transfer program between boards
karispark transfer <options>

# Used to verify the correctness and compare a saved program to one running in the given MCU
karispark verify <options>
```


## 🔨 Usage
### karispark doctor
Used to inspect your environment and install required dependencies to get you started on **kariSpark**.  
Syntax:
```bash
karispark doctor
```

### karispark init
Used to generate **kari.toml** config file for use with **kariSpark**.  
The ***kari.toml*** file is used to add configurations to your project or work instead of passing the parameters on the cli each time you run a given command.  
Syntax:
```bash
karispark init
```

### karispark detect
Used to detect the connected boards in your pc.  
Syntax:
```bash
karispark detect
```

### karispark upload
Used to upload programs to connected boards. Requires the port to be specified when flashing to single board but also supports parallel flashing for uploading multiple boards.  
For single board you use:
```bash
karispark upload port <options>
```
Example:
```bash
karispark upload COM3 --board uno --programmer arduino --dir . 
```
For multiple board uploading also known as parallel flashing is as:
```bash
karispark upload --parallel <options>
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

  ### karispark download
  Used to download programs to connected boards. Requires the port to be specified when downloading the program with an exception only when using usbasp as the programmer.
  It also allows for one to specify a directory where the downloaded program will be saved. If not saved it will be saved in the temp directory.
  
  Syntax:
  ```bash
  karispark download port <options>
  ```
  Example:
  ```bash
  karispark /dev/ttyACM0 -b mega2560 --programmer arduino  -d "./downloaded_program"
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

  ### karispark transfer
  This is the most powerful and unique feature of ***kariSpark***.
  It's used to transfer program between boards of same MCU architecture e.g atmega328 to atmega328. It requires the source port and target port to be specified when transferring programs between two boards.

  It also supports multiple board program transfer also know as parallel transfer. It works by single source multiple targets strategy.

  The term parallel isn't just sugar-coating, but an explanation of how the transfer works. **kariSpark** spawns multiple threads in which each transfer/upload during the transaction has its own thread hence saving transfer and upload times when transferring to multiple devices. Hence transfers occur concurrently instead of waiting for one board to finish before starting the next.

  Two boards transfer signature is as follows:
  ```bash
  karispark transfer source_port target_port <options>
  ```
  Example:
  ```bash
  karispark transfer COM3 COM4 -b uno -p arduino 
  ```
  In parallel transfer you only need specify the ***source_port*** only.
  ```bash
  karispark source_port --parallel <options>
  ```
  Example:
  ```bash
  karispark transfer COM14 --parallel --programmer arduino --board leonardo
  ```
This command usage is as follows:
```bash
Arguments:
  [SOURCE_PORT]
  [TARGET_PORT]

Options:
  -b, --board <BOARD>
  -p, --programmer <PROGRAMMER>
      --parallel
  -h, --help                     Print help
  ```

### karispark verify
Used to verify the correctness of a program in a given microcontroller and also comparing if a given saved program file is same as one running in a given MCU.
Syntax:
```bash
karispark verify port <options>
```
Example:
```bash
karispark verify COM15 -b attiny85 -p arduino --file ./saved_program.hex
```
You also just pass a directory to file and it will automatically search in that directory for the saved program with the name ***temp_program.hex*** for AVR MCUs. If you don't give the path   or file it will check it in the temp directory.

The port argument is optional when using usbasp programmer.

This command usage is as follows:
```bash
Arguments:
  [PORT]

Options:
  -f, --file <FILE>
  -b, --board <BOARD>            [default: uno]
  -p, --programmer <PROGRAMMER>  [default: arduino]
  -h, --help                     Print help
  ```

kariSpark is part of the kari ecosystem.

## 📋 Supported Board Names

When using `kariSpark` commands, you can specify your board using any of the following aliases.  
All names on the left resolve to the canonical MCU on the right:

| Aliases | Resolved MCU |
|---------|--------------|
| `uno`, `nano`, `328p`, `atmega328p`, `atmega328`, `328` | **ATmega328P** |
| `mega`, `atmega2560`, `mega2560`, `2560` | **ATmega2560** |
| `mega1280`, `1280`, `atmega1280`, `m1280` | **ATmega1280** |
| `leonardo`, `micro`, `promicro`, `32u4`, `atmega32u4`, `m32u4` | **ATmega32u4** |
| `nanoevery`, `4809`, `atmega4809`, `m4809` | **ATmega4809** |
| `mini`, `168`, `atmega168`, `m168` | **ATmega168** |
| `attiny85`, `tiny85`, `t85` | **ATtiny85** |
| `attiny13`, `tiny13`, `t13` | **ATtiny13** |


## 📜 License
This library is licensed under the Apache License 2.0.
See the full license here: [Apache-2.0 License.](https://opensource.org/licenses/Apache-2.0)

## ⚖️ Trademark Notice
kari and kariSpark are trademarks of Vincent Muriithi Karimi.

## ✨ Author
**Vincent Muriithi Karimi**  
GitHub: [vincentmuriithi](https://github.com/vincentmuriithi)  
Email: kari.clientdesk@gmail.com 

