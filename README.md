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

Check installation:
```bash
kari --version
```

## 🛠 Command Reference
```bash
# Used for generating a kari.toml config file
kari init

# Used for detecting the available connected boards
kari detect

# Used to upload the specified program
kari upload port <options>

# Used to download a program from the specified board
kari download port <options>

# Used to transfer program between boards
kari transfer <options>

# Used to verify the correctness and compare a saved program to one running in the given MCU
kari verify <options>
```


## 🔨 Usage
### kari doctor
Used to inspect your environment and install required dependencies to get you started on **kariSpark**.  
Syntax:
```bash
kari doctor
```

### kari init
Used to generate **kari.toml** config file for use with **kariSpark**.  
The ***kari.toml*** file is used to add configurations to your project or work instead of passing the parameters on the cli each time you run a given command.  
Syntax:
```bash
kari init
```

### kari detect
Used to detect the connected boards in your pc.  
Syntax:
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
  
  Syntax:
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

  ### kari transfer
  This is the most powerful and unique feature of ***kariSpark***.
  It's used to transfer program between boards of same MCU architecture e.g atmega328 to atmega328. It requires the source port and target port to be specified when transferring programs between two boards.

  It also supports multiple board program transfer also know as parallel transfer. It works by single source multiple targets strategy.

  The term parallel isn't just sugar-coating, but an explanation of how the transfer works. **kariSpark** spawns multiple threads in which each transfer/upload during the transaction has its own thread hence saving transfer and upload times when transferring to multiple devices. Hence transfers occur concurrently instead of waiting for one board to finish before starting the next.

  Two boards transfer signature is as follows:
  ```bash
  kari transfer source_port target_port <options>
  ```
  Example:
  ```bash
  kari transfer COM3 COM4 -b uno -p arduino 
  ```
  In parallel transfer you only need specify the ***source_port*** only.
  ```bash
  kari source_port --parallel <options>
  ```
  Example:
  ```bash
  kari transfer COM14 --parallel --programmer arduino --board leonardo
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

### kari verify
Used to verify the correctness of a program in a given microcontroller and also comparing if a given saved program file is same as one running in a given MCU.
Syntax:
```bash
kari verify port <options>
```
Example:
```bash
kari verify COM15 -b attiny85 -p arduino --file ./saved_program.hex
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

## 📜 License
This library is licensed under the Apache License 2.0.
See the full license here: [Apache-2.0 License.](https://opensource.org/licenses/Apache-2.0)

## ✨ Author
**Vincent Muriithi Karimi**  
GitHub: [vincentmuriithi](https://github.com/vincentmuriithi)  
Email: kari.clientdesk@gmail.com 