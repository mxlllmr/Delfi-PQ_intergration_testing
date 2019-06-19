# Delfi-PQ - Intergration testing

## Purpose
(Maybe including that subsystems for small satellits do not consist of any additional units other then the ones absolute needed, cannot monitor the execution of the task, only send command and receive replyies, but no guarantee it actually worked.)

Delfi-PQ belongs to the new type of miniaturized satellites, namely PocketQube satellites, which exhibit certain advantages over the widely used CubeSats. Due to their smaller size, PocketQubes bring a decrease in total cost and development time. The main objective of Delfi-PQ is to test the application of a PocketQube satellite for future missions, since it is expected that the reduced development cycle will allow a comparably higher launch frequency, increasing flight experience and reliability of the system.

To ensure that the respective subsystems are operational, the intergration of the Delfi-PQ system must be tested. While in space, the functionality of a subsystem can not be observed directly and positive feedback from the subsystem itself does not guarantee a faultless execution of the specified task. Additionally, the subsystem hardware itself is not able to detect if the execution was in fact successful. Hence, it is necessary to deploy verification through an external hardware if the command given to the subsystem was effectively executed.

This project will focus on the design and implementation of a solution for verifying the successful execution of a command given to the subsystem. In particular, the verification of the LED command to the Launchpad (Texas Instruments) board is demonstarted via external hardware (Arduino Mega). This software is intended to be modified easily in order to adapt the intergration testing to different subsystems of the Delfi-PQ.


- receiving feedback from external hardware if the command given to the board was successfully executed
- verification
- when in space no-one can check if the led is actually on
- hard ware itself does not detect if the led is on itself
tests the intergration of the Delfi-PQ system

## Repository overview
Besides this README file, the repository includes following files:
- **arduino_feedback.ino**:
- **client_LED.py**:
- **client_ADB.py**:
- **client_ADB_noUI.py**:
- **pq_comms.py**:


## Design

general Idea ......

### Arduino 

#### Arduino block diagram

### Python

#### Python block diagrams


<img src="images/setup_LED.jpg" width="500">

so i was thinking of having something that says each 5/10 seconds: The LED is ON (like, if serial reads 1, print this)

if the user sets it to off and it's on, it says: Error, output is not what was chosen. Log file saved; then it saves a file with the time, with what was chosen and what it read


tell user what to do
exit programm press control C
use there commands: led on of

get user input
thread 1
use user input
turn on -> turn led on
turn off -> turn led off


thread 2
check arduino serial,
compare to user input


if error
log file will be saved, saves error
and saves comment



## How to use

### Requiered hardware

In order to use the scripts provided in this repository, the user will need following hardware items: 
- **SimpleLink™ MSP432P401R LaunchPad**: Communicates with the python scripts and is simulating the Delfi-PQ subsystem
- **Arduino (UNO, MEGA, ect.)**: Used as the external hardware 
- **Wires and Breadboard**: To connect the LaunchPad and Arduino

### Software implementation

#### How to use client_LED.py

Libraries needed: ```numpy```, (...) - in the 'import' section)

Commands: 

- **0,1,2,...+ENTER**: Chooses Arduino port
- **0/1+ENTER**: Turns OFF/ON the LED
- **CTRL+c followed by ENTER**: Exits the program

First, run the EGSE software. This java software should be in a local folder, and it runs by writing:
```
java -........ 
```
in the terminal. After making the connection and emulation of the LaunchPad board is established (this can be verified by opening the browser in ```localhost:8080```, run the python script as:
```
python client_LED.py localhost:8080
```

A message of how to utilise the program will appear:

```
Welcome to...
(...)
Choose Arduino port (0,1,2,...): 
```

Choose the port that connects the Arduino to the CPU. 
NOTE: The Arduino board has to be connected in the format of ```/dev/ttyACM(...)```, else the program does not recognise it. In case the connection to the CPU has a different designation, change line (XXXXX) of ```client_LED.py``` to the specific designation (for example, ```COM(...)```)

If the connection is established, the program will attempt to connect to the LaunchPad board. This is represented by a progress bar that looks like the following:
```
[###       ]
```
Each ```#``` represents a successful ping with the DEBUG subsystem of the board. 

Finally, the program is ready to take commands to turn ON and OFF the LED. Every 10 seconds (after the first user input) an update on the Arduino feedback is printed on the screen. Every time a user inputs 1/0 (+ENTER), the message that is sent to the board is printed. If there is feedback from the board, the following should appear:
```
Command received from DEBUG
```
Additionaly, an immediate Arduino feedback check is performed.

Anytime the Arduino disagrees with the subsystem feedback, an ERROR message is saved in an external .log file.

FINAL REMARKS: This program is binded such that if any undesired input exists, a 'try again' type of notification is printed; The ```ENTER``` command after ```CTRL+c``` is required because there is an existing thread that contains the function ```input()``` that stalls the program. The waiting period is demanded such that existing ```time.sleep()``` functions cease.

#### How to use client_ADB.py

#### How to use client_ADB_noUI.py

NOT TO FORGET: in the readme, we need to specify that the arduino has to be connected in the format of "/dev/ttyACM(...)", else the program does not recognise it. In case this is different, one must change that line to the specific designation (like "COM(...)")

java file running - as local host

in stall py.serial library  in order to allows communication between python and arduino

run python script 

commend  to open and be in right directory 
libraries

(does this belong into design? no, right?)

When xxx.py is running, the user will be provided with the commands available and a short description thereof:
```
command 1: ---------
command 2: ---------
command 3: --------
Press Ctrl + C: Exit program
```
Additionally, the user will provide the subsystem with a command to execute a task, here either 'Turn LED on' or 'Turn LED off'.
...


## Results

## Issues encountered

- the connection between LaunchPad and EGSE is not established: remedy: try until it is

- The launchpad LED P1.0 connection burned and the LED was constantly ON when plugged to that GPIO. This was remedied by manually connecting/disconnecting the cable accordingly (or not) to the user input


## Future changes and recommendations
instead of connecting to the led can be used to verify the funcionality of another Delfi-PQ subsystem

Using a better version of the EGSE software/Launchpad that are not as flawed and consume time to deal with

