# Drive Auto Programming Language

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

- intake (piece) (number) [avoid_obstacles || ignore_obstacles ]
- score (piece) (grid number) (node level) (column number)
- balance
- leave_community
- wait (time in seconds)
- drop

## Keywords

Keywords are used in conjunction with statements to choose where, when, and how the robot should do the action. They appear after the first word of a statement, in varying numbers and order. Keywords are seperated by spaces.

### Piece

A game piece. There are two options: `cube` and `cone`.

### Number

A positive, whole number. Example: `1`, `5`, `0`

TODO: what does this mean in the intake command? (left, mid, right)

### Obstacle avoidance

Whether the robot should consider obstacles when driving to its new location. Can be ommitted.

TODO: what is the default?

Can be `avoid_obstacles` to drive around them or `ignore_obstacles` to act as if they're not there.

## Scratch-like auto builder

You can access a Scratch version of autolang at <https://frc3636.github.io/auto-builder/>. Right-click the background and click "Export" to copy the generated code to your clipboard. This was designed to be used at outreach events so that curious visitors could try controlling the robot. The right-click-to-export process is designed to be a little bit confusing to young kids who aren't familiar with computers so that their code can be reviewed by a team member beforehand.
