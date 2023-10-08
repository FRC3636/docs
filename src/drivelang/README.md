# Drive Auto Programming Language

[Source Code](https://github.com/FRC3636/frc-2023/blob/main/src/main/java/frc/robot/utils/AutoLanguage.java) ([permalink](https://github.com/FRC3636/frc-2023/blob/94c680b3bd4396697c61f67183e001fc52389ddc/src/main/java/frc/robot/utils/AutoLanguage.java))

The 3636 Auto Language ("autolang" or "drivelang") is a custom-made, text-based way to set what a robot does during the Autonomous period. Programs can be entered into a text field on the drive station Shuffleboard dashboard. The version described in this document was designed for the 2023 FRC robotics season.

## Example program

```js
intake cone 2;
score cone 1 low cone_right;
wait 5;
intake cone 3;
score cone 1 mid cone_right;
wait 1;
balance;
```

## Commands

Instructions for the robot are called commands and are separated by semicolons (`;`). During the autonomous period, the robot completes each command, one after the other, from start to finish.

- intake (Piece) (Pre-placed Game Piece) [Pathing Mode]
- score (Piece) (Grid) (Node Level) (Node Column)
- balance
- leave_community
- wait (Time)
- drop (Drop Position)

### Intake

This command makes the robot pick up the specified pre-placed game piece. Examples:

```js
intake cone 1 ignore_obstacles;
intake cube 2;
```

### Score

This command makes the robot score the currently loaded game piece at a certain grid, node level, and column. Examples:

```js
score cube 1 low cube_left;
score cone closest mid cone;
```

### Balance

Makes the robot balance on the charge station.

### Leave Community

Makes the robot leave the community to earn a small amount of points.

### Wait

Makes the robot wait a certain number of seconds. Example:

```js
wait 5.1;
```

### Drop

Makes the robot drop its game piece in a preset position, indicated by the number following the command. The lowest valid number is `1`. Example:

```js
drop 1;
...
drop 2;
```

### Shoot (WIP)

Currently a work-in-progress but this will tell the the robot to shoot a cube (if held) on the open side of the grid or in place if specified. 

Shoot in place:

```js
shoot true;
```

Shoot on open side:

```js
shoot;
```


## Keywords and values

Keywords are used in conjunction with statements to configure where, when, and how the robot should do an action. They appear after the first word of a statement, in varying numbers and order. Keywords are separated by spaces.

```js
some_command keyword1 125;
```

### Piece

A game piece. There are two options: `cube` and `cone`.

### Node Level

The height at which to score a game piece. There are three options: `low`, `mid`, and `high`.

### Node Column

The horizontal position to score at. There are three options: `cone_left`, `cube`, and `cone_right`. When not using the `low` node level, these columns should only be used with the game piece in their name.

### Grid

`1`, `2`, or `3`, or `closest`. When scoring, refers to which grid to score at.

### Pre-placed Game Piece

Configures the robot to pick up a certain pre-placed game piece. When the robot on the blue team side, this is counted from left to right. When the robot is on the red team side, this is counted from right to left. The options are `1`, `2`, `3`, or `4`. `closest` is another option but is unlikely to work.

### Drop Position

A number that controls where the robot drops a game piece. There are [several](https://github.com/FRC3636/frc-2023/blob/94c680b3bd4396697c61f67183e001fc52389ddc/src/main/java/frc/robot/Constants.java#L312) preset positions where the robot can drop, and setting this to the index of the desired drop position will make the robot drop the game piece at that position.

### Pathing Mode

Whether the robot should consider obstacles when driving to its new location. Can be omitted. The default value is `avoid_obstacles`.

Can be `avoid_obstacles` to drive around them or `ignore_obstacles` to act as if they're not there.

### Time

A positive decimal point number that controls how long the robot should wait, measured in seconds.

## Scratch-like auto builder

You can access a Scratch version of autolang at <https://frc3636.github.io/auto-builder/>. Right-click the background and click "Export" to copy the generated code to your clipboard. This was designed to be used at outreach events so that curious visitors could try controlling the robot. The right-click-to-export process is designed to be a little bit confusing to young kids who aren't familiar with computers so that their code can be reviewed by a team member beforehand.
