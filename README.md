# npk-soil-monitoring-system

Smart Soil Nutrient Monitoring System Using NPK and Soil Moisture Sensor

Executive Summary:
This project implements a Smart Soil Nutrient Monitoring System   that continuously measures critical soil parameters Nitrogen (N), Phosphorus (P), Potassium (K) along with soil moisture. It uses sensors NPK sensor with RS485 Interface and soil moisture sensor connected to a microcontroller Arduino  to collect data. The data is processed and can be viewed locally on a serial monitor and optionally sent to an Blynk app for remote monitoring. This system aims to aid farmers and researchers with real-time soil analytics for precision agriculture, optimizing fertilizer use and irrigation. 

The Smart Soil Nutrient Monitoring System measures soil fertility by reading N, P, K nutrient levels and moisture content in real time. It typically consists of:

•	An NPK Soil Sensor commonly an RS485 Modbus sensor that outputs nutrient concentrations

•	A Soil Moisture Sensor giving an analog voltage proportional to moisture.

•	A Microcontroller Arduino that reads sensor data, applies calibration, and logs or transmits results.

•	An IoT Connectivity Module built-in Wi-Fi on ESP32 for cloud upload to Blynk app.

Key Components & Specs (example sensors): 

NPK Sensor:  DFRobot RS485 Soil NPK Sensor (SEN0605). Specs: 5–30V supply, measures N/P/K . Outputs via RS485 (Modbus-RTU) at 9600 baud. IP68 rated probe.  

Soil Moisture Sensor: DFRobot Gravity Capacitive Moisture (SEN0193).Specs: 3.3–5.5V supply, outputs 0–2.5V corresponding to 0–100% moisture. Corrosion-resistant, analog output. 

Microcontroller: Arduino Uno R3 (5V logic, one hardware serial)  

RS485 Interface: MAX485 TTL-to-RS485 converter module (for NPK sensor).  

Other: Jumper wires, power supply (5V/9V-12V for NPK), optional display, enclosures.

Overview

Modern agriculture benefits greatly from precise soil monitoring. By knowing exact NPK and moisture levels, farmers can:

Optimize Fertilizer Use:  Apply NPK fertilizers only as needed, reducing cost and environmental impact.

Improve Crop Yield:  Ensure plants have sufficient nutrients for optimal growth.

Efficient Watering: Irrigate based on moisture levels to conserve water and avoid over-watering.

Data-Driven Decisions: Collect long-term soil data to observe trends or model predictions.

This project provides a DIY, affordable solution to gather these insights continuously, with the possibility of cloud dashboards for remote monitoring.

Objectives

Measure soil nutrients (N, P, K): Use a dedicated NPK sensor to read nutrient concentrations.  

Monitor soil moisture: Use a capacitive moisture sensor to gauge water content (percentage).  

Real-time data: Continuously sample sensors and display values via Serial Monitor or local LCD.  

Remote monitoring: Optionally send data to an IoT cloud Blynk app for visualization.  

User-friendly interface: Provide clear output formats and thresholds

Scalability: Design for expansion (e.g. additional sensors like temperature, pH, or adding actuators for irrigation).  
Block Diagram

Firmware Overview

The firmware Arduino reads sensor values, formats them, and outputs via Serial or Wi-Fi. A typical code structure:

Sensor Libraries:  For NPK (ModbusMaster or manual Modbus frames) and moisture (analogRead). We use ModbusMaster library for Arduino, which simplifies Modbus RTU communication.

File Structure (example):
`main.ino` – Setup and loop. Initializes Serial, sensors, calls read functions, prints/sends data.  
`npk_sensor.h/.cpp` – Functions to query N, P, K (Modbus commands).  
 Modbus Commands: For many sensors (e.g. SEN0605 or generic), registers are: N at 0x001E, P at 0x001F, K at 0x0020. Commands include CRC (see example code).
Control of RS485 Driver: Before sending Modbus query, set DE high (transmit mode), send frame, then set DE low (receive mode) to read response.

Output: Print formatted readings to Serial. Example from  Arduino code:

Serial.print("Nitrogen: "); Serial.print(nitrogen); Serial.print(" mg/kg, ");

Serial.print("Phosphorus: "); Serial.print(phosphorus); Serial.print(" mg/kg, ");

Serial.print("Potassium: "); Serial.println(potassium); Serial.println(" mg/kg");


  (Sample output shown below).  
IoT: For Blynk, include `<BlynkSimpleEsp32.h>`. Authenticate with API keys/tokens and send data in loop.

 Wiring and Power
 
Assemble the circuit as per the Circuit & Wiring section above. Double-check connections.  


Power: Connect 5V (or chosen supply) to NPK sensor VCC and moisture sensor VCC. The Arduino’s 5V pin or an external 9V/12V adapter (regulated to 5V) can be used. The ESP32 requires 3.3V for logic but the sensor’s power can still be 5V (just share grounds).  

Ensure common ground between all parts.

 Monitor Output
 
Open Serial Monitor . The readings seen like:
  
  Nitrogen: 12 mg/kg, Phosphorus: 16 mg/kg, Potassium: 33 mg/kg, Moisture: 92
  (Values vary by sensor and soil).  
  
using Blynk, open the Blynk app to view gauges/sliders.
