# Units of Measure by Example

At some point in the robot programming process you will find yourself needing to work with a united value such as an angle or a distance. One common example is creating a function that moves a motor to a certain angle:

```kotlin
class ArmIO {
    private val motor = TalonFX(1)

    // Use combined PID + Feedforward control to move arm to target.
    // CTRE devices store their angles in rotations, so use that unit.
    fun setTargetAngle(rotations: Double) {
        motor.setControl(PositionVoltage(rotations))
    }
}
```

Elsewhere in the code, you now have to remember that this function expects rotations as its unit of angle. If you elsewhere values from a software vendor that does their work in radians, it can be easy to mix them up:

```kotlin
class Arm: Subsystem {
    val io = ArmIO()

    // Face the same way as the robot gyro.
    fun pivotToGyro() {
        // WPILib gives us radians and degrees options... let's just use radians!
        val rotation = Drivetrain.estimatedPose.rotation.radians

        // Oops! Should have converted to rotations first.
        io.setTargetAngle(rotation)

        // This is what the intention here was:
        // io.setTargetAngle(rotation / TAU)
    }
}
```

## Introducing WPILib Units

WPILib Units aims to make this mistake much harder by automatically converting between different units depending on what you need at the time. For example, if you store an `Angle` value made from radians or degrees, you can use it as a rotations value without needing to know what unit the measure started out in:

```kotlin
val angle = Degrees.of(360.0)
val angle2 = Rotations.of(1.0)

assert(angle.`in`(Rotations) == 1)
assert(angle == angle2)
```

Let's update our original code to take advantage of this feature:

```kotlin
class ArmIO {
    private val motor = TalonFX(1)

    // Use combined PID + Feedforward control to move arm to target.
    fun setTarget(angle: Angle) {
        val rotations = angle.`in`(Rotations)
        motor.setControl(PositionVoltage(rotations))
    }
}
```

Now, we can update our subsystem to specify our unit of preference:

```kotlin
class Arm: Subsystem {
    val io = ArmIO()

    fun pivotToGyro() {
        val rotation = Drivetrain.estimatedPose.rotation.radians

        io.setTarget(Radians.of(rotation))
    }
}
```

Nice! This make the subsystem a bit more robust; there's now one less thing to be thinking about when adding on to this part of the code. It's also nice to be able to read what units we're using right in the robot code itself.

## Ergonomics

Converting to and from an `Angle` value everywhere can make the code get pretty verbose, so there's a few ways to cut it down. The most important is that some vendors have added support for using WPILib Units values directly with their APIs. Cross The Road Electronics (CTRE)—who makes the Talon FX motor controller—is one of these vendors, meaning we can simplify our subsystem I/O code by using the angle value directly:

```kotlin
class ArmIO {
    private val motor = TalonFX(1)

    // Use combined PID + Feedforward control to move arm to target.
    fun setTarget(angle: Angle) {
        motor.setControl(PositionVoltage(angle))
    }
}
```

That's a lot better, on par with the simplicity of the first draft of `ArmIO`.

WPILib itself has also been continually adding support for its Units library throughout the codebase. This means we can request a generic `Angle` instead of a specific unit in some places, like in our subsystem code:

```kotlin
class Arm: Subsystem {
    val io = ArmIO()

    fun pivotToGyro() {
        io.setTarget(
            // Returns an `Angle` rather than a `Double`
            Drivetrain.estimatedPose.rotation.measure
        )
    }
}
```

### First-party addons

Using Kotlin also allows us to use an improved, shorthand syntax for creating units:

```kotlin
// Old:
val speed = MetersPerSecond.of(20.0)
// New:
val speed = 20.metersPerSecond
```
