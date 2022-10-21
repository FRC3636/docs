# Applied Command-Based Programming

Now that you understand
what commands and subsystems are,
you're ready to
apply them to make a basic robot program.

## Materials

- A Romi robot
- A computer with WiFi
- A USB game controller or joystick(s)

## Driving the Romi

First,
create a new project 
like you did in [2.2](./creating-a-project.md)
using the `Romi - Command Robot` template for Java.

The template includes
a few files that are
important to understand:

- `RobotContainer.java` -
  This file acts kind of like a "configuration" file for the robot.
  You'll use it
  to configure button bindings,
  to set auto commands,
  and for anything else which doesn't cleanly fit into a subsystem or command.
- `subsystems/RomiDrivetrain.java` -
  This is a subsystem abstraction for the Romi's drivetrain.
  It's a good example of a simple, idiomatic subsystem,
  and you'll be making use of it in this tutorial.

If you look at `RomiDrivetrain.java`,
you'll see that it has a few member variables
which represent the Romi's motors and encoders.
Then, it has methods which can be used
to control the motors and read from the encoders.
You'll be using these methods in the commands you write.

### A Drive Command

Now, let's write a command.

Create a new file in the `commands` directory called `ArcadeDriveCommand.java`.
Then, scaffold a `Command` in it:

```java
package frc.robot.commands;

import java.util.Set;

import edu.wpi.first.wpilibj2.command.Command;
import edu.wpi.first.wpilibj2.command.Subsystem;

public class ArcadeDriveCommand implements Command {
    public ArcadeDriveCommand() {
        System.out.println("Hello, World!");
    }

    @Override
    public Set<Subsystem> getRequirements() {
        return null;
    }
}
```

If you run the robot code now,
nothing will happen.
This is because, although you've created a command,
you haven't told the robot to run it anywhere.

To do that,
you'll want to edit
the `configureButtonBindings` method in `RobotContainer.java`.
This method is called when the robot is initialized,
and is where button bindings are configured
using WPILib's input system.

Since you want the `ArcadeDriveCommand` to run most of the time,
you can set it to be the drivetrain's default command
with the following code:

```java
import frc.robot.commands.ArcadeDriveCommand;

...

public void configureButtonBindings() {
    m_robotContainer.getDrivetrain().setDefaultCommand(
        new ArcadeDriveCommand());
}
```

Now, if you run the robot code,
you should see the message "Hello, World!" printed to the console.

Now that your command is running,
you can add some code to it
to actually drive the robot.

First, you'll need to add a subsystem to the command.
This is done by creating
a member variable to hold the subsystem,
a constructor parameter to initialize it,
and adding it to the requirements returned by `getRequirements`:

```java
public class ArcadeDriveCommand implements Command {
    private final RomiDrivetrain drivetrain;

    public ArcadeDriveCommand(RomiDrivetrain drivetrain) {
        this.drivetrain = drivetrain;
    }

    @Override
    public Set<Subsystem> getRequirements() {
        return Set.of(drivetrain);
    }
}
```

Since you need a way to get the speed and rotation values from the controller,
you should add two member variables which "supply" those values.
It would also be possible to read the values directly in the command,
but this spreads out control logic across multiple classes,
which is not ideal.
Instead, the control method will be defined
in the `RobotContainer` class when the command is constructed.

`commands/ArcadeDriveCommand.java`:

```java
private final FloatSupplier speed;
private final FloatSupplier rotation;

public ArcadeDriveCommand(
    RomiDrivetrain drivetrain,
    FloatSupplier speed,
    FloatSupplier rotation
) {
    this.drivetrain = drivetrain;
    this.speed = speed;
    this.rotation = rotation;
}
```

`RobotContainer.java`:

```java
public class RobotContainer {
    private final RomiDrivetrain romiDrivetrain = new RomiDrivetrain();

    private final Joystick joystick = new Joystick(0);

    public RobotContainer() {
        configureButtonBindings();
    }

    private void configureButtonBindings() {
        romiDrivetrain.setDefaultCommand(
            new ArcadeDriveCommand(
                romiDrivetrain,
                () -> (float) joystick.getY(),
                () -> (float) joystick.getX()));
    }

    public Command getAutonomousCommand() {
        return null;
    }
}
```

_Note: if you're using a different kind of controller,
you'll need to use different class(es) and methods to read the relevant values._

Then, you can use the drivetrain's methods
inside the command's `execute` method,
which is called repeatedly while the command is running:

```java
@Override
public void execute() {
    drivetrain.arcadeDrive(speed.getAsFloat(), rotation.getAsFloat());
}
```

Now, run the robot and you should be able to drive it around!