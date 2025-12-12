# 💨 Air Pressure Monitoring System

This module monitors the air pressure from the compressor using a high-precision Honeywell sensor. Maintaining the correct air pressure is vital for the external mixing nozzle to atomize the acrylic liquid correctly without splattering.

## 📝 Overview
We use the **Honeywell SSCDANN150PGAA5** sensor to measure the input air line. The STM32 reads the analog voltage from the sensor and converts it into a PSI (Pounds per Square Inch) value using a transfer function.

## 🔌 Circuit Diagram: Signal Conditioning
The sensor operates at **5V**, but the STM32 microcontroller requires a max input of **3.3V**. To interface them safely, we use a **Voltage Divider** circuit.

<img width="524" height="318" alt="image" src="https://github.com/user-attachments/assets/98ab465e-fc99-4025-9659-c7699fadc19e" />


### Component Breakdown
* **Sensor (SSCDANN150PGAA5):**
    * **Type:** Piezoresistive Silicon.
    * **Range:** 0 to 150 PSI (Gage Pressure).
    * **Output:** Analog Voltage (0.5V to 4.5V typical).
* **Voltage Divider (7.5kΩ / 10kΩ):**
    * This circuit scales down the sensor's output voltage to a safe range for the STM32.
    * **Ratio:** The 5V signal is stepped down by factor $\approx 0.57$.
    * **Calculated Max Voltage:** $5V \times \frac{10k}{10k + 7.5k} \approx 2.85V$ (Safe for STM32).

## 🧮 Formulas & Logic

The system uses the STM32's **12-bit ADC** to read the data. The conversion happens in two steps:

### 1. Convert ADC Value to Voltage
First, we calculate the actual voltage present at the microcontroller pin.
$$Voltage = \left( \frac{ADC_{value}}{4095} \right) \times 3.3V$$

### 2. Convert Voltage to Pressure
We apply the linear transfer function to convert the voltage into a pressure reading.
$$Pressure = P_{max} \times \frac{(Voltage - V_{min})}{(V_{max} - V_{min})}$$

* **P_max:** 150 PSI (Sensor Max Range)
* **V_min:** Voltage at 0 PSI (Offset)
* **V_max:** Voltage at 150 PSI (Full Scale)

## 💻 Code Implementation

```c
/* Macros for Calibration */
#define P_MAX 150.0f
#define V_MIN 0.33f  // Adjusted for voltage divider
#define V_MAX 2.60f  // Adjusted for voltage divider

float get_pressure(uint32_t adc_val) {
    // 1. Convert Raw ADC to Voltage
    float voltage = (float)adc_val / 4095.0f * 3.3f;

    // 2. Apply Transfer Function
    float pressure = P_MAX * (voltage - V_MIN) / (V_MAX - V_MIN);

    // 3. Clamp negative values (sensor noise at 0 psi)
    if (pressure < 0) pressure = 0;

    return pressure;
}
