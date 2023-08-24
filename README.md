# UNITY

## A Line Follower and Obstacle Avoider Robot

Welcome to the repository for my undergraduate project—an autonomous robot capable of line following and obstacle avoidance, designed using the PIC18F4550 microcontroller.

## Overview

This project was conceived during my undergraduate studies as a demonstration of the fusion between hardware and software in robotics. The goal was to create a versatile robot that could autonomously follow a black line while also avoiding obstacles in its path.

## Features

- **Line Following:** The robot is equipped with infrared (IR) sensors that detect the differential light absorption properties of a black line on a contrasting surface. This allows the robot to accurately track and follow the line.

- **Obstacle Avoidance:** Beyond line following, the robot integrates additional hardware components to enhance its sensory perception. When obstacles or edges are detected, the robot's PIC18F4550 microcontroller triggers responsive maneuvers to navigate around them.

- **PIC18F4550 Microcontroller:** The heart of the robot's intelligence is the PIC18F4550 microcontroller. It processes sensor inputs, executes algorithms, and coordinates motor control to ensure smooth line following and effective obstacle avoidance.

## Abstract

  This abstract encapsulates a significant milestone achieved during my undergraduate studies—a project that underscores innovation and technical prowess. The primary objective was to conceptualize and construct a dual-functionality robotic system employing a PIC18F4550 microcontroller. The project seamlessly integrates two distinct functionalities: line following and obstacle avoidance.

  At its core, the project centers on designing an autonomous robot that traces a designated black line through the application of infrared (IR) and TSOP sensors. The IR sensors are strategically positioned to detect the differential light absorption properties of the black surface, enabling precise path tracking. When the robot veers off the intended trajectory, the microcontroller orchestrates a realignment, ensuring the robot resumes its course without human intervention.

  Additionally, the project extends beyond basic line following by imbuing the robot with obstacle avoidance capabilities. This is achieved by integrating supplementary hardware components that enhance the robot's sensory perception. As the robot approaches obstacles or edges, these components trigger responsive maneuvers, enabling the robot to skillfully navigate its environment while circumventing potential collisions.

  Central to this accomplishment is the PIC18F4550 microcontroller—a technological cornerstone that orchestrates the intricate dance between hardware and software. Its versatile architecture empowers the robot with the intelligence to make informed decisions based on sensory inputs, thereby engendering autonomous motion and obstacle evasion.

  In conclusion, this project exemplifies the fusion of theoretical knowledge and practical implementation. It embodies the essence of autonomous robotics, where a PIC18F4550 microcontroller harmonizes with meticulously designed hardware and software components to create a dual-functional marvel capable of line following and obstacle avoidance.


## Repository Contents

- `src/`: Contains the source code written in assembly language for programming the PIC18F4550 microcontroller.
- `hardware/`: Includes any hardware-related information, such as circuit diagrams and component lists.
- `media/`: Houses images and videos showcasing the robot's operation.
- `docs/`: Documentation detailing project specifications, instructions, and lessons learned.

## Usage

1. Review the source code in the `src/` directory to understand the logic and algorithms governing the robot's behavior.
2. Explore the hardware information in the `hardware/` directory to replicate or modify the robot's physical setup.
3. Refer to the documentation in the `docs/` directory for comprehensive project details, including lessons learned and insights gained.

## Acknowledgments

I extend my gratitude to my mentors and educators who guided me through this project, imparting invaluable knowledge and guidance.

## Contributing

Contributions to enhance and expand this project are welcome! Feel free to fork the repository, make improvements, and submit pull requests.

## License

This project is licensed under the [MIT License](LICENSE).

---

Feel free to explore the project further and dive into the code and resources. If you have any questions or suggestions, don't hesitate to open an issue or reach out through the provided contact information.
