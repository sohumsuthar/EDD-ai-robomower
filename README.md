# EDD AI Robomower

An autonomous robotic lawn mower prototype designed to run on a Raspberry Pi. This project was developed as part of an Engineering Design and Development (EDD) course.

## Overview

The robomower uses a differential-drive system controlled by a PCA9685 PWM driver, an inertial measurement unit (IMU) for orientation tracking, and a camera feed paired with a machine learning model to distinguish grass from obstacles. PID controllers keep the mower driving straight by continuously correcting motor speeds based on IMU feedback.

### Features

- **Differential drive control** — forward, reverse, left, and right movement via two drive motors
- **PID-based steering correction** — dual PID controllers adjust motor speeds in real time to maintain heading
- **Inertial localization** — dead-reckoning position estimation from IMU orientation data
- **Vision-based obstacle avoidance** — camera frames are classified by an ML model to detect non-grass regions and trigger evasive maneuvers
- **Autonomous mowing loop** — continuously localizes, avoids obstacles, and mows in a pattern while tracking covered area

## Hardware Requirements

- Raspberry Pi (any model with GPIO header)
- PCA9685 16-channel PWM driver board
- Two DC drive motors and one mower blade motor
- Inertial measurement unit (IMU / inertial sensor)
- Camera module for video feed

## Software Dependencies

- Python 3
- `RPi.GPIO`
- `adafruit-circuitpython-pca9685` (Adafruit PCA9685 library)
- `adafruit-blinka` (for `board` and `busio`)
- `numpy`
- A PID controller library (referenced as `PID`)
- Custom modules: `pca9685`, `inertial_sensor`, `video_feed`

## Project Structure

```
mower          — Main LawnMower class with drive, mow, localize, and obstacle avoidance logic
PID control    — Standalone PID control loop for dual-motor orientation correction
```

## Usage

These scripts are intended to run on a Raspberry Pi with the required hardware connected:

```bash
python3 mower
python3 "PID control"
```

> **Note:** External modules (`pca9685`, `inertial_sensor`, `video_feed`, `PID`) must be available on the system. A `requirements.txt` is not yet provided.

## How It Works

1. The `LawnMower` class is initialized with a motor controller, inertial sensor, and video feed.
2. The main `mow_lawn()` loop continuously:
   - Reads the IMU to estimate the current position.
   - Captures a camera frame and runs an ML model to check for obstacles.
   - Drives forward and mows, turning periodically to cover the lawn.
   - Tracks which areas have been mowed.
3. PID controllers correct steering drift by adjusting individual motor speeds based on the difference between the desired heading and the current IMU reading.

## Status

This project is in early prototype / proof-of-concept stage. Several components (ML model integration, full localization, external module implementations) are still under development.

## License

No license specified.
