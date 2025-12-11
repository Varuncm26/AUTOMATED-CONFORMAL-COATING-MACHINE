# AUTOMATED-CONFORMAL-COATING-MACHINE

![Project Status](https://img.shields.io/badge/Status-Completed-success)
![Hardware](https://img.shields.io/badge/Hardware-STM32L476-blue)
![Motion](https://img.shields.io/badge/Motion-CNC_6550-orange)
![Language](https://img.shields.io/badge/Language-C%2B%2B-red)


An automated conformal coating machine developed for PCB manufacturing, built using an **STM32L476** microcontroller, **CNC 6550** 2-axis motion system, and **VL6180X ToF** sensor logic. This project automates the application of acrylic coating to protect PCBs from environmental damage.

---

## 🚀 Project Overview

This project implements a compact and fully automated system capable of delivering controlled acrylic spray coating over PCBs. It was designed to bridge the gap between manual spraying and expensive industrial machinery.

**Key capabilities include:**
* **Precision Sensing:** Uses Time-of-Flight technology to detect PCBs on the belt without physical contact.
* **Fluid Logic:** Manages an external mixing nozzle (Air + Acrylic) using solenoid pinch valves.
* **Custom Electronics:** All control circuits were custom-designed, SMD-assembled, and tested to ensure reliable operation in industrial environments.
* **Safety First:** Includes hardware-level interrupts for emergency stops to meet safety standards.

---

## 🛠️ Features

### 🔹 1. Conveyor Belt Control
* **Actuation:** DC Motor driven belt system.
* **Control:** PWM-based speed regulation for smooth acceleration/deceleration.
* **Sync:** Motion is strictly synchronized with the spray cycle to ensure the PCB is stationary during coating.

### 🔹 2. PCB Detection (VL6180X)
* **Sensor:** STMicroelectronics VL6180X Time-of-Flight (ToF) distance sensor.
* **Function:** Provides accurate presence detection regardless of PCB color or surface finish.
* **Logic:** Acts as the primary trigger to halt the conveyor and initiate the coating sequence.

### 🔹 3. Spray Nozzle & Valve System
* **Mechanism:** External mixing nozzle combining compressed air and acrylic liquid.
* **Actuation:** Solenoid pinch valves used to control input lines.
* **Result:** Prevents clogging and ensures a stable, uniform spray pattern.

### 🔹 4. CNC 6550 Motion System
* **Axes:** Automated X–Y axis movement.
* **Pattern:** Executes predefined spray paths (snake/raster patterns).
* **Software:** Supports pattern files generated from computer software, parsed by the controller.

### 🔹 5. STM32L476 Controller
* **Core:** ARM Cortex-M4.
* **Role:** Central processing unit handling motor drivers, sensor I2C communication, and valve GPIOs.
* **Performance:** Ensures real-time operation and synchronization between motion and spraying.

### 🔹 6. Safety System
* **Hardware:** Integrated Emergency Stop buttons.
* **Response:** Immediate interrupt-driven shutdown of all motors and closure of valves.
* **UI:** Status display provides real-time feedback on machine state (Idle, Spraying, Emergency).

---

## ⚙️ System Architecture

### Hardware Block Diagram
*(Optional: Add your block diagram image here)*
`![Block Diagram](images/block_diagram.png)`

1.  **Input:** VL6180X (I2C), Control Panel (GPIO), E-Stop (Interrupt).
2.  **Processing:** STM32L476 (Custom Firmware).
3.  **Output:**
    * H-Bridge -> Conveyor DC Motor.
    * Stepper Drivers -> CNC Gantries.
    * MOSFETs -> Pinch Valves.
    * Display -> Status info.

---


