# Naive closed-loop

Perhaps a better solution is to use conditional statements to target a certain point then. We can achieve this by using sensors, such as the one built into the motor!

```cpp
const double TARGET = 3.333;
motor.move_velocity(200);
while(motor.get_position() < TARGET) pros::delay(10); // Assume get_position is in revolutions
motor.brake();
```

This might be a little bit better, especially in cases where the motor takes a while to get up to speed, but theres a notable flaw in this: it will *always* overshoot.
This is just the natural consequence of waiting until the position gets past the target to stop, since braking still takes some distance.

Ok, well, if its overshooting, then we just need to turn the motor back the other way when that happens! Lets try this:

> ![NOTE]
> For the sake of keeping this concise, this code is going to have a completely different use case from the code block further up this page. Particularly, we're going
> to use an infinite loop here, since an exit condition is not relevant to the topic at hand.

```cpp
const double TARGET = 3.333;
while(true) {
    pros::delay(10);
    double pos = motor.get_position();
    if(pos < TARGET) motor.move_velocity(200);
    else if (pos > TARGET) motor.move_velocity(-200);
    else motor.brake();
}
```

Ok great, now we have closed loop code that will target a position and work towards it! However, its not the best algorithm ever. It will attempt to go full power towards the target point, even if its really close, which will then cause it to overshoot and repeat from the other side, potentially forever, creating a very shaky robot. This is not ideal. Fortunately, there are better algorithms out there.