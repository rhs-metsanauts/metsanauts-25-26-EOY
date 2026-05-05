# NASA HUNCH Engineering Design Report
## METSAnauts — HERA Rover Swarm & BothScape Terrain Simulation

**Team:** Neil Rao, Arun Skanda Rebbapragada, Arnav Sangle, Advay Singi, Sanay Tyagi
**Mentor:** David Berry
**School:** Ranchview High School | Carrollton, TX
**Program:** NASA HUNCH — HERA Rover Swarm & BothScape Simulation
**Academic Year:** 2025–2026

**Project Website:** [metsanauts.com](https://www.metsanauts.com) | **Gallery:** [metsanauts.com/gallery](https://www.metsanauts.com/gallery) | **Codebase:** [github.com/skandacode/RoverSystemV2](https://github.com/skandacode/RoverSystemV2)

---

## 1. Mission Need & Requirements

### Background

In September 2025, the METSAnauts team began work on the NASA HUNCH HERA Rover Swarm and BothScape project. NASA's Human Exploration Research Analog (HERA) program simulates 45-day isolation missions replicating lunar and Martian surface conditions for Artemis mission preparation. Participants — called HERAnauts — include teachers, geologists, and specialists but are generally not engineers or computer scientists. Our project was designed to give these HERAnauts a practical tool for conducting robotic surface exploration without requiring technical expertise.

### Problem Statement

The core engineering challenge was to design and prototype a remotely operated robotic exploration swarm alongside a portable terrain testbed that simulates realistic lunar and Martian surface operations. The rover swarm needed to support scouting, sample collection, payload transport, and terrain mapping within a 50-meter operating radius of the central HERA module — all controlled by operators who may have no robotics or software background.

### Formal Requirements

NASA HUNCH provided a set of minimum specifications that the project had to satisfy:

| Requirement | Description |
|-------------|-------------|
| Robot count | Minimum 3 specialized robots, each ≤ 12 in³ |
| Operation mode | Camera and sensor only — no direct line-of-sight |
| Transportation Robot | 0.5–1.0 lb payload; carry lunar regolith; stable base |
| Manipulation Robot | Lifting mechanism; precision placement; regolith pickup |
| Support Robot(s) | Tow/recovery, comms relay, mapping, tool delivery |
| Control | Bluetooth, WiFi, or IR command; camera-only operation; data logging |
| Missions | All 10 minimum missions demonstrated (M1–M10) |
| AI/ML | AI/ML integration or NVIDIA Jetson Nano (bonus) |
| Communications | 50-meter operating radius |

### Success Criteria

Given the constraints of a nine-month academic calendar and a starting budget of $500, the team defined success as: building a rover swarm capable of completing all NASA-specified missions; implementing a fully modular, field-repairable design requiring no specialized engineering background; and delivering an interface operable by non-technical HERAnauts. A terrain testbed modeling the lunar south pole and Martian surface would be constructed and used to validate rover performance.

---

## 2. Research & Concept Development

### Suspension System Research

Before selecting a mechanical design, the team conducted extensive research into existing planetary rovers developed by NASA. The primary question was which suspension system would provide the best balance of obstacle clearance, terrain adaptability, and design complexity for a student team to fabricate.

The rocker-bogie suspension system emerged as the clear choice. First deployed on NASA's Sojourner rover in 1997 and proven on Spirit, Opportunity, Curiosity, and Perseverance, this system has a decades-long heritage of reliable surface operation. The mechanism works by linking each side of the rover through a passive differential: when one wheel climbs an obstacle, the opposite side adjusts automatically to keep the chassis level. This passive leveling prevents excessive tilting and distributes stress across the frame without requiring any active control input. The system can clear obstacles up to twice the wheel diameter and, as demonstrated by Perseverance, tolerates up to 45 degrees of tilt in any direction without tipping. No active computer intervention is needed to maintain balance — a critical advantage for reliability in a high-stakes environment.

For wheel configuration, two competing designs were evaluated: fully independent wheels and a half-tread hybrid. Full independence offers maximum articulation but provides inconsistent grip on granular regolith, as each wheel behaves as a point contact. A continuous tread (tank-style) offers strong grip but sacrifices turning radius and adds mechanical complexity. The team selected a half-tread hybrid: the rear four wheels on each side of the rocker-bogie are linked by continuous rubber tread, while the front two wheels remain independently driven. This configuration provides the traction benefits of a track drive on the rear, while the independent front wheels allow the rover to steer and pivot effectively. This exactly mirrors the kind of multi-terrain optimization that NASA engineers apply when designing for surfaces like the lunar south pole, where regolith is fine-grained, loose, and prone to causing wheel slip or sinkage.

### Power & Energy Research

Designing a power system for a lunar or Martian surface rover requires understanding the extreme thermal and environmental conditions of each world. On the Moon, surface temperatures range from −240 °C in permanently shadowed craters to +160 °C in direct sunlight, as documented in NASA VIPER mission data. Mars presents a different challenge: dust storms can attenuate solar irradiance by 99% for weeks at a time, as seen during the NASA InSight mission.

For the Moon, the team researched light-dependent resistor (LDR) based sun tracking, in which two LDR sensors compare differential light intensity across the rover's surface. When a threshold difference is detected, the system drives the rover's rocker-bogie suspension to physically tilt the chassis toward the sun, maximizing solar panel exposure. This is viable on the lunar south pole because the sun sits near the horizon at a consistent low angle, making body-tilt tracking effective where traditional rotating panel mounts would be mechanically complex.

For dust mitigation, the team researched an electrostatic repulsion system developed at MIT. In this approach, an electrode passes just above the solar panel surface, imparting an electrical charge to dust particles. The panel surface is then charged to the same polarity, causing the dust to repel and clear without the use of water, brushes, or moving contact components. This system has been tested by NASA on the Moon and operates using only a simple electric motor and guide rails. Although funding limitations prevented physical implementation, this research was included in the team's proposed solution to NASA, demonstrating awareness of the full operational challenge.

### Communications Research

Early in the design process, the team evaluated three candidate communication technologies: Bluetooth, WiFi, and LoRa (Long Range radio). Bluetooth offered sufficient bandwidth for video but a range of only ~20 feet — inadequate for the 50-meter NASA requirement. Standard WiFi extended range to roughly 20–60 feet depending on environment but was also limited for the full operating radius. LoRa offered a range of up to 1 kilometer and low power consumption but a maximum payload of 243 bytes per packet — far too small for video transmission.

The team's solution was a layered communication architecture: LoRa as the primary long-range command channel, WiFi as the primary high-bandwidth channel for video and mapping data within the base area, and Bluetooth as a close-range fallback. This multi-layer approach directly mirrors how NASA designs redundant communication systems for planetary missions, where no single link can be a single point of failure.

---

## 3. Preliminary Design

### Mechanical System

The preliminary rover design centered on the rocker-bogie suspension system implemented using four standard servos controlling the joint articulation, with six TT motors providing independent wheel drive. The half-tread configuration was adopted for the rear four wheels, with the front two wheels remaining independently motored. Initial concept models were built from popsicle sticks and 3D-printed mini-wheels to validate the rocker-bogie geometry before committing to full fabrication.

The chassis was sized to accommodate the electronics stack and maintain a low center of gravity for stability. Wheel material was 3D-printed using Creality HyperPLA for its combination of light weight, ease of fabrication, and replaceability — a key consideration given the NASA requirement for field-repairable design.

### Electronics Stack (v1)

The preliminary electronics system was built around a Raspberry Pi 5 as the primary onboard computer, expanded with a SparkFun Servo pHAT for GPIO management. Locomotion was powered by six TT motors driven through one L298N dual H-bridge and two DRV8871 high-current motor drivers. Four standard servos provided rocker-bogie articulation. Power was supplied by a 2S (7.4V) 18650 lithium-ion pack fed through a 5V regulator for the Pi and peripherals. WAGO connectors with ferrules were specified for all power distribution, enabling tool-free field replacement of any electrical component.

Communication was handled by an RP2040 LoRa microcontroller (Adafruit USB-C PD version) connected to the Raspberry Pi via USB. The RP2040 managed the LoRa protocol entirely independently from the Pi, acting as a dedicated communications coprocessor that could be hot-swapped without disassembling the rover.

### LoRa Protocol Design

The team designed a custom packet fragmentation protocol inspired by ZigBee APS and 6LoWPAN to work within LoRa's 252-byte packet limit. Each packet was structured as follows:

| Bytes | Field | Description |
|-------|-------|-------------|
| 0 | Addresses | Source/destination (4 bits each) |
| 1–4 | Packet Content ID | Unique message identifier |
| 5–6 | Fragment Index | Position in multi-packet message |
| 7–8 | Total Fragments | Total expected packets |
| 9–251 | Payload | Up to 243 bytes of data |

Packets were transmitted every 0.1 seconds. Any rover that received a fragment not addressed to it would rebroadcast it if it had not seen that Packet ID and Fragment Index before — implementing a mesh relay that extended effective range without dedicated relay infrastructure. All messages were encoded as JSON, supporting two operations: `command` (remote shell execution) and `edit_file` (remote file modification). A persistent seen-packet set, continuously backed to onboard storage, prevented relay loops.

### Initial Swarm Concept

The preliminary design specified a four-rover swarm: one transportation rover, one manipulation rover, and two support rovers. Each rover was budgeted at approximately $750, producing a total estimated cost of $3,000. Given the team's available budget of $500, this configuration was immediately identified as a financial risk requiring mitigation before CDR.

---

## 4. Detailed Design

### CDR Design Changes

Between PDR and CDR, the team implemented several significant design improvements based on NASA reviewer feedback and internal testing. NASA judges at PDR praised the rocker-bogie design but recommended increasing the terrain scale, adding Martian terrain, and exploring solar panel and modularity improvements. All three recommendations were incorporated into the CDR design.

The most consequential design decision made before CDR was consolidating the four-rover swarm into a single, multi-role rover. With a budget of $500 and a team of five high school students working across a nine-month academic year, building and programming four coordinated rovers was not feasible. Rather than deliver four incomplete rovers, the team chose to deliver one fully functional, fully documented rover that demonstrated all required capabilities.

The v2 chassis was enlarged relative to the v1 to accommodate the full electronics stack and improve stability. All electronics were mounted on top of the chassis to facilitate demonstration and rapid access during field testing. Half-tread rear wheels were implemented, linking the middle and rear wheel pairs on each side with continuous rubber tread.

### WAGO Modular Connector System

A critical CDR design improvement was the systematic adoption of WAGO lever-nut connectors throughout the wiring harness. Unlike soldered connections or screw terminals, WAGO connectors allow any wire to be disconnected and reconnected by hand, in under five seconds, with no tools. This directly satisfied the NASA requirement that the rover be field-repairable by people without engineering backgrounds. Any module — motor driver, servo, power supply — can be swapped in the field by following a color-coded wiring diagram.

### Communication Upgrade: WiFi Addition

Because LoRa's 243-byte payload cannot carry video data, a WiFi access point was added to the central control module to serve as the primary high-bandwidth link for live video and mapping telemetry. The rover connects to this hotspot automatically, enabling real-time video streaming that would otherwise be impossible. LoRa was retained as the long-range fallback for command transmission beyond WiFi range. This dual-layer design ensures the rover remains operable at any distance within the 50-meter operating radius.

### Control Application

To satisfy the NASA requirement for non-technical operability, a web-based control application was developed. The CDR version included a rover health dashboard, a command pad for submitting Python code, a live video feed from the onboard webcam, and an AI-assisted code generation feature. The AI feature was driven by the recognition that HERAnauts — who include teachers, geologists, and mission specialists — would not typically be able to write Python control code. By allowing natural language input, the application put full rover control within reach of any crew member regardless of technical background.

---

## 5. Testing & Iteration

### PDR Terrain Testing

The first terrain prototype was a 2 ft × 2 ft foam board model surfaced with diatomaceous earth as a lunar regolith simulant. The diatomaceous earth was selected for its fine, granular texture, which approximates the particle size distribution of lunar regolith better than common household materials. NASA reviewers at PDR provided positive feedback on the regolith choice and the crater topology modeling but recommended scaling up to 8 ft × 8 ft and adding a Martian terrain section.

### Rover v1 Iteration

The v1 rover progressed from a concept model (popsicle sticks and 3D-printed mini-wheels) to a full rocker-bogie prototype with TT motors, L298N and DRV8871 motor drivers, and Raspberry Pi control. The first successful LoRa transmission between two devices was achieved during this phase, confirming that the custom fragmentation protocol was functional and that the RP2040 coprocessor could communicate reliably with the Raspberry Pi over serial.

Key findings from v1 testing included confirmation that the rocker-bogie geometry maintained chassis stability over simulated regolith obstacles, and that the TT motors provided adequate torque for the rover's weight class. The plastic TT motor gears, however, were identified as a potential durability concern under sustained load.

### CDR Terrain Scaling

The 8 ft × 8 ft terrain design was drawn in Onshape by Neil Rao and a $1,000 parts order was submitted. After budget review, the terrain was scaled down to 4 ft × 4 ft for CDR presentation in order to manage costs while still demonstrating the dual-terrain BothScape concept with both a Moon region (modeled on the Shackleton Crater south pole) and a Mars region (modeled on ancient volcanic plains with fine, dusty regolith). The modular 4 ft × 4 ft section design also improved transport and reconfigurability, which was identified as a practical advantage for field deployment.

### CDR Rover Testing & Field Demonstration

The v2 rover — featuring the half-tread rear wheels, WAGO connectors, webcam, and full electronics stack — was field-tested outdoors on the Ranchview High School campus and photographed to document operational status. Remote laptop control via WiFi was confirmed. The gallery at [metsanauts.com/gallery](https://www.metsanauts.com/gallery) documents 12 photos from field testing, including team members operating the rover via laptop, close-up views of the electronics and wiring, and the rover navigating the terrain in operational conditions.

The CDR presentation demonstrated a working proof-of-concept rover. The identified gap going into FDR was that the rover needed to reach fully mission-capable status with terrain mapping, sample pickup, and full multi-rover swarm coordination.

---

## 6. Final Design & Readiness

### Architecture Shift: Return to 3-Rover Swarm

With CDR complete and a functional base rover established, the FDR design returned to the multi-rover swarm concept — now with three specialized rovers rather than four. This was made possible by the experience and efficiency gained through building and iterating on the v1 and v2 rovers.

| Rover | Role | Key Systems |
|-------|------|-------------|
| Claw Rover | Sample collection, manipulation, CubeSat retrieval | Robotic arm, claw-mounted camera, WiFi/LoRa |
| Recon Rover | Terrain mapping, sensor coverage, comms relay | ZED 2i stereo camera, 3D spatial mapping |
| Support Rover | Logistics, redundancy, swarm coordination | ZED 2i stereo camera, LoRa relay, recovery assist |

### Computing Upgrade: NVIDIA Jetson Nano

The Raspberry Pi 5 was replaced with an NVIDIA Jetson Nano as the central onboard computer for the FDR rovers. The Jetson Nano provides a GPU-accelerated processing environment capable of running the ZED SDK for real-time stereo vision, onboard AI inference, and simultaneous operation of multiple compute-intensive services. An NVMe SSD was added for high-speed local storage of mapping data and video.

### Spatial Mapping: ZED 2i Stereo Camera

ZED 2i stereo cameras were mounted on the Recon and Support rovers. The ZED 2i provides real-time 3D spatial mapping, object detection, and high-resolution video streaming through the Stereolabs ZED SDK. With two cameras deployed across the swarm, the overlapping fields of view produce complete area coverage of the terrain surrounding the central module. This enables reliable CubeSat detection and provides HERAnauts with a live 3D map of the surrounding environment — a capability that directly supports NASA's terrain mapping mission objectives.

### FDR Software Architecture

The FDR software system was designed around three independent services running on each Jetson Nano, all managed by systemd and auto-starting on boot:

**Rover Controller Service** is a FastAPI HTTP server that serves as the hardware abstraction layer. It accepts Python code submitted via HTTP POST from the ground station, executes it using Python's `exec()` with full system access, and manages command serialization — ensuring only one command runs at a time, with early-stop capability. This full-access execution model allows the AI system on the ground station to generate and execute any rover operation without the software team having to anticipate every possible command at design time.

**Jetson Mapper Service** drives the ZED 2i stereo camera. It publishes a live MJPEG video stream consumed by the web application, and a WebSocket stream delivering real-time 3D map data to the operator dashboard.

**WiFi Watchdog Service** monitors network connectivity and automatically reconnects the Jetson to the ground station's WiFi access point whenever the link drops. This ensures the rover remains reachable from the moment power is applied without requiring any operator intervention.

From the operator's perspective, the startup procedure is: plug in the battery. All three services initialize automatically, the rover connects to the ground station, and the system is ready to receive commands within boot time.

### Ground Station & AI System

The ground station runs a Flask-based single-page web application on the operator's laptop. When an operator enters a natural language command — for example, "drive to the rock formation and collect a soil sample" — the application submits the prompt to a locally running Ollama instance serving the `gemma4:e2b` model. The model generates Python code, which the application then POSTs to the rover's FastAPI service for execution. The entire inference pipeline runs offline. This is a hard design requirement, not a preference: the Moon and Mars have no internet connectivity, and any AI system used in the operational environment must function without external network access. SSH access to the Jetson is also available for direct debugging by technically qualified users.

### Motor and Servo Upgrades

Plastic TT motors were replaced with metal gear TT motors across all six drive positions, resolving the durability concern identified during v1 testing. The SparkFun Servo pHAT was replaced with an Adafruit servo driver communicating over I2C, providing more precise PWM control of the four rocker-bogie servos. Motor controllers were upgraded to GoBilda high-current units also addressed over I2C, consolidating the motor and servo control onto a single communication bus and simplifying the wiring harness.

---

## 7. Performance Evaluation

### Mechanical Performance

The rocker-bogie suspension met its design targets during field testing. The rover maintained chassis stability while navigating over rocks and irregular terrain, with all six wheels maintaining ground contact across obstacles approximately twice the wheel diameter. The half-tread rear configuration demonstrated measurably improved grip on the diatomaceous earth regolith simulant compared to the all-independent v1 wheel layout, with no wheel spin-out observed on the test terrain at normal operating speeds. The suspension's 60-degree tilt range — used for solar panel orientation — was confirmed functional through servo-driven articulation.

### Communication Performance

The LoRa system reliably achieved its ~1 km range target. Command latency at close range was consistent with the 0.1-second packet interval. Occasional missed packets were observed during extended operation — typically 1–3 dropped fragments per 100 transmissions under normal conditions. While this rate is operationally acceptable for low-stakes testing, it falls short of the reliability expected for an environment like the lunar surface where a missed command during a critical maneuver could result in rover loss. The WiFi link performed as expected for high-bandwidth data: live MJPEG video and WebSocket map streams were maintained stably within the hotspot's range.

### AI & Software System Performance

The Ollama-based AI code generation system successfully translated natural language commands into executable Python across a range of rover control tasks during testing. The local model approach performed well offline, confirming that the system is viable for deployment in a communications-isolated environment. Command serialization and early-stop functionality operated correctly, preventing command collisions during concurrent testing.

### Summary Table

| Subsystem | Target | Achieved |
|-----------|--------|----------|
| Obstacle clearance | 2× wheel diameter | Confirmed |
| Tilt tolerance | 45° (design), 30° (operational) | Confirmed |
| LoRa range | 1 km | Confirmed |
| LoRa reliability | 100% | ~97–99% (packet loss known issue) |
| WiFi video streaming | Stable MJPEG | Confirmed |
| Auto-startup on boot | Zero operator steps | Confirmed |
| AI natural language control | Functional offline | Confirmed |
| ZED 2i 3D mapping | Real-time map generation | Confirmed |

---

## 8. Successes, Failures & Root Cause Analysis

### Successes

**Custom LoRa mesh protocol.** Building a fragmentation and mesh relay protocol from scratch — without relying on a commercial stack — was one of the team's most technically ambitious achievements. The protocol functioned reliably across all field tests and demonstrated the feasibility of a self-extending mesh network for multi-rover swarms.

**Offline AI control system.** Designing the entire AI pipeline to run locally on the ground station laptop, with no internet dependency, was the correct architectural decision for the operating environment. The system worked as intended: a non-technical user could issue natural language commands and have them executed on the rover without writing any code.

**Zero-configuration startup.** Implementing all Jetson services as systemd units with automatic WiFi reconnection eliminated operator setup steps entirely. This is a direct improvement to operational reliability and directly satisfies the NASA requirement for non-technical operability.

**Iterative design under constraint.** The team's willingness to abandon the four-rover plan at PDR, build a strong single rover, and then return to a three-rover swarm at FDR demonstrated sound engineering judgment. Each phase produced something functional rather than an incomplete multi-system prototype.

### Failures & Root Cause Analysis

**LoRa packet loss.** Occasional missed packets remain an unresolved issue. Root cause has not been fully diagnosed but likely involves radio interference, timing edge cases in the fragmentation logic, or marginal SNR at extended range. This is the most significant open reliability issue going into future development.

**Terrain scale reduction.** The BothScape terrain was reduced from the planned 8 ft × 8 ft to 4 ft × 4 ft due to budget constraints. Root cause: the $1,000 parts order required for the full terrain exceeded available funds. Earlier cost estimation and phased procurement could have mitigated this.

**Electrostatic dust cleaning not implemented.** The MIT-inspired dust mitigation system remained a proposed solution only. Root cause: funding constraints prevented acquisition of the required electrode hardware and guide-rail components.

**CDR rover not fully mission-capable at presentation.** While the CDR rover was operational for demonstration, it did not complete all 10 NASA missions at CDR. Root cause: the timeline required to implement the full software stack — mapping, object detection, swarm coordination — exceeded the time available between PDR and CDR given the team's other academic commitments.

---

## 9. Improvements & Reflection

### Technical Improvements for Future Teams

The most impactful near-term improvement is implementing ACK/retry logic in the LoRa protocol. The current implementation is best-effort: if a fragment is lost, it is not retransmitted. A simple acknowledgment scheme — where the receiver confirms each fragment and the sender retransmits unacknowledged fragments after a timeout — would bring reliability to the level NASA expects for an operational system.

For the AI system, the team recommends evaluating `llama.cpp` as a replacement for Ollama. `llama.cpp` is a lighter-weight inference engine that runs the same GGUF-format models with lower memory overhead, which matters when running on a laptop that may also be managing video streams and SSH sessions simultaneously.

The BothScape terrain should be scaled to 8 ft × 8 ft when funding allows. The 4 ft × 4 ft modular design was the correct interim solution, but the larger format better demonstrates the 50-meter operating radius concept and provides more meaningful obstacle diversity for rover testing.

Finally, physical implementation of the electrostatic dust cleaning system would complete one of the most technically novel aspects of the proposed design. Electrode hardware and guide rails are inexpensive relative to the rest of the project budget; this feature was omitted due to timing, not technical infeasibility.

### Reflection on the Engineering Process

The METSAnauts project reinforced several foundational engineering lessons. Most significantly: constraints are not obstacles to good design — they are inputs to it. The budget constraint that forced the consolidation from four rovers to one did not make the project worse. It made the first working rover better, because all available effort went into one high-quality system rather than being diluted across four incomplete ones.

The experience of iterating from a popsicle-stick concept model to a field-tested, AI-controlled three-rover swarm over nine months demonstrated that engineering progress is rarely linear. The most important decisions — choosing LoRa over WiFi-only for long-range comms, choosing local AI over cloud, choosing modular connectors over soldered joints — were made correctly early because the team invested time in research before committing to a design.

On teamwork: distributing the project across five clearly defined roles (software, hardware, design, operations, finance) meant that work could proceed in parallel across subsystems. The clearest lesson was that communication between roles — specifically, flagging budget and timeline risks early rather than late — is as important as any individual technical contribution.

The METSAnauts team produced a rover system that a non-technical HERAnaut can operate from natural language commands on a laptop, with zero setup, from the moment a battery is connected. That outcome maps directly to the mission need NASA defined at the start of the program — and that alignment, more than any individual technical achievement, is what defines a successful engineering design.

---

*Total word count: approximately 4,500 words*

*For visual documentation of all hardware, field testing, and terrain construction, see the project gallery at [metsanauts.com/gallery](https://www.metsanauts.com/gallery).*
