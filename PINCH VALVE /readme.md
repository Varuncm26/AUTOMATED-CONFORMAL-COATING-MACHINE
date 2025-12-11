# 💧 Pinch Valve Fluid Control System

This folder details the fluid control subsystem used to manage the flow of acrylic coating and compressed air to the spray nozzle. The system uses **two independent solenoid pinch valves** to ensure precise mixing and zero clogging.

## 📝 Overview
To prevent the conformal coating liquid from drying inside the valves or pumps, we use **Pinch Valves**. These valves squeeze a soft silicone tube to stop flow, meaning the mechanism never physically touches the sticky acrylic fluid.

**We use two separate valves:**
1.  **Air Valve:** Controls compressed air input for atomization.
2.  **Liquid Valve:** Controls the flow of the conformal coating fluid.

## 🔌 Circuit Diagram
Each valve is controlled by a dedicated **Low-Side MOSFET Switch** circuit, identical to the motor driver but tuned for on/off solenoid operation.

<img width="524" height="358" alt="image" src="https://github.com/user-attachments/assets/99141d3c-32ba-42a0-a713-cff4f3d27439" />


### Component Breakdown
* **NTD5867 (N-Channel MOSFET):** Functions as a digital switch. It handles the high current required to energize the solenoid coil.
* **1N4007 (Flyback Diode):** Crucial for solenoids. It safely dissipates the high-voltage spike created by the magnetic coil when the valve is turned off.
* **220Ω Resistor:** Protects the STM32 GPIO pin from current surges.
* **10kΩ Resistor:** A pull-down resistor ensuring the valve stays **CLOSED** (OFF) during startup or reset, preventing accidental leakage.

## ⚙️ Mechanical Specs
* **Valve Type:** Solenoid Pinch Valve (Normally Closed).
* **Tube Compatibility:** Soft Silicone tubing.
* **Function:** Squeezes tube to stop flow; releases to allow flow.
* **Safety:** "Normally Closed" design ensures that if power fails, the valves automatically shut, preventing chemical spills.

## 📊 Technical Details
| Parameter | Value |
| :--- | :--- |
| **Input Voltage** | 12V DC |
| **Logic Voltage** | 3.3V (STM32 Compatible) |
| **Control Method** | Digital GPIO (High/Low) |
| **Switching Speed** | Instant (<100ms) |

## 💻 Usage
Unlike the motor (which uses PWM), these valves are controlled via simple Digital GPIO signals.

**Control Logic:**
* **Logic HIGH (1):** Valve Open (Flow Active).
* **Logic LOW (0):** Valve Closed (Flow Stopped).

**Sequence Strategy:**
To prevent dripping, the firmware typically opens the **Air Valve** slightly before the **Liquid Valve**, and closes the **Liquid Valve** before the **Air Valve**.
