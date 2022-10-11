# Commands and Subsystems

WPILib supports two different programming paradigms,
referred to as *imperative* and *command-based* programming.
Most teams' preferred programming paradigm is *command-based*,
because it is more flexible and easier to maintain.
Unfortunately, it is also a little more difficult to learn.

Command-based programming uses
three main abstractions:
*subsystems*, *commands*, and *inputs*.

## Subsystems

A subsystem encapsulates related lower-level robot mechanisms
and defines the way the rest of the robot code interacts with them.
For example,
a subsystem might represent
the motors and encoders in a drivetrain
or the solenoids and motors in a deployable intake.

In WPILib, subsystems implement the [`Subsystem`](https://first.wpi.edu/wpilib/allwpilib/docs/release/java/edu/wpi/first/wpilibj2/command/Subsystem.html) interface, which looks something like this (note this is pseudocode and not the actual interface):

```java
public interface Subsystem {
    // this method is called periodically by the scheduler
    void periodic();
}
```

Typically, subsystems will have
their own methods as well. For example,

```java
public class Drivetrain implements Subsystem {
    // these fields represent the motor hardware
    // they are declared private because they should
    // never be accessed directly outside this subsystem
    private final leftMotor = ...;
    private final rightMotor = ...;
    // this field represents the encoder hardware
    private final encoders = ...;

    public void drive(double leftSpeed, double rightSpeed) {
        leftMotor.set(leftSpeed);
        rightMotor.set(rightSpeed);
    }

    public double getDistance() {
        // do odometry calculations
        // to get the distance the robot has traveled
        return ...;
    }
}
```

## Commands

Commands encapsulate robot actions.
They can run concurrently with other commands,
meaning that multiple commands can run at the same time.
Each command takes exclusive control of zero or more subsystems,
and affects the robot by calling methods on those subsystems.

Note the requirement that commands take
*exclusive* control of subsystems.
This is to prevent
a situation where two commands
both try to use the same subsystem to
do different things at the same time.
Such a situation usually results in
unexpected and unwanted behavior.

In WPILib, commands implement the [`Command`](https://first.wpi.edu/wpilib/allwpilib/docs/release/java/edu/wpi/first/wpilibj2/command/Command.html) interface, which looks something like this:

```java
public interface Command {
    // this method is called periodically while the command is running
    void execute();

    // this method is called once when the command scheduled
    void initialize();

    // this method is called when the command ends
    // if the command was interrupted (i.e. canceled),
    // it is passed `true` as an argument, otherwise `false`
    void end(boolean interrupted);

    // this method is called periodically by the scheduler
    // to determine whether the command is finished
    boolean isFinished();

    // the scheduler calls this method to determine
    // which subsystems the command needs exclusive control of in order to run
    Set<Subsystem> getRequirements();
}
```

An example command using the `Drivetrain` subsystem described above might look like this:

```java
public class DriveOneMeter implements Command {
    private final Drivetrain drivetrain;

    public DriveOneMeter(Drivetrain drivetrain) {
        this.drivetrain = drivetrain;
    }

    public void initialize() {
        // this method is called once when the command is scheduled
        // we can use it to reset the drivetrain's encoders
        drivetrain.resetEncoders();
    }

    public void execute() {
        // this method is called periodically while the command is running
        // we can use it to accelerate the drivetrain forward
        drivetrain.drive(0.5, 0.5);
    }

    public void end(boolean interrupted) {
        // this method is called when the command ends
        // we can use it to stop the drivetrain
        drivetrain.drive(0, 0);
    }

    public boolean isFinished() {
        // if the drivetrain has traveled more than 1 meter,
        // the command is finished
        return drivetrain.getDistance() >= 1;
    }

    public Set<Subsystem> getRequirements() {
        // tell the scheduler that this command requires
        // exclusive control of the drivetrain
        return Set.of(drivetrain);
    }
}
```

## Inputs

Inputs are the way that the program
receives information from the outside world.
In imperative programming,
the typical way to do this is to
poll the input state in a periodic method,
but in command-based programming,
we specify the commands that should run
as a result of inputs *declaratively*
in a single file.

The exception to this is analog inputs,
which typically use the polling method.

Unlike the other abstractions,
inputs aren't specific to each robot
and written for us by WPILib,
so we don't have to write them ourselves.

The important declarative WPILib input types are
[`Trigger`](https://first.wpi.edu/wpilib/allwpilib/docs/release/java/edu/wpi/first/wpilibj/buttons/Trigger.html),
[`Button`](https://first.wpi.edu/wpilib/allwpilib/docs/release/java/edu/wpi/first/wpilibj/buttons/Button.html),
and a few subclasses thereof.
There are also two important types representing imperative (polled) inputs:
[`DigitalInput`](https://first.wpi.edu/wpilib/allwpilib/docs/release/java/edu/wpi/first/wpilibj/DigitalInput.html)
and [`AnalogInput`](https://first.wpi.edu/wpilib/allwpilib/docs/release/java/edu/wpi/first/wpilibj/AnalogInput.html). These are typically used directly in the periodic methods of subsystems or commands.

## The Scheduler and Concurrency

If you read the pseudocode above closely,
you may have noticed
references to "the scheduler."
If you don't understand concurrency,
it might seem rather mystical, so
I hope to demystify it a bit here.

Most likely,
all the programs you've written so far have been synchronous.
In synchronous programs,
the program runs in a single thread
so only one thing can happen at a time.
Imagine a chef
who can only do one thing at a time.
If he wants to make a dozen sandwiches,
he has to make one sandwich,
then the next, then the next, etc;
he can never work on more than one sandwich at a time.

This works fine for many programs,
but it's not ideal for robots.
We want our robot to be able to
do many things at once.
To do this,
WPILib uses a scheduler to enable concurrency.
Let's say that the scheduler
is executing two commands simultaneously: A and B.
It loop and alternates between calling
first A's `execute` method,
then B's `execute` method.
In this way,
both commands can run at the same time!
This would be like the chef
splitting up the task of making sandwiches
into parts.
First, he can lay out the bread for all the sandwiches in turn.
Then, he can add mayonnaise to all of them, and so on.

In the next chapter,
you'll learn how to
use commands and subsystems
to create a basic program
for your Romi.

## Resources

- [WPILib's Official Documentation on Command-Based Programming](https://docs.wpilib.org/en/stable/docs/software/commandbased/index.html)
- 