# Time based/open-loop

Open-loop control is a control method which uses *no feedback* to change the output parameters based on external conditions. In other words, the system will just output values, typically motor voltages or velocities, without any consideration to what else is happening around the robot.

# Time-based control

The most common form of open-loop control in Vex is time-based control. Time-based control isn't great, but, used correctly, can still be decently powerful.
Typically, time based control consists of setting motor values, waiting a specified duration, then turning the motors off, like such:

```cpp
motor.move_velocity(200);
pros::delay(1000);
motor.brake();
```

The above code should move the motor approximately `3.33` revolutions, at least in theory. However, this can be imprecise for a number of reasons. The most obvious is that it ignores acceleration and deceleration; the motor is just trying to target a speed, and then it is trying to target a different speed 1 second later. If it can reach both instantaneously, and it is able to maintain them, then great, we have a working algorithm. However, unfortunately, the laws of physics prohibit this.