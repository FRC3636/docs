# Simulation

The WPILib simulator lets you control a (fake) robot without expensive real hardware. It's built into the project and runs the robot's code directly on your computer (rather than on the robot's RoboRIO computer).

## Starting the simulator

After the robot's code is downloaded, a simulator can be opened by pressing Ctrl-Shift-P and selecting the "Simulate robot code" option. The robot's state can be controlled in the upper right hand corner. Disabled means it will not accept input, autonomous means it will run a pre-programmed path, and teleoperated means it can be controlled by a driver.

## Setting up controls

Enable keyboard control by dragging "Keyboard 0" onto the "Joystick 0" slot and "Keyboard 1" to "Joystick 1".

### Joysticks

The robot is controlled using joystick input. In a real competition, these would be physical joysticks, but the simulator lets you use a keyboard to make it easier to test.

- **Joystick 0** controls the robot's left/right/up/down movement.
- **Joystick 1** controls the robot's rotation.

### Keyboards

Keyboard controls in the simulator are fake joysticks that correspond to certain key combinations.

- **Keyboard 0** means you're using WASD
- **Keyboard 1** means you're using J and L

## Seeing a 2D field

When using the robot in real life, it keeps track of where it thinks it is based on how far the wheels move and what it sees using its camera. In the simulator, this can be used to see how the robot thinks it will move after you input certain controls.

The NetworkTables menu in the simulator GUI can be used to view information directly connected to the robot's behavior - like the current controller sensitivity, or where the robot thinks it is on the field. Open `NetworkTables > Shuffleboard > Auto > Field`.

## Controlling the robot (and how it responds to controls)

## Setting up a 3D field (and common caveats)

## Changing code to affect the simulator
