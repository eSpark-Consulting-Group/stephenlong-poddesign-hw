# Pod Hardware Design and Development
#### Client Name: Stephen Long

## Project Requirements
The Pod is a modular add-on for the Trak system, designed to provide advanced power monitoring and sensing capabilities. This document outlines the hardware and firmware requirements for developing the Pod, as well as the updates required for the Trak app to integrate and manage Pod functionality.

1. Pod relies on Trak for:
    a. Data storage and cloud processing
    b. User interface and reporting
    c. AI-based troubleshooting enhancement
2. Trak module will handle:
    a. Cloud uploads
    b. Advanced data analytics
    c. User alerting based on set thresholds
    d. Interface with Trak App

### Schematic Requirements
#### Core Components
- Microcontroller: nRF52840 (for Bluetooth Mesh communication with Trak).
- Wireless Communication:
    - Bluetooth Mesh (BT Mesh) for data transmission to Trak.
    - 2.4GHz BLE monitoring for wireless signal health analysis.
- Battery:
    - 3,000 mAh Li-Po battery.
    - Battery automatically recharges when DC voltage is turned on.
 
#### Core Features
- Voltage Measurement:
    - Range: 0-240VAC, 0-48VDC (expandable to 100VDC if required).
    - Accuracy: ±0.5%.
    - Sensor Model: INA219 or equivalent.
    - Connection Method: Single wire taps into phase lines (A, B, C) and neutral, routed through a custom harness to the Pod. Voltage dividers and INA219 sensors are integrated into the Pod PCB.
- Current Measurement:
    - Range: 0-400A (AC/DC).
    - Accuracy: ±1%.
    - Sensor Model: ACS758 or equivalent.
    - Connection Method: ACS758 sensors are external to the Pod. Phase wires (A, B, C) pass through each sensor’s aperture. Analog outputs from the ACS758 sensors are routed via the wire harness to dedicated ADC inputs on the Pod PCB. Shared 5V and GND lines power the sensors.
- Ground Fault Detection:
    - Immediate alerts for ground shorts or leakages.
    - Implementation using differential current sensing or similar methods.
- Power Quality Monitoring:
    - Detection of voltage spikes, surges, and power interruptions.
    - Real-time calculation of power usage (Watts) and energy (kWh).
- Waveform Display & High-Speed Sampling:
    - Real-time and historical waveform analysis.
    - Capturing power fluctuations, voltage sag, spikes, and phase imbalance.
- AI-Assisted Troubleshooting:
    - On-device edge AI for anomaly detection.
    - Cloud-based AI for in-depth diagnostics.
- Environmental Monitoring:
    - Temperature Sensor: SHTC3 (-40°C to 125°C).
    - Humidity Sensor: SHTC3 (0-100% RH).
- Motion Detection:
    - ADXL345 3-axis accelerometer for impact detection.
- Wireless Signal Health Monitoring:
    - Wi-Fi spectrum analyzer and RF monitoring.
    - ~2.4GHz, 5GHz, and 900 MHz RF interference detection.~
- Data Storage & Logging:
    - Local storage capability for network outage scenarios.
    - Automatic data upload once connection is restored.
- Power and Data Interface:
    - USB-C connector for power and data.
    - Support for USB Power Delivery (PD) for efficient power negotiation.
- MCU Integration:
    - Onboard MCU to handle sensor data aggregation and communication.
    - Suggested MCU: ESP32 for SPI/I2C/UART interfacing with sensors.
- Regulation
    - Onboard voltage regulators for stable operation (e.g., LM7805 for 5V, AMS1117 for 3.3V).
- Connectors and Expansion:
    - Headers for additional external sensors if needed.
    - Test points for debugging and QA.
- Sensor Interface
    - Voltage Sensor: Internal voltage dividers and INA219 sensors handle voltage measurement. External taps connect directly to the Pod harness
    - Current Sensor: External ACS758 sensors handle current measurement. Analog outputs are routed via the harness to the Pod ADC.
    - GPIO or I2C for future expansion.

### PCB Requirements
- Power Management:
    - Support for a maximum power draw of 10W.
- PCB Layout:
    - Isolation between high-voltage AC and low-voltage DC circuits.
    - High-speed traces for sensor data communication.
    - ESD protection on USB-C and sensor lines.
- Environmental Protection:
    - Conformal coating for humidity resistance.
    - IP67-rated enclosure compatibility. (Not our concern)
