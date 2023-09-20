# Drive Auto Programming Language

[Source Code](https://github.com/FRC3636/frc-2023/blob/main/src/main/java/frc/robot/utils/AutoLanguage.java) ([permalink](https://github.com/FRC3636/frc-2023/blob/94c680b3bd4396697c61f67183e001fc52389ddc/src/main/java/frc/robot/utils/AutoLanguage.java))

The 3636 Auto Language ("autolang" or "drivelang") is a custom-made, text-based way to set what a robot does during the Autonomous period. Programs can be entered into a text field on the drive station Shuffleboard dashboard. The version described in this document was designed for the 2023 FRC robotics season.

## Example program

```js
intake cube 2;
score cube 1 low cone_right;
wait 5;
intake cone 3;
score cone 1 mid cone_right;
wait 1;
balance;
```

## Commands

Instructions for the robot are called commands and are separated by semicolons (`;`). During the autonomous period, the robot completes each command, one after the other, from start to finish.

- intake (Piece) (Grid) [Pathing Mode]
- score (Piece) (Grid) (node level) (column number)
- balance
- leave_community
- wait (Time)
- drop (Drop Position)

## Keywords and values

Keywords are used in conjunction with statements to configure where, when, and how the robot should do an action. They appear after the first word of a statement, in varying numbers and order. Keywords are separated by spaces.

```js
some_command keyword1 125;
```

### Piece

A game piece. There are two options: `cube` and `cone`.

### Node Level

TODO: which values?

### Column Value

TODO: which values?

### Grid

`1`, `2`, or `3`, or `closest`. When scoring, refers to which grid (left/mid/right) to score at.

### Drop Position

Controls where the robot drops a game piece. There are [several](https://github.com/FRC3636/frc-2023/blob/94c680b3bd4396697c61f67183e001fc52389ddc/src/main/java/frc/robot/Constants.java#L312) preset positions where the robot can drop, and setting this to the index of the desired drop position will make the robot drop the game piece at that position.

### Pathing Mode

Whether the robot should consider obstacles when driving to its new location. Can be omitted. The default value is `avoid_obstacles`.

Can be `avoid_obstacles` to drive around them or `ignore_obstacles` to act as if they're not there.

### Time

An unsigned floating point number that controls how long the robot should wait, measured in seconds.

## Scratch-like auto builder

You can access a Scratch version of autolang at <https://frc3636.github.io/auto-builder/>. Right-click the background and click "Export" to copy the generated code to your clipboard. This was designed to be used at outreach events so that curious visitors could try controlling the robot. The right-click-to-export process is designed to be a little bit confusing to young kids who aren't familiar with computers so that their code can be reviewed by a team member beforehand.
