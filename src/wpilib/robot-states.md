# Robot States

Robots programmed with WPILib can be in one of four modes of operation, called states:

- **Disabled**:
  The robot does not accept user input and shouldn't move mechanisms.
  It is safe for humans to be around the robot while it is disabled.
  This is the default state.
- **Teleoperated**:
  The robot accepts user input and moves mechanisms.
  This is the most common state used while testing,
  and the teleoperated period of the FRC game is spent in this state.
- **Autonomous**:
  The robot does not accept user input, and instead goes through a sequence of preprogrammed actions.
  This state is active for the first 15 seconds in an FRC match.
- **Emergency Stopped (E-Stopped)**:
  This is a special state which is identical to the disabled state,
  except that the bot cannot be re-enabled after it has been emergency stopped.
  This state is triggered by pressing the spacebar in FRC Driver Station.

Here's a table comparing each state:

| State          | Moves | Accepts Input | Can Be Enabled |
| -------------- | ----- | ------------- | -------------- |
| Disabled       | No    | No            | Yes            |
| Teleoperated   | Yes   | Yes           | N/A            |
| Autonomous     | Yes   | No            | N/A            |
| Emergency Stop | No    | No            | No             |

Excepting emergency stops,
the code will follow this flowchart during a match:

[![](https://mermaid.ink/img/pako:eNqFkT-PwjAMxb9KZIkF0eXGDEgnwcjEbYTB1L4SXZNUSYqEEN_93Iby524gk_P8s_zkd4E6EIOGlDHzymIT0VWnD-OVvN18r6pqqVY24aFlKur0G1uffQ4-uNAnrTaY66M6cGN9KuijO8Jf3HLoOMoq0s9N0WwgxZ5ug8_ki4VpS0ELPJu9elpvc-i6gV07jg37-qwG6U7_sfWW_-fm7cQEjLSccbAKCxDYoSW592VADeQjOzagpSSMPwaMvwqHYnB79jXoHHteQN_RIx7Q39gmUZlsDnFTAhxzvP4C5lKYIw?type=png)](https://mermaid.live/edit#pako:eNqFkT-PwjAMxb9KZIkF0eXGDEgnwcjEbYTB1L4SXZNUSYqEEN_93Iby524gk_P8s_zkd4E6EIOGlDHzymIT0VWnD-OVvN18r6pqqVY24aFlKur0G1uffQ4-uNAnrTaY66M6cGN9KuijO8Jf3HLoOMoq0s9N0WwgxZ5ug8_ki4VpS0ELPJu9elpvc-i6gV07jg37-qwG6U7_sfWW_-fm7cQEjLSccbAKCxDYoSW592VADeQjOzagpSSMPwaMvwqHYnB79jXoHHteQN_RIx7Q39gmUZlsDnFTAhxzvP4C5lKYIw)

The complete flowchart with emergency stops looks like this:

[![](https://mermaid.ink/img/pako:eNp9kTELwjAQhf9KuFHs4phBEHR00s04nL2zBpukJKkg4n_32lpUFDMl733hHvduUAZi0JAyZl5arCK64jIzXsnZTfaqKOZqaRMeaqZBHV-9tWhz8MGFNmm1xlye1IEr69OAvtwe3nLNoeEoo0i_m6LZQIo9PT--kx8RxikD-iPQapND03TgynGs2JdXlUT6Geg__BXiPz66PSqr6-LBFIR0aEl2fOs4A_nEjg1ouRLGswHj78KhRNtcfQk6x5an0Db0qgT0EeskKpPNIa6H0vru7g_Lh5bb?type=png)](https://mermaid.live/edit#pako:eNp9kTELwjAQhf9KuFHs4phBEHR00s04nL2zBpukJKkg4n_32lpUFDMl733hHvduUAZi0JAyZl5arCK64jIzXsnZTfaqKOZqaRMeaqZBHV-9tWhz8MGFNmm1xlye1IEr69OAvtwe3nLNoeEoo0i_m6LZQIo9PT--kx8RxikD-iPQapND03TgynGs2JdXlUT6Geg__BXiPz66PSqr6-LBFIR0aEl2fOs4A_nEjg1ouRLGswHj78KhRNtcfQk6x5an0Db0qgT0EeskKpPNIa6H0vru7g_Lh5bb)
