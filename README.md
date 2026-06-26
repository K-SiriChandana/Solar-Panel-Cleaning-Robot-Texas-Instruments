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
