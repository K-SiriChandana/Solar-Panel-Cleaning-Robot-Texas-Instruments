# Autonomous Solar Panel Cleaning Robot

An autonomous embedded robotic system designed to detect contamination on photovoltaic (PV) panels, perform efficient cleaning, and verify panel performance while minimizing water usage and human intervention.

> Developed as part of the **Texas Instruments WiSH'26 Design Thinking Project**.

---

## Overview

Solar panels lose efficiency due to dust accumulation, bird droppings, and other contaminants. Traditional cleaning methods are labor-intensive, consume excessive water, and require regular manual inspection.

This project proposes an autonomous solar panel cleaning robot capable of:

- Detecting contaminated regions on solar panels
- Navigating safely across panel surfaces
- Performing dual-mode cleaning (dry + wet)
- Detecting potential dead cells using thermal sensing
- Verifying cleaning effectiveness after every cleaning cycle

The system follows a closed-loop workflow:

```
Sense → Analyze → Clean → Verify
```

---

## Problem Statement

The robot addresses four major challenges in solar panel maintenance:

- Efficient contamination detection
- Water-efficient cleaning
- Autonomous navigation and edge detection
- Differentiating dirt-induced power loss from actual panel faults

---

## Features

- Autonomous panel navigation
- Encoder-based position tracking
- IR-based edge detection
- Dual-mode cleaning
  - Dry brush cleaning
  - Targeted wet cleaning
- Electrical performance monitoring
- Thermal fault detection
- Post-cleaning verification
- Modular embedded software architecture

---

## System Workflow

```text
Power On
    │
    ▼
System Health Check
    │
    ▼
Panel Performance Analysis
    │
    ▼
Dirty Panel?
 ├── No → Generate Report
 └── Yes
        │
        ▼
 Dry Cleaning
        │
        ▼
Wet Cleaning (if required)
        │
        ▼
Performance Verification
        │
        ▼
Dead Cell Detection
        │
        ▼
Completion Report
```

---

# Hardware Components

## Microcontroller

- MSP430FR2433

## Sensors

- INA219 Current & Voltage Sensor
- TCRT5000 IR Reflective Sensor
- MLX90614 Infrared Temperature Sensor
- LDR Sensor
- IR Edge Detection Sensors
- Wheel Encoders

## Actuators

- N20 12V DC Geared Motors with Encoders
- DRV8871 Motor Driver
- Brush Motor
- Water Pump

## Power System

- 12V Lithium-Ion Battery Pack
- LM2596 Buck Converter

---

# Sensor Responsibilities

| Sensor | Purpose |
|---------|----------|
| INA219 | Measures panel voltage and current to estimate efficiency |
| TCRT5000 | Detects surface contamination |
| LDR | Measures ambient light conditions |
| MLX90614 | Detects abnormal temperature indicating dead cells |
| IR Edge Sensors | Detects panel boundaries and prevents falls |
| Wheel Encoders | Position tracking and distance estimation |

---

# Navigation

The robot autonomously traverses the solar panel using:

- Differential drive mechanism
- Wheel encoder-based odometry
- IR-based edge detection
- Panel coverage algorithm

This enables complete panel coverage while preventing the robot from falling off panel edges.

---

# Cleaning Strategy

### Phase 1 – Inspection

- Scan panel
- Measure electrical output
- Identify dirty regions

### Phase 2 – Cleaning

- Dry brush removes loose dust
- Targeted wet cleaning removes stubborn contaminants

### Phase 3 – Verification

- Measure panel performance again
- Compare efficiency before and after cleaning
- Detect abnormal thermal regions indicating possible dead cells

---

# Software Architecture

```text
                  Sensors
                     │
 ┌───────────────────┼───────────────────┐
 │                   │                   │
INA219          TCRT5000           MLX90614
 │                   │                   │
 └──────────────┬────┴───────────────────┘
                │
        MSP430FR2433 MCU
                │
 ┌──────────────┼───────────────┐
 │              │               │
Navigation   Cleaning      Fault Detection
 │              │               │
 └──────────────┼───────────────┘
                │
            Actuators
 ┌──────────────┼───────────────┐
 │              │               │
DC Motors   Brush Motor     Water Pump
```

---

# Project Structure

```text
Solar-Panel-Cleaning-Robot/
│
├── README.md
│
├── docs/
│   ├── Design.pdf
│   ├── Flowcharts/
│   └── Images/
│
├── firmware/
│   ├── navigation/
│   ├── edge_detection/
│   ├── sensors/
│   ├── motor_control/
│   ├── cleaning/
│   ├── fault_detection/
│   └── utilities/
│
├── hardware/
│   ├── schematics/
│   ├── pcb/
│   └── components/
│
└── images/
```

---

# Design Goals

- Improve solar panel efficiency
- Reduce manual maintenance
- Minimize water consumption
- Enable autonomous operation
- Detect potential panel faults
- Reduce maintenance costs

---

# Technologies Used

- Embedded C
- MSP430FR2433
- I²C Communication
- PWM Motor Control
- Wheel Encoders
- IR Sensors
- Current & Voltage Monitoring
- Thermal Sensing

---

# Future Improvements

- Computer vision-based contamination detection
- SLAM-based navigation for large solar farms
- Wireless monitoring dashboard
- AI-assisted predictive maintenance
- Autonomous charging dock
- IoT-enabled remote monitoring

---

# Team

- Siri Chandana
- Aishani Goenka
- Himani Naiknimbalkar
- Shreya Verma
- Sriya Bareddha
- Vaishnavi Katariya

---

# Acknowledgements

This project was developed as part of the **Texas Instruments WiSH'26 Embedded Software Mentorship Program**, applying embedded systems, sensor interfacing, navigation, and autonomous robotics concepts to solve a real-world renewable energy problem.


# Position Tracking and Edge Detection

## Overview

This module enables autonomous navigation of the solar panel cleaning robot by combining **wheel encoder-based position tracking** with **IR sensor-based edge detection**. The robot estimates the distance travelled, detects panel boundaries, and performs strip-by-strip cleaning while preventing falls from the panel.

---

## Features

- Wheel encoder-based position tracking
- Left and right edge detection
- Autonomous strip-by-strip navigation
- Safe movement across solar panels
- Distance estimation using encoder feedback

---

## Hardware Components

### Microcontroller
- MSP430FR2433

### Sensors
- 2 × Wheel Encoders
- 4 × IR Reflective Sensors
  - Front Left (FL)
  - Rear Left (RL)
  - Front Right (FR)
  - Rear Right (RR)

### Actuators
- 2 × DC Geared Motors with Encoders

---

## Position Tracking

The robot estimates its position using wheel encoder feedback.

Each encoder generates pulses as the wheel rotates.

The travelled distance is calculated using:

```
Wheel Circumference = π × Wheel Diameter

Distance per Pulse = Wheel Circumference / Pulses per Revolution

Distance Travelled = Encoder Count × Distance per Pulse
```

### Example

| Parameter | Value |
|-----------|------:|
| Wheel Diameter | 8 cm |
| Wheel Circumference | 25.13 cm |
| Pulses per Revolution | 20 |
| Distance per Pulse | 1.26 cm |

---

## Panel Parameters

The navigation algorithm requires the following panel dimensions:

- Panel Length
- Panel Width
- Brush Width

Example:

| Parameter | Value |
|-----------|------:|
| Panel Length | 200 cm |
| Panel Width | 100 cm |
| Brush Width | 20 cm |

These parameters determine the number of cleaning strips and the sideways movement after each pass.

---

## Edge Detection

IR reflective sensors continuously monitor the solar panel boundaries.

Sensor Output:

| Output | Meaning |
|--------|---------|
| 1 | Solar panel detected |
| 0 | Panel edge detected |

---

## Sensor Placement

```
              FRONT

FL                         FR
↓                          ↓

+-------------------------+
|                         |
|         ROBOT           |
|                         |
+-------------------------+

↓                          ↓
RL                         RR

              REAR
```

All IR sensors are mounted underneath the robot and face the solar panel surface.

---

## Navigation Algorithm

1. Robot starts at one corner of the panel.
2. Wheel encoders continuously measure travelled distance.
3. IR sensors monitor the left and right boundaries.
4. When an edge is detected, the robot stops and performs a controlled turn.
5. After completing one strip, the robot shifts sideways by one brush width.
6. The process repeats until the entire panel is cleaned.

---

## Working Principle

```
Wheel Rotation
      ↓
Encoder Pulses
      ↓
Encoder Count
      ↓
Distance Calculation
      ↓
Position Estimation
```

```
IR Sensor
      ↓
Panel Present?
      ↓
Yes ─────────► Continue Cleaning

No
↓
Edge Detected
↓
Stop Robot
↓
Turn
↓
Start Next Cleaning Strip
```

---

## Future Enhancements

- Closed-loop PID motor control using encoder feedback
- ToF sensor integration for improved edge detection
- IMU-based heading correction
- Adaptive path planning for different panel sizes
- Obstacle detection and avoidance

---

## Technologies Used

- MSP430FR2433
- Embedded C
- Wheel Encoders
- IR Reflective Sensors
- GPIO
- Timers
- Interrupts
- PWM

---

## Applications

- Autonomous rooftop solar panel cleaning
- Commercial photovoltaic maintenance
- Low-power embedded robotic systems
