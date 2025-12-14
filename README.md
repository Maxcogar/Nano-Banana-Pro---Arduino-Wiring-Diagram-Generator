

AI-Generated Wiring Schematics (Nano Banana Pro)

This repository demonstrates a workflow for generating high-fidelity, professional wiring diagram schematics using Nano Banana Pro.

By combining a rigorous System Instruction with a structured Hardware & Pin Map prompt, the model can generate electrically accurate schematics (SVG/PNG) that include correct pinouts, color-coded wiring, and specific circuit topologies (like MOSFETs and Pull-up resistors).

1. The Result

Below is an example generated from the text prompt found in this repository.

![alt text]("C:\Users\maxco\Documents\Generated Image December 14, 2025 - 1_23PM.jpeg")


2. The Setup (System Instructions)

To replicate this style, paste the following into the System Instructions or System Prompt field of the model configuration. This forces the AI to adopt the persona of a technical illustrator and electrical engineer.

code
Markdown
download
content_copy
expand_less
**Role:**
You are an expert Electrical Engineer and Technical Illustrator specialized in creating professional, high-fidelity wiring diagram schematics. Your goal is to visualize hardware connections with 100% accuracy based *only* on the provided hardware list and pin assignment chart.

**Objective:**
Take a user-provided **Hardware List** and **Pin Assignment Chart** and generate a technical schematic diagram image that clearly shows how components are wired together.

**Operational Rules:**

1.  **Visual Style:**
    *   **Style:** Flat, 2D technical schematic (not photorealistic 3D, not isometric).
    *   **Background:** Clean white (#FFFFFF) for maximum contrast.
    *   **Lines:** Use orthogonal routing (lines must be vertical or horizontal, with 90-degree distinct corners). No curved or messy wires.
    *   **Colors:**
        *   **Red:** 5V / 3.3V Power (VCC).
        *   **Black:** Ground (GND).
        *   **Blue/Green/Yellow:** Signal wires (distinct colors for different data buses like I2C, SPI, UART).
    *   **Text:** Use the model's advanced text rendering to label every pin and component clearly. Text must be legible, sans-serif, and black.

2.  **Component Representation:**
    *   Represent the "Main Controller" (e.g., Arduino, ESP32, Raspberry Pi) as a rectangular block in the center or left.
    *   Represent peripheral sensors/modules as distinct rectangular blocks arranged logically around the controller.
    *   Label each block with its name (e.g., "DHT22", "OLED Display").
    *   Label the specific pins *inside* or *next to* the blocks (e.g., "SDA", "SCL", "D4", "GND").

3.  **Connection Logic:**
    *   Draw explicit wire lines connecting the pins as defined in the **Pin Assignment Chart**.
    *   Do not hallucinate connections that are not listed.
    *   If multiple components share a line (like an I2C bus or common Ground), use distinct "junction dots" (small black circles) to show the split.

4.  **Error Handling:**
    *   If a pin number in the chart does not exist on the specified hardware (e.g., "Pin 50" on an Arduino Uno), generate a textual warning in the response before generating the image.
3. The Prompt Template

The quality of the output depends heavily on how the data is structured. The model performs best when the hardware is listed first, followed by specific sub-circuit instructions, and finally a strict pin mapping list.

Copy the template below to generate your own diagrams.

Template
code
Text
download
content_copy
expand_less
[Project Name/Description]
[Brief description of what the device does]

Hardware List
Microcontroller
[Name of the specific board, e.g., ESP32 WROOM DevKit V1]

Sensors (Inputs)
[Sensor Name] - [Interface Type] - [Purpose]
[Sensor Name] - [Interface Type] - [Purpose]

Actuators (Outputs)
[Device Name] - [Control Method, e.g., PWM/Relay] - [Purpose]
[Device Name] - [Control Method, e.g., PWM/Relay] - [Purpose]

Specific Circuit Details (Optional)
If you need specific components between pins (like resistors or transistors), describe them here:
- [Component Name]: [Source Pin] → [Resistor Value] → [Destination Pin]
- [Pull-up/down]: [Pin] → [Resistor Value] → [GND/VCC]

Pin Assignment Chart
Protocol/Group 1 (e.g., I2C):
  [Pin Number] → [Target Pin Name] (Device Name)
  [Pin Number] → [Target Pin Name]

Protocol/Group 2 (e.g., SPI):
  [Pin Number] → [Target Pin Name]
  [Pin Number] → [Target Pin Name]

Analog Inputs:
  [Pin Number] → [Target Pin Name]

Digital Outputs:
  [Pin Number] → [Target Pin Name]
4. Example Prompt

Here is the exact prompt used to generate the Terrarium Controller image shown above. Note how the pin assignments are grouped by protocol (I2C, Analog, PWM) rather than just a random list—this helps the AI group the wires visually.

code
Text
download
content_copy
expand_less
Terrarium Controller Firmware
ESP32 WROOM-based controller for tropical carnivorous plant terrariums.

Hardware
Microcontroller
ESP32 WROOM on Freenove Universal Breakout Board

Sensors
SHT31-D (I2C) - Air Temp/Hum
VEML7700 (I2C) - Light Level
DS18B20 (1-Wire) - Soil Temp
Capacitive Soil Moisture x2 (Analog)
TDS Meter (Analog)
pH Sensor (Analog)
NTC Thermistor (Analog)

Outputs
Circulation Fans x2 (MOSFET + PWM)
Heating Plate (MOSFET + PWM)
SG90 Servos x2 (PWM)
Grow Lights (SSR)
Water Pump (SSR)
Humidifier Modules x2 (SSR)

Wiring
MOSFET Circuit (IRLZ44N) - For Fans & Heater
ESP32 GPIO → 100-220Ω resistor → Gate
Gate → 10kΩ resistor → GND (pull-down)
Source → GND
Drain → Load negative
Load positive → 12V

Pin Assignments
I2C:
  GPIO21 → SDA (SHT31-D, VEML7700)
  GPIO22 → SCL

1-Wire:
  GPIO4  → DS18B20 soil temp

Analog Inputs:
  GPIO32 → Soil moisture 1
  GPIO33 → Soil moisture 2
  GPIO34 → TDS meter
  GPIO35 → pH sensor
  GPIO36 → Heater thermistor

PWM Outputs:
  GPIO16 → Fans (MOSFET)
  GPIO17 → Heater (MOSFET)
  GPIO18 → Servo 1
  GPIO19 → Servo 2

SSR Outputs:
  GPIO23 → Grow lights
  GPIO25 → Water pump
  GPIO26 → Humidifier 1
  GPIO27 → Humidifier 2
