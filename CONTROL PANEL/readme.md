# 🎛️ Control Panel & User Interface

This folder covers the Human-Machine Interface (HMI) of the machine. It allows the operator to start/stop the process and provides audio-visual feedback regarding the machine's status (Idle, Coating, Error).

## 📝 Overview
The control panel is designed for simplicity and safety. It features:
* **Push Buttons:** For user input (Start) and safety (Emergency Stop).
* **Buzzer:** For audible feedback (Start beep, Error alarms).
* **LCD Display:** Visual text feedback of the current machine operation.

## 🔌 Circuit Diagrams

### 1. Digital Inputs (Buttons) with Hardware Debounce
We use a **Pull-Up Configuration** with an **RC (Resistor-Capacitor) Filter**.

<img width="528" height="600" alt="Gemini_Generated_Image_wjzcbawjzcbawjzc" src="https://github.com/user-attachments/assets/888fe1fc-66ce-4fb0-879d-2a2bfa4c2537" />


* **Logic:** The button is "Active LOW".
    * **Pressed:** Logic `0` (GND).
    * **Released:** Logic `1` (3.3V).
* **R&D Feature (Hardware Debounce):**
    * **0.1µF Capacitor:** Mechanical buttons "bounce" (rapidly on/off) when pressed. This capacitor smooths out those voltage spikes.
    * **Result:** A clean signal reaches the STM32, preventing false triggers or double-clicks without needing complex software filtering.
    * **10kΩ Resistor:** Weak pull-up to keep the line High when the button is open.

### 2. Audio Output (Buzzer)
The STM32 GPIO pins cannot provide enough current to drive a buzzer directly. We use a **BC547 NPN Transistor** as a switch.

<img width="524" height="396" alt="image" src="https://github.com/user-attachments/assets/25b98567-0c2a-4917-ac51-9d10c4c771a3" />


* **BC547 Transistor:** Acts as a current amplifier. A small signal from the microcontroller Base (B) allows a larger current to flow from Collector (C) to Emitter (E), powering the buzzer.
* **Base Resistor:** Limits current from the MCU to the transistor base.
* **Operation:**
    * **Logic HIGH:** Transistor ON -> Buzzer Beeps.
    * **Logic LOW:** Transistor OFF -> Silent.

## ⚙️ Interface Logic

### Status Indicators
The machine communicates its state through the Buzzer and LCD:

| Event | Buzzer Pattern | LCD Message |
| :--- | :--- | :--- |
| **Startup** | 1 Short Beep | `System Ready` |
| **Start Process** | 2 Short Beeps | `Processing...` |
| **Completion** | 1 Long Beep | `Done. Remove PCB` |
| **Error / Stop** | 3 Rapid Beeps | `EMERGENCY STOP` |

### Emergency Stop
The **Emergency Stop** button is wired to a specific GPIO pin configured as an **External Interrupt**.
* **Priority:** Highest.
* **Action:** When pressed, the firmware immediately pauses the CPU, halts PWM generation (Conveyor), and de-energizes all valves.

## 📊 Technical Details
| Component | Value/Type | Function |
| :--- | :--- | :--- |
| **Buzzer** | 5V Active Buzzer | Audio Feedback |
| **Driver Transistor** | BC547 (NPN) | Low-side Switch for Buzzer |
| **Display** | LCD 16x2 (I2C) | Visual Status |
| **Button Filter** | 10kΩ R / 0.1µF C | Noise/Bounce Elimination |
| **Logic Level** | 3.3V / 5V | Mixed Voltage Interface |
