# METSAnauts — Complete Technical Reference
**NASA HUNCH | HERA Rover & BothScape Simulation Project**
**Ranchview High School | Team: Neil Rao, Arun Skanda Rebbapragada, Arnav Sangle, Advay Singi, Sanay Tyagi | Mentor: David Berry**

**Website**: [metsanauts.com](https://www.metsanauts.com) | **Gallery**: [metsanauts.com/gallery](https://www.metsanauts.com/gallery) | **Codebase**: [github.com/skandacode/RoverSystemV2](https://github.com/skandacode/RoverSystemV2)

---

## 1. Program Context & Mission Need

### 1.1 NASA HUNCH & HERA Background
- **NASA HUNCH** (High School Students United with NASA to Create Hardware): program engaging HS students in real engineering deliverables for NASA.
- **HERA** (Human Exploration Research Analog): 45-day isolation analog mission simulating lunar/Martian surface conditions for Artemis II preparation. Participants ("HERAnauts") live under mission constraints; astronauts include teachers, geologists, and specialists—not necessarily engineers or CS experts.
- **CHAPEA** (Crew Health and Performance Exploration Analog): Mars-focused analog mission sharing the same student platform.
- Project context: Artemis IV planning to launch within 2-3 years from project start.

### 1.2 Problem Statement
Design and prototype a remotely operated robotic exploration swarm + an 8 ft × 8 ft (later revised to 4 ft × 4 ft modular) portable terrain testbed simulating realistic lunar and Martian surface operations. Rovers support scouting, mining/sample collection, payload transport, and terrain mapping within a 50-meter operating radius of the central HERA module.

### 1.3 Formal Requirements (from NASA HUNCH requirements doc)
| ID | Requirement |
|----|-------------|
| GEN | 3+ specialized robots (minimum), each ≤12 in³; camera/sensor-only operation (no direct line-of-sight except garage/service area) |
| LUN-001 | Crater-within-crater topology (south pole region) |
| ROB-T1 | Transportation Robot: 0.5–1.0 lb payload; carry lunar regolith; stable mobile base |
| ROB-T2 | Manipulation Robot: lifting mechanism/arm; precision placement; pick up lunar regolith |
| ROB-T3/T4 | Support Robot(s): tow/recovery, comms relay, mapping beacon, tool delivery, or field repair |
| CTRL | Bluetooth command/control; camera-only operation; SD card video storage; data logging |
| MISS | All 10 minimum missions demonstrated and documented (M1–M10) |
| AI/ML | AI/ML features and/or NVIDIA Jetson Nano integration (bonus points) |
| COMM | 50-meter operating radius; IR, WiFi, or Bluetooth receiver for commands from central control module |

### 1.4 Success Criteria
- Rover swarm capable of completing all NASA-specified missions
- Fully modular, field-repairable design (no specialized engineering background required for repair)
- Operable by HERAnauts without IT/CS background
- CubeSat collection and transport capability
- Object detection (CubeSat recognition) via camera

---

## 2. Project Timeline & Design Evolution

### Phase 0 — Project Start (September 2025)
- Team formed; project scope defined
- Initial concept: 4-rover swarm (transport, manipulation, 2 support)
- Decision made early to **consolidate** all features into one rover due to time/budget constraints

### Phase 1 — PDR (October 2025)
- **Terrain**: 2 ft × 2 ft foam board model of lunar surface; diatomaceous earth regolith; received positive NASA feedback; judges recommended larger scale and dual-terrain (lunar + Martian)
- **Rover**: v1 prototype with rocker-bogie suspension (popsicle stick + 3D-printed mini-wheel concept model, then full v1); TT motors; L298N + DRV8871 motor drivers; Raspberry Pi control; RP2040 LoRa comms
- **Electrical schematic**: v1 completed; first LoRa transmission tested between devices
- **NASA feedback at PDR**: liked rocker-bogie; encouraged modularity; encouraged solar panel expansion; suggested larger terrain and Martian terrain addition
- **Key PDR decision**: Merge all 4 rover roles into a single rover
- **Key Constraint**: Cost -> 4 rovers would cost around $3000, whereas we only had $500 available 

### Phase 2 — Between PDR and CDR (Nov–Jan 2025–2026)
- **Terrain**: Neil Rao designed 8 ft × 8 ft schematic; submitted parts order worth $1000; researched virtual 3D environments as alternative; researched Martian soil composition
- **Mechanical**: Skanda designed modular swap-out system in Onshape; upgraded to half-tread system for rear wheels
- **Communication**: Skanda programmed LoRa module on RP2040 in Python; tested laptop→rover command transmission
- **Research**: Advay researched MIT electrostatic dust cleaning system; Sanay researched LDR-based sun tracking; both conducted sponsorship outreach (~10 companies contacted, 1 interested sponsor identified; sponsorship packet created)
- **Funding**: Grant applications submitted; potential sponsor in negotiation
- **Terrain Updates**: $1000 proved to be too much for CDR, decided to downscale to 4ft x 4ft for CDR

### Phase 3 — CDR (February 2026)
- **Terrain**: 4 ft × 4 ft "BothScape" planned (MoonScape + MarsScape); interim research into virtual environments during parts delivery wait
- **Rover v2**: Larger chassis; electronics on top for demo; WAGO connectors for modularity; half-tread rear wheel system implemented; webcam added for video feed; WiFi hotspot/LAN for high-bandwidth data
- **Software**: AI-assisted control app developed (dashboard, command pad, live video feed, AI code generation); WAGO connectors enable field-swappable components
- **Communication upgrade**: Added WiFi hotspot on central module for video/high-bandwidth; LoRa retained as long-range fallback
- **Identified gap**: Proof of concept operable rover demonstrated at CDR; target for FDR is fully functional rover swarm with mapping and pickup functionality

### Phase 4 — FDR / Current State (April 2026)
- **Architecture shift**: Return to **3-rover swarm** (Claw Rover, Recon Rover, Support Rover)
- **Computing upgrade**: Raspberry Pi → **NVIDIA Jetson Nano** as central controller
- **Vision upgrade**: **ZED 2i stereo camera** for real-time 3D spatial mapping
- **Motors upgrade**: Metal gear TT motors replaced plastic TT motors for higher durability. 
- **Electronics upgrade**: Replaced the Pi servo hat with an Adafruit servo driver over I2c. Connected Servos to servo driver, and used a PWM to motor controller

---

## 3. Mechanical Design

### 3.1 Suspension System — Rocker-Bogie
- Lineage: NASA Sojourner (1997), Spirit, Opportunity, Curiosity, Perseverance
- **Operating principle**: Each side linked through a differential; one wheel climbing adjusts opposite side to level chassis; prevents excessive tilting; reduces frame stress
- **Obstacle clearance**: Up to 2× wheel diameter
- **Tilt tolerance**: Withstands 45° tilt in any direction without tipping (design target mirrors Perseverance); operational limit 30°
- **Servo articulation**: Four standard servos control rocker-bogie joints; enables body angle adjustment for terrain adaptation and solar panel orientation
- **Solar tracking via suspension**: Chassis can tilt/rotate up to 60° to face solar panels toward the sun (critical for lunar south pole low-angle sun)
- **Powered suspension**: Body angle adjusts dynamically for terrain OR sun position

### 3.2 Wheel System — Half-Tread Hybrid
- **Configuration**: 6 independently motored wheels
- **Half-tread layout**: Rear 4 wheels (middle + rear pairs on each side) linked by continuous rubber tread; front 2 wheels remain independent
- **Benefits**: Increased grip on granular regolith; improved turning/pivoting vs. full-tread; superior stability vs. all-independent wheels; wider ground contact prevents sinking in loose, sandy regolith
- **Wheel material (v2)**: 3D printed with Creality HyperPLA — lightweight, easy to manufacture and replace
- **Drive scheme**: Front 2 wheels (independent motors); rear tread driven by 1 motor per side (2 motors for 4 wheels)
- **Ground clearance**: High clearance for rock obstacle navigation
- **Weight distribution**: Distributed evenly across all 6 wheels; no single-point stress concentration

### 3.3 Motor Allocation
| Phase | Motor Type | Qty | Driver |
|-------|-----------|-----|--------|
| v1 (PDR) | Plastic TT motors | 6 | L298N (1×) + DRV8871 (2×) |
| FDR | Metal TT motors | 6 | Gobilda High current Motor Controller with I2C servo driver |

### 3.4 Robotic Arm / Claw
- **Function**: Mining-style sample collection (loose soil, small rocks), CubeSat retrieval, payload manipulation
- **Camera**: Mounted directly on claw for close-up precision targeting; feeds live video to operator
- **Control**: Remote via WiFi (short-range) or LoRa (long-range) based on rover distance
- **Servos**: Gobilda Proton servos for joint articulation
- **Capability**: Handles loose soil, small rocks, surface materials without tip-over; designed for robot↔robot and robot↔terrain cube transfer

---

## 4. Electronics & Computing

### 4.1 Core Computing
| Version | Controller | Notes |
|---------|-----------|-------|
| v1/v2 (PDR–CDR) | Raspberry Pi 5 | Primary onboard compute; confirmed on research page |
| FDR (current) | NVIDIA Jetson Nano | Central control; WiFi + LoRa interfaces; AI/ML processing |

**FDR Jetson software stack**: Three independent systemd services run on the Jetson — Rover Controller (FastAPI hardware API), Jetson Mapper (ZED SDK, MJPEG + WebSocket streams), and WiFi Watchdog (auto-connect to ground station). See Section 14 for full architecture details.

### 4.2 Full v1/v2 Electronics Stack (PDR–CDR)
- **SBC**: Raspberry Pi 5 + USB Camera
- **GPIO expansion**: SparkFun Servo pHAT
- **Motor drivers**: 1× L298N dual H-bridge
- **Servos**: 4× standard servos (rocker-bogie)
- **Power**: 10S NIMH (12V) pack plugged into servo phat to power Raspberry pi and servos
- **Connectors**: WAGO connectors with ferrules for modular field repair
- **LoRa MCU**: RP2040 LoRa microcontroller (Adafruit USB-C PD version) connected to Pi via USB-A to USB-C

### 4.3 FDR Sensor Suite (Terrain Mapping Rover)
| Component | Cost | Qty (Per rover) | Role |
|-----------|------|-----|------|
| Jetson Nano | $250 | 1 | Main compute |
| NVME SSD | ~$130 | 1 | Storage |
| Metal TT motor | ~$13 | 6 | Drive |
| ZED 2i Stereo Camera | $500 | 1 | Real-time 3D spatial mapping; mounted on support rovers |
| Motor Controller | $40 | 2 | Controlling Movement |
| Servo Controller | $20 | 1 | Controlling Movement |
| Gobilda Servos | $20 | 4 | Controlling Rocker Bogie |


### 4.4 Power System
- **Primary**: 2S 18650 lithium-ion pack (solar rechargeable)
- **Solar charging**: Panels mounted on underside of chassis; tilt via rocker-bogie suspension to capture low-angle sun (lunar south pole geometry)
- **Sun tracking**: **Dual** LDRs (light-dependent resistors) optimize panel orientation across low sun angles; Raspberry Pi processes readings to locate strongest light direction; rover repositions chassis to face sun; design functional in either orientation (reversible)
- **LDR principle**: Resistance decreases under strong light; sensors compare intensity differentials; threshold crossing triggers motor rotation
- **Thermal environment**: Moon: −240°C to +160°C (reference: NASA VIPER solar panels); Mars: dust storm attenuation (reference: NASA InSight)
- **Mars consideration**: Solar panels viable on Moon; RTG (Radioisotope Thermoelectric Generator) preferred for Mars due to dust storms (reference: Opportunity rover with lithium-ion + solar as current implementation)
- **Dust mitigation (proposed)**: MIT electrostatic repulsion system — electrode passes above panel surface, charges dust particles, panel charge repels dust; no water or brushes required; operated via motor + guide rails; NASA-tested on Moon. Not physically implemented due to funding constraints; included in presented solution.

---

## 5. Communication System

### 5.1 Communication Stack (Multi-Layer)
| Layer | Technology | Range | Primary Use |
|-------|-----------|-------|------------|
| Primary | WiFi / LAN hotspot | ~20 ft (~6 m) | High-bandwidth: video feed, claw camera, app commands |
| Fallback | LoRa | ~1 km | Long-range commands when WiFi unavailable |
| Close-range video | Bluetooth | ~20 ft | Close-up claw-camera video transfer |
| Infrastructure | Laptop-hosted LAN | Local | Direct rover connection; no internet required |

### 5.2 LoRa Hardware
- **Radio chip**: RFM95
- **MCU**: RP2040 coprocessor — manages LoRa protocol **independently** from the main Pi; USB hot-swap capability (communication module replaceable without disassembly)
- **Range**: ~1 km

### 5.3 LoRa Packet Protocol (Custom Fragmentation — similar to ZigBee APS/6LoWPAN)
**Packet structure (252 bytes max):**
| Bytes | Field | Description |
|-------|-------|-------------|
| 0 | Addresses | From/To (4 bits each) |
| 1–4 | Packet Content ID | Unique message identifier |
| 5–6 | Fragment Index | Current fragment number |
| 7–8 | Total Fragments | Total fragments in message |
| 9–251 | Payload | Up to 243 bytes of data |

**Protocol behavior:**
- Packets sent every 0.1 seconds
- **Mesh relay**: Any rover receiving a fragment not addressed to it rebroadcasts if it hasn't seen that Packet ID + Fragment Index before → extends coverage
- **Message format**: Always JSON
- **Supported operations**: `"command"` (execute shell command remotely) and `"edit_file"` (write/modify file)
- **Serial chain**: Pi streams JSON line-by-line to RP2040 over serial → RP2040 validates JSON → converts to bytes → fragments → transmits → logs packet IDs/indexes
- **Reconstruction**: Destination rover collects all fragments, reassembles by fragment index, executes command/file write, resets shell state
- **Loop prevention**: Persistent "seen-packet" set continuously backed to onboard storage
- **Encoding**: Commands encoded in hexadecimal for transmission; rover decodes and executes automatically

### 5.3 WiFi / LAN Architecture
- Central module hosts WiFi access point (hotspot)
- Rover connects to hotspot for high-bandwidth data (live video, large telemetry)
- Enables video feed transmission impossible over LoRa (243-byte LoRa payload insufficient for video)
- Laptop hosts LAN as redundant path; rover connects directly without internet

### 5.4 Known Issue: LoRa Packet Loss
- Occasional missed packets observed during field testing; root cause not fully diagnosed
- Impact: minor operational nuisance; operator can manually resend commands
- **Future work**: implement ACK/retry logic or idempotent command design to meet NASA reliability standards
- This is an open problem for future teams — the protocol fragmentation and mesh relay are solid, but link-layer reliability needs improvement

### 5.5 Control App — Ground Station

**Stack:**
- **Framework**: Flask (Python) web server running on the ground station laptop
- **Architecture**: Single-page application (SPA) — all rover interaction happens in one browser view
- **AI engine**: [Ollama](https://ollama.ai) running locally on the ground station laptop, serving `gemma4:e2b`
- **Why local AI**: Moon and Mars have no internet access. The entire AI stack must operate offline. This is a hard constraint, not a preference. (Future teams: consider `llama.cpp` as a lighter-weight alternative to Ollama.)

**AI Code Generation Flow:**
1. Operator types a natural language command (e.g., "drive forward 2 meters and take a photo")
2. Web app sends the prompt to Ollama (`gemma4:e2b`) via local HTTP call
3. Ollama returns generated Python code
4. Web app HTTP POSTs the Python code to the Rover Controller service on the Jetson
5. FastAPI receives the code and executes it via `exec()` with full system access
6. Rover performs the action

**Execution model:**
- Commands are **serialized** — only one command runs at a time
- A running command can be **stopped early** via a stop endpoint
- The generated Python has **full access to the rover's filesystem and hardware APIs** — this is intentional; it gives astronauts maximum flexibility without needing to anticipate every possible operation at design time
- Security is a non-issue in the operational environment (closed LAN, no external network)
- SSH access is also available on the Jetson for direct debugging

**Earlier app (PDR/v1)**: "HERA Command Encoder" — encoded commands to hex for LoRa transmission; simple UI with no AI

---

## 6. Rover Swarm Architecture

### 6.1 Evolution of Swarm Concept
| Phase | Swarm Config |
|-------|-------------|
| Initial plan | 4 rovers (transport, manipulation, 2 support) |
| PDR decision | Consolidate to 1 rover (budget/time) |
| CDR presentation | 1 functional rover |
| FDR/current | 3-rover swarm (Claw, Recon, Support) |

### 6.2 FDR Rover Roles
| Rover | Role | Key Systems |
|-------|------|-------------|
| Claw Rover | Sample collection, manipulation, CubeSat retrieval | Robotic arm, claw camera, WiFi/LoRa |
| Recon Rover | Terrain mapping, sensor coverage, comms relay | ZED 2i stereo camera, 3D spatial mapping, relay |
| Support Rover | Logistics, redundancy, swarm coordination | ZED 2i stereo camera, LoRa relay, recovery assist |

### 6.3 Spatial Mapping (FDR Innovation)
- **ZED 2i stereo cameras** mounted on front of both support rovers
- Generates real-time 3D maps of surrounding terrain
- Dual-camera overlapping fields of view across swarm → full area coverage
- Enables reliable obstacle detection across full operating area
- Improves CubeSat detection and handling reliability

---

## 7. BothScape — Terrain Simulation Environment
> Photos of the terrain and rover in operation: [metsanauts.com/gallery](https://www.metsanauts.com/gallery)

### 7.1 Physical Specifications
| Attribute | PDR (Oct 2025) | CDR (Feb 2026) | FDR (current) |
|-----------|---------------|----------------|----------------|
| Size | 2 ft × 2 ft | 8 ft × 8 ft planned | 4 ft × 4 ft modular |
| Structure | Foam board + foam gap filler | Foam board + foam gap filler | Modular 4×4 ft sections |
| Regolith | Diatomaceous earth | Diatomaceous earth | Diatomaceous earth |
| Terrain types | Moon only | Moon + Mars | Moon + Mars |

### 7.2 Moon Region (HERA Moonscape — Shackleton Crater)
- Models lunar south pole: crater-within-crater topology (LUN-001)
- Permanent shadow zones (permanently shadowed craters near Shackleton)
- Reflective crater walls; simulated hydrogen deposits
- Tests rover navigation under low-light conditions
- Validates power system behavior under constrained solar input

### 7.3 Mars Region (CHAPEA Marsscape)
- Smoother, more eroded terrain reflecting Mars' older/more weathered surface
- Regolith mimics basaltic volcanic soil with fine, dusty textures
- Ancient channel features and lava plains (not found on Moon)
- Finer, dustier ground texture vs. Moon's chunky rocks
- Tests rover endurance on dry, powdery surface
- Reconfigurable with rocks, dunes, soft soil zones

### 7.4 Construction Materials
- Foamboard (structural base)
- Foam gap filler / spray foam (terrain topology shaping)
- Diatomaceous earth (regolith simulant surface layer)
- PVC pipe (structural elements, crater walls)
- 3D-printed rock props

### 7.5 Environmental Features
- Day/night lighting simulation (≥3 ft walls required per spec)
- Modular sections for reconfiguration and transport

---

## 8. Research Foundations

### 8.1 Rocker-Bogie Design Basis
- First deployed: NASA Sojourner (1997); proven on Spirit, Opportunity, Curiosity, Perseverance
- Differential linking between sides → chassis leveling over obstacles
- Can climb obstacles 2× wheel diameter
- Perseverance: 45° tilt tolerance; 30° operational limit

### 8.2 Solar Panel Technology
- Concept: LDR-based sun tracking → rotate rover body for maximum energy collection
- **Astrobotic concept**: Vertical solar arrays for lunar bases
- **Thin-film cells**: Flexible solar cells for Mars applications
- **MIT electrostatic dust cleaning**: Electrode charges dust → panel surface charge repels → dust removed without fluids/brushes; NASA-tested on Moon; proposed but not implemented (funding)
- **Thermal ranges**: Moon: −240°C to +160°C; Mars panels degraded by dust storms

### 8.3 Mars vs. Moon Power Trade-off
| Criterion | Moon | Mars |
|-----------|------|------|
| Recommended power | Solar + lithium-ion (Opportunity model) | RTG preferred (dust storms) |
| Team implementation | Solar rechargeable batteries | Same hardware; RTG explained as upgrade path |

---

## 9. Budget (CDR — February 2026)

| Category | Amount (USD) |
|----------|-------------|
| Camera & Sensors | $1,321.53 |
| Body | $950.00 |
| Computing System | $851.63 |
| Travel | $493.38 |
| Power | $326.29 |
| Batteries | $308.45 |
| Miscellaneous | $250.00 |
| Environment | $185.12 |
| Arm Assembly | $125.34 |
| Ground Station | $30 |
| **Total (known)** | **~$4,686.40** |

**Funding sources**: Mark Cuban Companies

---

## 10. Team Roles

| Member | Role | Responsibilities |
|--------|------|-----------------|
| Arnav Sangle | Software Lead | Rover software suite; web-based astronaut control interface; AI natural language command system; team website (metsanauts.com); system integration; mobility research |
| Arun Rebbapragada | Hardware Lead | Full hardware stack across all rovers; electronics integration; motor systems; wiring harnesses; physical assembly; Onshape CAD; LoRa programming (RP2040); fabrication & troubleshooting |
| Neil Rao | Design Lead & BothScape Builder | Overall design vision; BothScape construction (8 ft × 8 ft dual-terrain); material selection; structural layout; terrain research; virtual environment research |
| Advay Singi | Operations Lead | Day-to-day workflow coordination; milestone management; integration testing; deployment protocols across subsystems; research (dust cleaning, modularity); sponsorship outreach |
| Sanay Tyagi | Financial Lead & Documentation Manager | Team finances; expenditure tracking; all project documentation; technical records; write-ups; FDR submission materials; solar tracking research; sponsorship packet creation |
| David Berry | Faculty mentor/instructor | — |

---

## 11. Key Innovations Summary

| Innovation | Technical Detail |
|-----------|-----------------|
| Solar panel placement | Mounted on underside; rocker-bogie suspension tilts toward sun; 60° rotation capability |
| LDR sun tracking | Continuous ambient light reading → Pi processes → rover repositions for maximum exposure |
| Electrostatic dust cleaning | MIT system (proposed); electrode + panel charge → contactless dust removal |
| Half-tread hybrid wheels | Rear 4 wheels treaded, front 2 independent; balances grip, maneuverability, stability |
| LoRa mesh protocol | Custom fragmentation + mesh relay; ~1 km range; JSON command/file operations |
| WiFi + LoRa dual comms | WiFi primary (video/high-BW); LoRa fallback (long range) |
| Astronaut-friendly UI | Web app; AI code generation; predefined commands; no technical knowledge required |
| WAGO modular connectors | Tool-free field replacement; repair without engineering background |
| ZED 2i spatial mapping | Real-time 3D terrain maps across rover swarm; overlapping FOV coverage |
| Jetson Nano AI compute | Onboard AI/ML for perception, object detection, autonomous navigation |

---

## 12. Sections Requiring Completion (White Paper)

Per the outline, the following sections are drafted (PDR content) but FDR sections are empty:

- **Testing & Iteration (CDR)**: Prototypes built (v1, v2); LoRa transmission tested; terrain model tested; app tested — NEEDS SPECIFIC TEST DATA (measurements, success/failure rates, iteration records)
- **Final Design & Readiness (FDR)**: 3-rover swarm; Jetson Nano; ZED 2i; modular 4×4 terrain — NEEDS manufacturing plan, safety analysis, assembly sequence
- **Performance Evaluation**: Expected vs. actual — NEEDS quantitative data (range tests, obstacle clearance measurements, payload tests, comms reliability %)
- **Successes, Failures & Root Cause Analysis**: NEEDS specific incidents, failures, and engineering post-mortems
- **Improvements & Reflection**: NEEDS retrospective on engineering process and team dynamics

---

## 14. Software Architecture

### 14.1 Codebase Overview
- **Repository**: [https://github.com/skandacode/RoverSystemV2](https://github.com/skandacode/RoverSystemV2)
- **Deployment split**: Everything inside the `UI/` folder runs on the **ground station laptop**. Everything outside `UI/` runs on the **Jetson Nano** aboard the rover.

### 14.2 Ground Station (UI/ folder)
| Component | Technology | Role |
|-----------|-----------|------|
| Web server | Flask (Python) | Serves the single-page control app to the operator's browser |
| AI inference | Ollama (`gemma4:e2b`) | Local LLM for natural language → Python code generation; runs entirely offline |
| Control interface | SPA (single page) | Dashboard, video feed, map view, command pad, SSH terminal |

### 14.3 Jetson Nano Services (3 independent systemd services)

All three services start automatically on boot via **systemd**. They are fully independent — no service depends on another being running. Plugging in the battery is the only operator action required; the rover is ready to receive commands within boot time.

#### Service 1: Rover Controller
- **Type**: FastAPI HTTP server
- **Role**: Hardware abstraction layer — accepts Python code from the ground station and executes it on the rover
- **Code execution**: HTTP POST → `exec()` with full system access
- **Concurrency**: One command runs at a time; early stop supported via stop endpoint
- **Interfaces**: Motor controllers (I2C), servo driver (I2C), onboard sensors

#### Service 2: Jetson Mapper
- **Type**: Background service using the ZED SDK
- **Role**: Drives the ZED 2i stereo camera for spatial mapping, object detection, and video streaming
- **Video stream**: MJPEG stream (consumed by the web app for live video feed)
- **Map data**: WebSocket stream (consumed by the web app for real-time 3D map display)
- **SDK**: Standard ZED SDK — refer to [Stereolabs ZED SDK documentation](https://www.stereolabs.com/docs) for integration details

#### Service 3: WiFi Watchdog
- **Type**: Lightweight monitoring service
- **Role**: Automatically connects the Jetson to the ground station's WiFi access point on boot and reconnects if the link drops
- **Why**: Ensures the rover is always reachable over WiFi without any operator intervention — critical for a system operated by non-technical crew

### 14.4 Data Flow Summary
```
Operator (browser)
    │
    ├─ Natural language prompt ──→ Ollama (gemma4:e2b, local) ──→ Python code
    │
    ├─ HTTP POST (Python code) ──→ Rover Controller (FastAPI) ──→ exec() ──→ hardware
    │
    ├─ MJPEG stream (video) ←────── Jetson Mapper (ZED 2i)
    │
    └─ WebSocket (3D map) ←──────── Jetson Mapper (ZED 2i)
```

### 14.5 Design Principles
- **Offline-first**: No component requires internet. Ollama runs local models specifically because the operational environment (Moon/Mars) has no connectivity.
- **Zero-configuration startup**: All services are systemd units. The rover is operational the moment it boots.
- **Full-access execution model**: Rather than pre-defining a fixed command API, the system executes arbitrary Python. This gives HERAnauts (via AI assistance) the ability to instruct the rover to perform any operation without the software team anticipating it in advance.
- **Service independence**: The three Jetson services share no runtime state. Any service can crash, restart, or be updated without affecting the others.

---

---

## 13. Field Testing Evidence (from metsanauts.com Gallery)

Full photo documentation is available at **[metsanauts.com/gallery](https://www.metsanauts.com/gallery)**.

The gallery documents **12 photos** confirming the rover has been physically built and field-tested:
- Rover with tread wheels on terrain (operational)
- Team controlling rover via laptop outdoors (remote operation confirmed)
- Overhead view of rover structure and component layout
- Team member walking alongside rover (demonstrates scale)
- Team inspecting rover during fieldwork (multiple members)
- Rover electronics and wiring close-up (internal systems detail)
- Rover on grass in front of Ranchview High School campus
- Team members adjusting and calibrating rover
- Early prototype overhead on terrain (iteration evidence)

This confirms: rover reached operational status; outdoor field testing conducted; remote laptop control demonstrated; at least 2 rover iterations built (prototype + operational).

For additional project context, visit the main project site at [metsanauts.com](https://www.metsanauts.com).

---
