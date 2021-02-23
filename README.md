# smart-coffemachine

> Project of course: Embedded systems and IoT ([unibo](https://www.unibo.it/it))


The project consists in the creation of an embedded system that implements a coffee machine. 

# Components:
* 3 green leds (L1, L2, L3) 
* 1 (T1) tactile button and 1 potentiometer (Pot)
* 1 (PIR) motion sensor and one (SDist) sonar Serial link with PC

# States:
* **_STANDY_BY_**: initial state of the machine where it is assumed to have a reduced electricity consumption.
* **_ON_**: it switches to on mode when the presence of someone nearby is detected. 
            If no more presence is detected for 5 seconds, the machine returns to STAND BY mode.
* **_READY_**: one passes into this is when a presence is detected at a distance of less than 0.3m for 1s. On the PC side, the message "welcome!" Is displayed. If the machine no longer detects anyone for 5 seconds, it returns to the ON state.

# Arduino 
The part developed in Arduino deals with the management of the components that make up the subsystem
![*System diagram*](https://github.com/coklefy/smart-coffemachine/blob/main/doc/FSM%20Diagram/arduino.jpg?raw=true)


# Software architecture
The system was modeled through a task approach, with a behavior model based on finite state machines.
* *SharedSpace*: The tasks do not communicate directly with each other, but interact through variables, thus remaining synchronized and being able to exchange information. These variables are located in a buffer called SharedSpace. It is a space where variables are stored and tasks access this space to obtain information. It also offers an interface that allows tasks to interact. Each task is passed this buffer as a parameter.
* *MotionDetectorTask*: The task takes care of checking if a presence has been detected in the vicinity. In sharedSpace this task changes the value of the motion variable. This way other tasks can read it and interact in their own way.
* *ControlStateTask*: This task defines the behavior that the machine must assume at each of its states. In this state we manage the changes in the state of the machine.
* *CommandsTask*: This task manages the situation in which the machine is found when it is in the READY state and after the state of accepting commands from the user.
* *TurnOnLedTask*: The task is responsible for coordinating the lighting of the LEDs in sequence. That is, the ignition of the first, then the second and finally the third. The lighting of the 3 LEDs simulates the progress of coffee preparation.
* 
