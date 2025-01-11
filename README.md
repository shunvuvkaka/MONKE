# MONKE
OO OO AA AA


Your code for `MAXSwerveModule` appears well-structured overall. However, there are potential issues or things to check for:

---

### **1. MotorType Selection**
```java
m_drivingSparkMax = new CANSparkMax(drivingCANId, MotorType.kBrushed);
m_turningSparkMax = new CANSparkMax(turningCANId, MotorType.kBrushed);
```
- **Issue:** Are you certain your motors are brushed? Many FRC teams use brushless motors (e.g., NEOs) with REV hardware. If you're using brushless motors, replace `MotorType.kBrushed` with `MotorType.kBrushless`.

---

### **2. Absolute Encoder Type and Configuration**
```java
m_turningEncoder = m_turningSparkMax.getAbsoluteEncoder(Type.kDutyCycle);
```
- **Issue:** Ensure your encoder is of type `kDutyCycle`. If it’s not, verify its type (`kPWM`, `kAnalog`, etc.) and update this accordingly.

- Also, ensure your encoder’s physical connection and settings match the software configuration.

---

### **3. PID Constants**
```java
private static final double DRIVING_P = 0.1; // Example value, update with actual PID constant
private static final double DRIVING_I = 0.0; 
private static final double DRIVING_D = 0.0; 
private static final double DRIVING_FF = 0.0; 
```
- **Issue:** These are placeholders. You’ll need to tune these values on the robot to achieve smooth and accurate control.

- Use tools like REV’s `SparkMax Client` or WPILib’s logging to monitor PID behavior during testing and adjust these values.

---

### **4. Conversion Factors**
```java
private static final double DRIVING_POSITION_CONVERSION_FACTOR = (Math.PI * WHEEL_DIAMETER) / GEAR_RATIO;
private static final double DRIVING_VELOCITY_CONVERSION_FACTOR = (Math.PI * WHEEL_DIAMETER) / (60 * GEAR_RATIO);
```
- **Issue:** Verify `WHEEL_DIAMETER` and `GEAR_RATIO` values are accurate for your robot's drivetrain.

---

### **5. Chassis Angular Offset**
```java
private double m_chassisAngularOffset = 0;
```
- **Issue:** If the offset isn't properly initialized or calibrated, it can cause misalignment. Make sure you measure and set this offset based on your robot's orientation during initialization.

---

### **6. Optimization Logic**
```java
SwerveModuleState optimizedDesiredState = SwerveModuleState.optimize(correctedDesiredState,
  new Rotation2d(m_turningEncoder.getPosition()));
```
- **Issue:** The `optimize` function ensures the module rotates the shortest path. If the behavior isn’t as expected, debug `m_turningEncoder.getPosition()` to confirm its output matches expectations.

---

### **7. Voltage Monitoring Methods**
```java
public double getDrivingVoltage(){
  return m_drivingSparkMax.getOutputCurrent();
}

public double getTurningVoltage(){
  return m_turningSparkMax.getOutputCurrent();
}
```
- **Issue:** These methods return current (amperage), not voltage. If you intended to monitor voltage, use `m_drivingSparkMax.getBusVoltage()` or similar.

---

### **8. General Safety**
- Ensure you set appropriate **current limits** and validate all wiring, connections, and configurations before testing on hardware.

---

### Suggested Debugging Steps:
1. **Verify Hardware:**
   - Ensure CAN IDs and motor types match the physical hardware.
2. **Simulation/Test:**
   - If using WPILib simulation, verify outputs and states behave as expected in simulation.
3. **Live Tuning:**
   - Use REV’s `SparkMax Client` to tune and monitor behavior live.

If any specific error messages appear during deployment or testing, share them for further analysis!
