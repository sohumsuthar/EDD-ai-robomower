# CLAUDE.md

## Project Overview

EDD-ai-robomower is a prototype autonomous robotic lawn mower project built for a Raspberry Pi platform. It combines motor control, inertial navigation, computer vision-based obstacle avoidance, and PID-based steering correction to autonomously mow a lawn.

This is an early-stage / proof-of-concept project. The code files are Python scripts (without `.py` extensions) and contain class stubs and prototype logic rather than a fully runnable application.

## Tech Stack

- **Language:** Python 3
- **Target Platform:** Raspberry Pi (uses `RPi.GPIO`)
- **Hardware Interfaces:**
  - PCA9685 PWM driver (via `adafruit_pca9685` / custom `pca9685` module) for motor control
  - Inertial sensor (IMU) for orientation/localization
  - Camera video feed for obstacle detection
- **Key Libraries:**
  - `RPi.GPIO` — Raspberry Pi GPIO access
  - `adafruit_pca9685` / `pca9685` — PWM servo/motor driver
  - `busio` / `board` — I2C communication (Adafruit Blinka)
  - `numpy` — numerical computation for position estimation
  - `PID` — PID controller (external or custom module)
  - Computer vision model (referenced as `self.model.predict()`) — likely an ML model for grass vs. non-grass classification

## Directory Structure

```
EDD-ai-robomower/
├── mower              # Main LawnMower class — motor control, localization,
│                      #   obstacle avoidance, mowing loop, PID-based steering
├── PID control        # Standalone PID control loop script for dual-motor
│                      #   orientation correction using PCA9685
├── README.md          # Project README
└── CLAUDE.md          # This file
```

## Key Files

- **`mower`** — Contains the `LawnMower` class with methods for:
  - `drive(speed, direction)` — differential drive with PID correction
  - `mow(speed)` — controls the mower blade motor (motor channel 2)
  - `localize()` — dead-reckoning position from IMU orientation
  - `avoid_obstacles()` — camera-based obstacle detection using an ML model
  - `mow_lawn()` — main autonomous mowing loop
  - `is_on_grass()` — binary grass/non-grass classification from camera

- **`PID control`** — A standalone script demonstrating dual PID controllers reading from an inertial sensor and driving two motors to maintain orientation.

## Build / Run

There is no build system, test suite, or `requirements.txt` at this time. The project consists of raw Python scripts intended for a Raspberry Pi with the appropriate hardware connected.

To run (on a properly configured Raspberry Pi with hardware attached):
```bash
python3 mower
python3 "PID control"
```

Note: The files lack `.py` extensions, which may cause import issues. Several imported modules (`pca9685`, `inertial_sensor`, `video_feed`, `PID`) are not included in this repository and would need to be provided separately or installed.

## Coding Conventions

- No `.py` file extensions on Python source files
- Class-based design for the main mower logic
- PID tuning constants are hardcoded (kp=0.5/1.0, ki=0.1, kd=0.01)
- Multiple `drive()` method definitions exist in the `mower` file (the last definition wins in Python) — only the PID-based version at the bottom is actually active
- Spaces in filenames (e.g., `PID control`)

## Important Notes

- The `mower` file defines `drive()` three times. In Python, only the last definition is used. The first two are effectively dead code.
- `self.model` is referenced in `avoid_obstacles()` and `is_on_grass()` but is never initialized in `__init__()`. This would raise an `AttributeError` at runtime.
- The `localize()` method uses a simplistic dead-reckoning approach (cos/sin of IMU orientation) without integrating velocity or tracking cumulative displacement.
- Several external modules (`pca9685`, `inertial_sensor`, `video_feed`, `PID`) are imported but not included in the repository.
- There are no tests, no dependency management files, and no CI/CD configuration.
