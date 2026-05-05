# **NASA HUNCH \- METSAnauts White Paper**

By: Neil Rao, Skanda Rebbapragadda, Arnav Sangle, Advay Singi, Sanay Tyagi

## **Mission Need & Requirements (PDR):** What problem are you solving? Define goals, constraints, and success criteria.

We embarked on the NASA HUNCH HERA Rover Swarm and MoonScapeproject in September of 2025\. NASA HERA as a whole is meant to create a simulated environment where participants (called HERAnauts) will live in similar conditions to the moon and Mars. 

Our overall goal was to create a swarm of four specialized rovers that could be used for the exploration of the lunar terrain surrounding the Shackleton Crater. In addition, we had to create an accurate MoonScape (and later MarsScape) in order to test the effectiveness of these rovers.

The rovers had to be constructed to: receive commands from a central control module using either IR, WiFi, or Bluetooth; to be fully modular and easy to repair by people who didn’t specifically have an engineering background; and to be fully operable by someone without a background in IT or Computer Science. Each rover would be designed for the task of support, transportation, and manipulation— at least one having an attached arm for sample collecting or payload manipulation, and at least one that had cameras with object detection capabilities. The rovers would also be operated within a 50-meter radius around the central HERA module, requiring a form of long range communication. Lastly, the rovers had to possess the capability of collection and transportation of the CubeSat.

The environment had to be constructed to match the terrain and the lighting conditions of the Lunar South Pole, later extending to simulating conditions on Martian Terrain. The rovers had to be tested on this terrain in order to determine their real-life efficacy in the real environment.

We decided that our project would be successful if we created a rover swarm had the capabilities to complete each of the criteria given to us by NASA. Our ability to create the rovers exactly up to specification was greatly limited by both the amount of time and budget that we possessed upon starting this project, so the success criteria reflected these limitations.

## **Research & Concept Development (PDR):** Summarize research, multiple concepts considered, and justify your selected design.

To create our preliminary rover design, we did research into modern rovers that NASA had created and deployed on the moon and mars. (Insert Research Here. Based on this research, we decided to use a rocker-bogie suspension system. The rocker-bogie design was first introduced with NASA’s Sojourner rover in 1997 and has since been used on Spirit, Opportunity, Curiosity, and Perseverance. The system works by linking each side of the rover through a differential, so when one wheel climbs over an obstacle, the opposite side adjusts to keep the chassis level. This prevents excessive tilting and reduces stress on the frame, which is critical for long-term exploration, such as that seen in the HERA project. In addition, we decided to use a half-tread wheel layout, where the back four wheels of the Rocker-Bogie suspension system would be linked together with a grippy tread to ensure traction on uneven and changing terrain (like the lunar surface with its fine regolith). This half tread system also makes it easier to turn and pivot the rover. 

A solar panel system for lunar and Martian rovers can use light-dependent resistors (LDRs) to detect sunlight and rotate the rover’s body for maximum energy collection. LDRs lower resistance under strong light and increase it in dim conditions, allowing sensors on the top and bottom surfaces to measure average intensity. When a threshold is crossed, motors rotate the rover so panels face the sun, and rotate again when intensity drops below effective levels. A sun-tracking system can further align panels by comparing differences in light intensity. On the Moon, panels must endure extreme temperatures from –240 °C to \+160 °C, as seen in NASA’s VIPER rover solar panels, while Mars missions face dust storms that can block sunlight, as with NASA’s InSight mission. To address these challenges, concepts like Astrobotic’s vertical solar arrays for lunar bases and flexible thin-film cells for Mars are being developed. Additionally, to mitigate dust accumulation, the system incorporates electrostatic repulsion: an electrode passes just above the panel’s surface, charging dust particles that are then repelled by a charge applied to the panel itself. This mechanism, operable with a simple motor and guide rails, eliminates the need for water or brushes and has been successfully tested by NASA on the Moon. By combining LDR-based tracking, resilient solar technologies, and dust mitigation systems, rovers can reliably harness solar energy for extended operations on both worlds.

MIT has created a system using electrostatic repulsion to cause dust particles to detach and virtually leap off the panel’s surface, without the need for water or brushes. To activate the system, a simple electrode passes just above the solar panel’s surface, imparting an electrical charge to the dust particles, which are then repelled by a charge applied to the panel itself. The system can be operated automatically using a simple electric motor and guide rails along the side of the panel. We cannot physically implement this without funding, but it was a part of our presented solution to NASA. Similar systems have been tested by NASA on the moon to success.

## **Preliminary Design (PDR):** Initial design, basic calculations, and identified risks.

With TT motors providing the main drive power and servos controlling the joints of the rocker-bogie, our rover can scale rugged terrain while keeping all wheels in contact with the ground. This gives us superior stability compared to most other suspension systems, allowing us to climb obstacles up to twice the wheel diameter and still maintain balance. By combining TT motors with servo-powered articulation, our rover gains precise control over its orientation. This means we can not only scale rocks and uneven terrain but also adjust the rover to face the sun directly, maximizing solar panel efficiency. NASA’s Perseverance rover uses this same principle to traverse the Martian surface while conducting science operations, proving the effectiveness of rocker-bogie in real-world planetary exploration. 

The rover’s electronics system is built around a Raspberry Pi (3–5) paired with the Raspberry Pi Camera Module 3 and expanded using a SparkFun Servo pHAT and a 40-pin GPIO 1-to-2 expansion board. Locomotion is powered by six TT motors driven through one L298N dual H-bridge and two DRV8871 high-current motor drivers, while articulation uses four standard servos for the rocker-bogie system plus MG90S micro-servos for the arm. Power is supplied by a 2S (7.4V) pack made from two 18650 lithium-ion cells routed through a Pololu D24V90F5 5V/9A regulator for stable Pi and peripheral power, with JST-XH connectors and 20AWG silicone wire used for distribution. The system includes an RP2040 LoRa microcontroller (Adafruit USB-C PD version) connected to the Pi via a USB-A to USB-C data cable, with an optional USB-C-to-bare-wire cable for external charging. These electronics together form the base for rover control, vision, and wireless communication across all four rovers in the HERA mesh network.

Communication follows a custom LoRa packet fragmentation protocol similar to ZigBee APS/6LoWPAN: packets are limited to 252 bytes, where byte 0 encodes from/to addresses (4 bits each), bytes 1–4 store a Packet Content ID, bytes 5–6 store the fragment index, bytes 7–8 store the total number of fragments, and bytes 9–251 carry up to 243 bytes of payload. Packets are sent every 0.1 seconds; if a rover receives a fragment not addressed to it and has not seen that packet/index before, it rebroadcasts it to extend mesh coverage. Messages are always JSON, supporting two operations: "command" (executing a shell command remotely) and "edit\_file" (writing or modifying a file). The Raspberry Pi streams JSON over serial to the RP2040 line-by-line until valid JSON is detected, then the RP2040 converts it to bytes, fragments it, transmits it, and logs packet IDs/indexes to avoid repeats. When the destination rover receives all fragments, it reconstructs them into a list indexed by fragment number, executes the command or file write, and resets shell state between tasks. A persistent “seen-packet” set is continuously backed up to onboard storage to avoid loops and ensure reliable mesh operation. 

## **Detailed Design (CDR):** Final design decisions, materials, systems, and supporting engineering analysis (CAD, drawings, calculations).

To improve upon our design from PDR we decided to utilize a half tread system with the Rocker Bogie Suspensison system, where the back wheels on either side of the suspension would be linked together using a tread. This design would both improve grip and maneuverability, helping the rover to navigate harsh terrain. In addition, our next model would be bigger and contain the electronic components on the top side for demonstration purposes. We also decided to use WAGO connectors to improve upon the modular design, making it incredibly easy to repair the rover by replacing parts.

On communication, we needed to figure out how to transmit more data than LoRa was capable of. The packet limit of LoRa devices is 243 bytes of payload to and from the command module. If we were to code object detection and mapping onto this rover, LoRa would not be effective enough to transmit a live feed. Rovers would need to be controlled in order to collect samples, requiring some level of visual feedback from the rover itself. To do this, we decided that, in addition to LoRa, we would also use a WiFi access point, or a HotSpot on the central module that the rover could connect to in order to transmit large amounts of data. Bluetooth also possesses the capability to transmit large amounts of data, but the range of Bluetooth is shorter than WiFi, making it a redundancy for WiFi. 

To meet the criteria that NASA gave to us, we would also have to add a camera to the rover design that can utilize object detection for items like the CubeSat. Since, at the time, we didn’t have access to high quality cameras, we would be using a WebCam to transmit video feed to the central command module.

To organize the coding, the video feed, and the rovers’ health, we also decided to create an app to manage the rovers. The app had a dashboard for the rovers’ health, a command pad to write and push Python code for the rover, a live video feed relaying the webcam, and an AI assist feature that was able to create code for the rover. The AI assist feature is incredible important in this project, as the HERAnauts utilizing this app might not have the qualifications or experience to write Python code to control the rover. 

While our design was effective, we were not able to demonstrate the functionality of our rover, so for CDR, we wanted to have a fully operable rover to communicate our progress and show our design ideas practically to NASA. 

## **Testing & Iteration (CDR):** Prototypes, testing methods, data collected, and design improvements.

## 

## **Final Design & Readiness (FDR):** Final solution, manufacturing/implementation plan, and safety considerations.

## 

## **Performance Evaluation:** Compare expected vs. actual performance using data.

## 

## **Successes, Failures & Root Cause Analysis:** What worked, what didn’t, and why.

## 

## **Improvements & Reflection:** Future changes and what you learned about engineering and teamwork.

