# From Sensor to User Report

# 1. Project Title

**AIoT Smart Granary: Intelligent Monitoring and Protection System for Agricultural Storage Facilities**

---

# 2. Problem Description

Agricultural products such as rice, corn, and grains are highly vulnerable to environmental conditions during storage. Excessive humidity can cause mold growth, product degradation, and significant economic losses. In addition, rodents and pests may damage stored goods and reduce their quality.

In many rural and remote areas, storage facilities often experience unstable electricity and unreliable Internet connectivity. Traditional monitoring methods require manual inspection, which is inefficient and may fail to detect problems early.

This project proposes an AIoT-based Smart Granary system that continuously monitors environmental conditions, detects potential rodent activities, and automatically activates protective mechanisms. The system utilizes Edge Processing on an ESP32 microcontroller to ensure real-time operation even when Internet connectivity is unavailable.

---

# 3. Stakeholders

## Primary Stakeholders

- Farmers
- Warehouse owners
- Agricultural cooperatives

## Secondary Stakeholders

- Agricultural insurance companies
- Agricultural authorities
- Food supply chain operators

## System Operators

- Warehouse managers
- Maintenance technicians

---

# 4. PEAS Analysis

## Performance

The system is considered successful when:

- Humidity is maintained below 65%.
- Rodent activities are detected within 1 second.
- Alerts are generated immediately when abnormal conditions occur.
- The system continues operating during temporary Internet outages.
- Data is reliably transmitted to the dashboard when connectivity is available.

## Environment

The system operates in agricultural storage warehouses characterized by:

- Low-light conditions
- Dusty environments
- Variable temperatures and humidity
- Presence of rodents and pests
- Intermittent Internet connectivity
- Limited technical supervision

## Actuators

The system uses the following actuators:

- Relay Module (controls ventilation fan)
- Buzzer (rodent deterrent and alarm)
- OLED Display (local monitoring)
- LED Indicators (status visualization)

## Sensors

The system collects data using:

- DHT11 Temperature and Humidity Sensor
- PIR Motion Sensor
- LM393 Obstacle Sensor
- Light Dependent Resistor (LDR)

---

# 5. STA Analysis

## Constraints

- Limited network connectivity in rural areas.
- Low-cost hardware requirements.
- Sensor accuracy limitations.
- Environmental dust and moisture affecting sensors.

## Risks

- False alarms caused by sensor noise.
- Sensor degradation over time.
- Wi-Fi connection interruptions.
- Power outages.

## Failure Points

- DHT11 malfunction.
- PIR sensor false triggering.
- Wi-Fi disconnection.
- Relay failure.
- ESP32 reboot due to unstable power supply.

## Mitigation Strategies

- Implement threshold validation and filtering.
- Use local OLED display for offline monitoring.
- Enable automatic Wi-Fi reconnection.
- Store critical system states locally.
- Design the system to continue operating without cloud connectivity.
- Periodically inspect and clean sensors.

---

# 6. DIKW Data Flow

## Data

Raw sensor readings:

- Temperature (°C)
- Humidity (%)
- PIR motion status (HIGH/LOW)
- LM393 detection status (HIGH/LOW)
- Light intensity readings

### Example

```text
Temperature = 32°C
Humidity = 78%
PIR = HIGH
LM393 = LOW
```

## Information

Processed observations:

- Humidity exceeds safety threshold.
- Motion detected near storage area.
- Rodent activity detected.
- Warehouse door potentially opened during nighttime.

## Knowledge

By combining multiple information sources:

- High humidity indicates risk of mold development.
- Repeated obstacle detections suggest rodent presence.
- High humidity combined with rodent activity indicates increased risk of crop damage.

## Decision

The system performs actions such as:

- Activating ventilation fan.
- Triggering buzzer alarm.
- Sending notifications to the dashboard.
- Updating risk level.

## User Interaction

Users can:

- View real-time environmental data.
- Receive alerts through Blynk dashboard.
- Switch between Auto and Manual modes.
- Adjust humidity thresholds remotely.
- Monitor historical system behavior.

---

# 7. System Architecture

## Hardware Layer

### Sensors

- DHT11
- PIR HC-SR501
- LM393 Obstacle Sensor
- LDR

### Controller

- ESP32

### Actuators

- Relay Module
- Buzzer
- OLED Display
- LED Indicators

## Edge Processing Layer

ESP32 performs:

- Data collection
- Data preprocessing
- Risk score calculation
- Local decision making

## Communication Layer

- Wi-Fi
- HTTP / MQTT
- Blynk Cloud

## Application Layer

- Blynk Dashboard
- Mobile Notifications
- Historical Data Visualization

### Architecture Flow

```text
+---------------------------------------------------+
|                    Blynk Cloud                    |
+---------------------------------------------------+
                    ▲
                    │
              WiFi / MQTT
                    │
                    ▼
+---------------------------------------------------+
|                 ESP32 Edge Node                   |
|                                                   |
| - Data Collection                                 |
| - Risk Analysis                                   |
| - Local Decision Making                           |
+---------------------------------------------------+
      ▲            ▲            ▲            ▲
      │            │            │            │
    DHT11         PIR         LM393         LDR

      ▼
+---------------------------------------------------+
|                  Actuators                        |
+---------------------------------------------------+
| Relay Fan | Buzzer | OLED Display | LEDs         |
+---------------------------------------------------+
```

---

# 8. Implementation

## Hardware Components

- ESP32 Development Board
- DHT11 Sensor
- PIR HC-SR501
- LM393 Obstacle Sensor
- OLED SSD1306 Display
- Relay Module
- Active Buzzer
- LEDs
- Breadboard
- Jumper Wires

## Software Components

- Arduino IDE
- Blynk Cloud
- ESP32 Libraries
- DHT Library
- Adafruit SSD1306 Library

## Edge Intelligence

A lightweight risk scoring mechanism is implemented:

| Condition | Score |
|------------|---------|
| Humidity > 65% | +40 |
| Rodent Detection | +30 |
| Motion Detection | +20 |

### Risk Classification

| Risk Score | Status |
|------------|---------|
| 0 - 29 | Safe |
| 30 - 59 | Warning |
| 60 - 100 | Danger |

The ESP32 uses this score to make autonomous decisions without relying on cloud processing.

---

# 9. Results and Demo

## Scenario 1: Normal Operation

- Temperature and humidity displayed on OLED.
- Dashboard updates in real time.
- Green LED indicates safe status.

## Scenario 2: Rodent Detection

- LM393 sensor detects an object.
- Buzzer activates.
- Red LED flashes.
- Alert notification sent to Blynk.

## Scenario 3: High Humidity

- Humidity rises above threshold.
- Relay activates ventilation fan.
- Dashboard status changes to warning.

## Scenario 4: Internet Disconnection

- ESP32 continues local monitoring.
- OLED remains operational.
- Automatic reconnection attempts are performed.

---

# 10. Security and Ethics Considerations

## Security by Design

- Secure Wi-Fi authentication.
- Protected Blynk authentication token.
- Access control for dashboard commands.
- Automatic Wi-Fi reconnection.
- Local decision making to reduce cloud dependency.

## Privacy by Design

- No cameras are used.
- No audio recordings are collected.
- Only environmental and system status data are processed.

## Ethical Considerations

- Reduces dependence on chemical rodent control.
- Minimizes agricultural product losses.
- Supports sustainable farming practices.
- Improves workplace safety through early warning systems.

---

# 11. Limitations and Future Work

## Current Limitations

- DHT11 has limited accuracy.
- LM393 cannot uniquely identify rodents.
- Dependence on Wi-Fi for remote monitoring.
- Limited historical data storage.

## Future Improvements

- Replace DHT11 with DHT22 or SHT31.
- Integrate machine learning for rodent behavior prediction.
- Add solar-powered operation.
- Implement local database storage.
- Deploy LoRa communication for long-range operation.
- Add predictive analytics for mold risk assessment.

---

# 12. Team Contributions

| Team Member | Responsibilities |
|------------|------------------|


---

# 13. References

1. ESP32 Technical Documentation.
2. DHT11 Sensor Datasheet.
3. PIR HC-SR501 Datasheet.
4. LM393 Obstacle Sensor Documentation.
5. Blynk Cloud Documentation.
6. MQTT Protocol Specification.
7. AIoT System Design Principles.
8. Course Materials: From Sensor to User & Security and Ethics for Data.