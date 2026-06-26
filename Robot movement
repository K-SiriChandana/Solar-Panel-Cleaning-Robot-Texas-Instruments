// ============================================================
//  SOLAR PANEL CLEANING ROBOT — Arduino UNO
//  Subsystems:
//    1. Electrical Monitoring  (INA219)
//    2. Contamination Detection (IR reflective + LDR)
//    3. Thermal Monitoring      
//    4. Locomotion              (AFMotor DC motors)
//    5. Edge Detection          (IR edge sensors — 2 pins)
//    6. Cleaning System         (Pump motor via AFMotor)
//    7. Decision & Control      (Autonomous )
// ============================================================

#include <AFMotor.h>
#include <Wire.h>
//#include <Adafruit_INA219.h>

// ─────────────────────────────────────────────
// 1. MOTOR OBJECTS
// ─────────────────────────────────────────────
AF_DCMotor right_motor(1, MOTOR12_8KHZ);
AF_DCMotor left_motor (2, MOTOR12_8KHZ);
AF_DCMotor pump_motor (3, MOTOR12_8KHZ);

#define MOTOR_SPEED_NORMAL    200
#define MOTOR_SPEED_SLOW      130
#define CLEAN_DURATION_MS     5000  // ms pump runs per cleaning pass
#define TURN_DELAY_MS         600   // ms for 90° turn
#define STEP_DELAY_MS         400   // ms per forward step


enum RobotState {
  STATE_IDLE,
  STATE_MOVING,
  STATE_CLEANING,
  STATE_TURNING,
  STATE_EDGE_AVOID,
  STATE_FAULT
};

RobotState currentState = STATE_IDLE;


void moveForward(uint8_t spd = MOTOR_SPEED_NORMAL) {
  right_motor.setSpeed(spd);
  left_motor.setSpeed(spd);
  right_motor.run(FORWARD);
  left_motor.run(FORWARD);
}

void moveBackward(uint8_t spd = MOTOR_SPEED_NORMAL) {
  right_motor.setSpeed(spd);
  left_motor.setSpeed(spd);
  right_motor.run(BACKWARD);
  left_motor.run(BACKWARD);
}

void turnLeft(uint8_t spd = MOTOR_SPEED_NORMAL) {
  right_motor.setSpeed(spd);
  left_motor.setSpeed(spd);
  right_motor.run(FORWARD);
  left_motor.run(BACKWARD);
}

void turnRight(uint8_t spd = MOTOR_SPEED_NORMAL) {
  right_motor.setSpeed(spd);
  left_motor.setSpeed(spd);
  right_motor.run(BACKWARD);
  left_motor.run(FORWARD);
}

void stopMotors() {
  right_motor.run(RELEASE);
  left_motor.run(RELEASE);
}



void setup() {
  Serial.begin(9600);
  Wire.begin();

  // Motor defaults
  right_motor.setSpeed(MOTOR_SPEED_NORMAL);
  left_motor.setSpeed(MOTOR_SPEED_NORMAL);
  pump_motor.setSpeed(255);
  stopMotors();

  Serial.println("==============================");
  Serial.println(" Solar Panel Cleaning Robot");

}

// ─────────────────────────────────────────────
// MAIN LOOP
// ─────────────────────────────────────────────
void loop() {
 
 
   
  }
}
