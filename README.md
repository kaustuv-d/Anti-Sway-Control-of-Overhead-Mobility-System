#  Anti-Sway Control System for Suspended Payloads

This repository contains the design, implementation, and testing of an **Anti-Sway Control System** developed as part of the **Instrumentation and Control Systems** course at **Indian Institute of Technology Indore**. The project focuses on stabilizing suspended payloads using a real-time PID control mechanism integrated with a custom-built H-belt drive system.

---

## Abstract

The project aims to reduce sway in suspended payloads (like crane systems) by applying corrective actions using a **PID controller**. The controller reads angular displacement via an **MPU-6050** sensor and drives **stepper motors** to reposition the suspension point using an **H-belt drive mechanism**. The system is capable of reducing oscillations by up to **90%**, improving safety and operational efficiency in suspended load systems.

---

## Components Used

* Arduino Uno
* Raspberry Pi (for supervision/interface)
* MPU-6050 (6-axis IMU sensor)
* NEMA 17 Stepper Motors (x2)
* A4988 Stepper Motor Drivers (x2)
* H-Belt drive system (custom-built)
* 2-axis Joystick module

---

## ⚙️ Control System

### PID Controller Design

A classical **PID control algorithm** was implemented to minimize the deviation ($\theta$) of the payload from the vertical:

```
u(t) = Kp * e(t) + Ki * integral(e(t)) + Kd * derivative(e(t))
```

Each axis (roll and pitch) is controlled using separate PID loops. Parameters were tuned using the **Ziegler-Nichols method** followed by iterative refinement.

---

## 🛠️ Implementation Details

### 1. Sensor and Hardware Integration

* The **MPU-6050** sensor is rigidly mounted on the payload using a custom 3D-printed bracket to reduce measurement errors.
* Sensor data is transmitted to the Arduino Uno via **I2C** protocol for real-time control.

![MPU-6050 Mounted on Payload](media/mpu_mount.jpg)

### 2. Angle Estimation using Complementary Filter

* The accelerometer provides a noisy but drift-free estimate.
* The gyroscope offers fast dynamic response but accumulates drift.
* A **complementary filter** is used to combine both, ensuring stable and accurate **roll and pitch** angles.

```c
angle = alpha * (gyro_angle) + (1 - alpha) * (accel_angle);
```

### 3. PID Control Logic

* Independent PID loops are implemented for roll and pitch.
* Each controller adjusts the **position of the suspension point** to counteract the sway.

### 4. H-Belt Drive Mechanism

* The motors drive a **continuous H-pattern belt** that translates PID commands to **X-Y movement** of the suspension point.
* Stepper motors are operated with **1/16 microstepping**, giving a high linear resolution (\~6.25µm/microstep).

![H-Belt Drive Schematic](media/hbelt_diagram.png)

* **Kinematics**:

  ```
  Motor1 = X + Y
  Motor2 = X - Y
  ```

### 5. Joystick-Based Manual Override

* A **joystick** allows manual control of the suspension point.
* Useful for:

  * Initial positioning
  * Manual testing
  * Inducing controlled disturbances for PID evaluation

---

## Experimental Validation

### Test Setup

* Adjustable suspension cable (0.5–1.5 m)
* Payloads: 2–4 kg
* Initial disturbances: 5°, 10°, 15°
* Testing both **open-loop** and **closed-loop** behavior

### Results

* **Sway reduction up to 90%** compared to uncontrolled motion
* Stable damping and smooth transition between manual and automatic control

---

## Future Improvements

### Control Algorithms

* Model Predictive Control (MPC)
* Adaptive and Learning-Based Controllers

### Hardware Upgrades

* Industrial-grade IMUs
* Load cells for real-time payload feedback
* Larger drive area for heavy-duty operation

### Software Enhancements

* ML-based trajectory optimization
* Real-time system identification

---

## References

* [Anand Control – EOT Crane Anti-Sway](https://www.anandcontrol.in/blog/eot_crane_anti_sway_control.html)
* [Ziegler–Nichols Method – ScienceDirect](https://www.sciencedirect.com/topics/computer-science/ziegler-nichols-method)
* [Springer Research Article](https://link.springer.com/article/10.1007/s42452-021-04793-0)

---

## 👥 Team Members

* Kaustuv Devmishra
* Kshitij Shetty
* Prachi Patil
* Mihir Hemani
* Jatin Joshi
* Krishan Swami
