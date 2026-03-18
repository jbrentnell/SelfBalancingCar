# Self-Balancing Car

![Arduino](https://img.shields.io/badge/Arduino-Compatible-green.svg)
![License](https://img.shields.io/badge/License-MIT-blue.svg)
![Hardware](https://img.shields.io/badge/Hardware-Robotics-orange.svg)

A sophisticated self-balancing two-wheeled robot built with Arduino, featuring multiple autonomous modes, sensor fusion, and real-time PID control.

## 🤖 Project Description

This Self-Balancing Car is an inverted pendulum robot that maintains its balance on two wheels using advanced control algorithms. The project demonstrates key robotics concepts including sensor fusion, PID control, motor control, and autonomous navigation.

The robot uses a MPU6050 6-axis IMU sensor for balance detection, ultrasonic and infrared sensors for obstacle detection, and features multiple operating modes including obstacle avoidance, object following, and remote control.

## 🚀 Key Features

- **Self-Balancing**: Maintains balance using PID control algorithms with MPU6050 IMU
- **Multiple Operating Modes**:
  - Obstacle Avoidance Mode
  - Object Following Mode (2 variants)
  - Bluetooth Remote Control
  - IR Remote Control
  - Standby/Idle Mode
- **Sensor Fusion**: Combines data from IMU, ultrasonic, and IR sensors
- **Real-time Control**: 5ms control loop with encoder feedback
- **Status Indicators**: RGB LED status lights for different modes
- **Battery Monitoring**: Low-voltage protection and monitoring
- **Motor Control**: Precise speed and direction control with encoders

## 📋 Hardware Requirements

### Core Components
- **Microcontroller**: Arduino Uno/Nano/Pro Mini
- **Motor Driver**: L298N H-bridge motor driver
- **IMU Sensor**: MPU6050 (6-axis Accelerometer + Gyroscope)
- **Distance Sensor**: HC-SR04 Ultrasonic Sensor
- **IR Sensors**: 2x IR receiver modules + IR LED
- **LEDs**: WS2812B RGB LED strip (4 LEDs)
- **Motors**: 2x DC gear motors with encoders
- **Power**: 7.4V Li-ion battery pack

### Additional Components
- Motor mounting brackets
- Wheels (2x)
- Chassis frame
- Resistors (220Ω, 1kΩ)
- Capacitors (100nF)
- Breadboard/PCB for connections
- Jumper wires

## 🔧 Circuit Diagram

```
Arduino Connections:
┌─────────────────┬─────────────────┬─────────────────┐
│   Arduino       │   Components    │   Purpose       │
├─────────────────┼─────────────────┼─────────────────┤
│ D2              │ Encoder Left A  │ Speed Feedback  │
│ D3              │ RGB LED Data    │ Status Lights   │
│ D4              │ Encoder Right A │ Speed Feedback  │
│ D5              │ PWM Left Motor  │ Motor Control   │
│ D6              │ PWM Right Motor │ Motor Control   │
│ D7              │ IN1 (Left)      │ Motor Direction │
│ D8              │ STBY            │ Motor Enable    │
│ D9              │ IR Send         │ IR Transmission │
│ D10             │ Key Mode Input  │ Mode Selection  │
│ D11             │ Ultrasonic Trig │ Distance Meas.  │
│ D12             │ IN2 (Right)     │ Motor Direction │
│ A0              │ IR Left Receive │ Obstacle Detect │
│ A1              │ IR Right Receive│ Obstacle Detect │
│ A2              │ Voltage Measure │ Battery Monitor │
│ A3              │ Ultrasonic Echo │ Distance Meas.  │
└─────────────────┴─────────────────┴─────────────────┘
```

## 📝 Installation & Setup

### 1. Hardware Assembly
1. Mount the Arduino and motor driver on the chassis
2. Install the MPU6050 IMU sensor securely
3. Attach the ultrasonic sensor at the front
4. Position IR sensors on left and right sides
5. Connect the RGB LED strip
6. Wire the motors with encoders
7. Connect the battery with voltage monitoring

### 2. Software Installation
1. Install Arduino IDE
2. Install required libraries:
   - `Adafruit_NeoPixel`
   - `Wire` (built-in)
3. Download the project files
4. Upload the code to your Arduino

### 3. Calibration
1. Place the robot on a flat surface
2. Upload the code and wait for initialization
3. The robot will automatically calibrate the IMU
4. Adjust PID parameters if needed (see Tuning section)

## 🎮 Usage Instructions

### Power On Sequence
1. Connect the battery
2. The robot will initialize and beep
3. Wait for the calibration sequence (2 seconds)
4. The robot will balance itself automatically

### Control Modes

#### 1. Obstacle Avoidance Mode (Key '2')
- Robot autonomously avoids obstacles
- Uses ultrasonic and IR sensors
- RGB LEDs flash yellow during operation

#### 2. Object Following Mode (Key '1' and '0')
- Robot follows objects in front of it
- Maintains safe distance
- RGB LEDs flash green during operation

#### 3. Bluetooth Control (Key 's' + 'f'/'b'/'l'/'i')
- Connect via Bluetooth serial
- 'f' = Forward, 'b' = Backward
- 'l' = Turn Left, 'i' = Turn Right
- 's' = Stop

#### 4. IR Remote Control (Key 's' + remote buttons)
- Use IR remote for manual control
- Standard remote control functions

#### 5. RGB Light Effects (Key '3')
- Cycle through various LED light effects
- Press '3' repeatedly to change effects

#### 6. Manual Control Keys
- 'f' = Move Forward
- 'b' = Move Backward  
- 'l' = Turn Left
- 'i' = Turn Right
- 's' = Stop/Standby

### Emergency Stop
- Press '4' to trigger emergency stop
- Robot will back up and stop if balance is lost
- Use '5' to restart balancing

## ⚙️ Code Architecture

The project uses a modular design with separate header files for each subsystem:

### Core Files
- **`Tumbller.ino`** - Main program loop and state machine
- **`BalanceCar.h`** - PID control and motor management
- **`mode.h`** - Operating mode definitions
- **`Pins.h`** - Pin configuration

### Subsystem Modules
- **`Command.h`** - Input handling (Bluetooth, IR remote)
- **`Rgb.h`** - RGB LED control and effects
- **`Ultrasonic.h`** - Distance measurement and obstacle detection
- **`voltage.h`** - Battery monitoring and protection
- **`KalmanFilter.h`** - Sensor fusion algorithm

### Control System
The robot uses a dual-loop PID control system:
1. **Balance Loop**: Maintains upright position using IMU data
2. **Speed Loop**: Controls forward/backward movement
3. **Rotation Loop**: Controls turning

## 🔧 PID Tuning Guide

The balance performance depends on these PID parameters in `BalanceCar.h`:

```cpp
// Balance Control Parameters
double kp_balance = 55;    // Proportional gain for balance
double kd_balance = 0.75;  // Derivative gain for balance

// Speed Control Parameters  
double kp_speed = 10;      // Proportional gain for speed
double ki_speed = 0.26;    // Integral gain for speed

// Rotation Control Parameters
double kp_turn = 2.5;      // Proportional gain for turning
double kd_turn = 0.5;      // Derivative gain for turning
```

### Tuning Process
1. **Start with Balance Loop**:
   - Increase `kp_balance` until robot starts to oscillate
   - Add `kd_balance` to dampen oscillations
   - Find the sweet spot where robot balances smoothly

2. **Tune Speed Loop**:
   - Adjust `kp_speed` for responsive movement
   - Use `ki_speed` to eliminate steady-state error
   - Too much integral gain causes overshoot

3. **Fine-tune Rotation**:
   - Adjust `kp_turn` and `kd_turn` for smooth turning
   - Ensure robot doesn't wobble during turns

### Balance Angle Limits
```cpp
char balance_angle_min = -22;  // Minimum safe angle
char balance_angle_max = 22;   // Maximum safe angle
```

## 🚨 Troubleshooting

### Robot Won't Balance
- **Check IMU calibration**: Ensure MPU6050 is properly mounted
- **Verify center of gravity**: Battery and components should be centered
- **Adjust PID parameters**: Start with lower gains
- **Check motor wiring**: Ensure correct polarity

### Erratic Behavior
- **Loose connections**: Check all wiring
- **Power issues**: Ensure battery is fully charged
- **Sensor interference**: Keep sensors clean and unobstructed
- **Encoder issues**: Verify encoder connections

### Motor Problems
- **No movement**: Check motor driver connections
- **One motor not working**: Verify wiring and power
- **Jittery movement**: Check encoder feedback

### Sensor Issues
- **Ultrasonic not working**: Check trigger/echo connections
- **IR sensors false triggers**: Adjust sensitivity or shielding
- **IMU calibration failed**: Re-upload code and recalibrate

## 🤝 Contributing

Contributions are welcome! Please:
1. Fork the repository
2. Create a feature branch
3. Test your changes thoroughly
4. Submit a pull request with clear description

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- MPU6050 library by Electronic Cats
- Adafruit NeoPixel library
- PID control theory and implementation
- Open-source robotics community

---

**Note**: This is an advanced robotics project. Ensure proper safety precautions when operating. Always supervise the robot during operation.