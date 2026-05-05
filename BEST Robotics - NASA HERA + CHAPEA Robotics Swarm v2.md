# **Requirements Document for BEST Robotics, and NASA HUNCH High School Students** {#requirements-document-for-best-robotics,-and-nasa-hunch-high-school-students}

## **Sponsored by NVIDIA and NASA HUNCH**  ***AI/ML integration and use of NVIDIA Jetson Nano earn bonus points*** {#sponsored-by-nvidia-and-nasa-hunch-ai/ml-integration-and-use-of-nvidia-jetson-nano-earn-bonus-points}

![][image1]

# 

# **Table of Contents** {#table-of-contents}

[**Requirements Document for BEST Robotics, and NASA HUNCH High School Students	1**](#requirements-document-for-best-robotics,-and-nasa-hunch-high-school-students)

[Sponsored by NVIDIA and NASA HUNCH](#sponsored-by-nvidia-and-nasa-hunch-ai/ml-integration-and-use-of-nvidia-jetson-nano-earn-bonus-points)  
 [AI/ML integration and use of NVIDIA Jetson Nano earn bonus points	1](#sponsored-by-nvidia-and-nasa-hunch-ai/ml-integration-and-use-of-nvidia-jetson-nano-earn-bonus-points)

[**Table of Contents	2**](#table-of-contents)

[Key Takeaway:	3](#key-takeaway:)

[Make sure you look at the Minimum Mission Set (Acceptance Demonstrations) and Student-Derived New Innovative Missions	3](#make-sure-you-look-at-the-minimum-mission-set-\(acceptance-demonstrations\)-and-student-derived-new-innovative-missions)

[1\. Purpose	3](#1.-purpose)

[2\. Mission Context	4](#2.-mission-context)

[HERA (Lunar Analog)	4](#hera-\(lunar-analog\))

[CHAPEA (Mars Analog)	4](#chapea-\(mars-analog\))

[3\. Scope	4](#3.-overall-scope)

[4\. System Overview — What You Must Deliver	5](#4.-system-overview-—-what-you-must-deliver)

[4.1 Physical Test Environments (2 Terrains “Think Reversible Sweater )	5](#4.1-physical-test-environments-\(2-terrains-“think-reversible-sweater-\))

[4.2 Robotic Swarm (3+ robots; As required to complete missions)	5](#4.2-robotic-swarm-\(3+-robots;-as-required-to-complete-missions\))

[4.3 Mission Control \+ Evidence Package	5](#4.3-mission-control-+-evidence-package)

[5\. Requirements	6](#5.-requirements)

[5.1 Terrain (Moon \+ Mars) — Physical Specifications	6](#5.1-terrain-\(moon-+-mars\)-—-physical-specifications)

[5.2 Moon Environmental Features (HERA Moonscape)	6](#5.2-moon-environmental-features-\(hera-moonscape\))

[5.3 Robotics — General Specifications (All Robots)	7](#5.3-robotics-—-general-specifications-\(all-robots\))

[AI/ML Bonus:	7](#ai/ml-bonus:)

[5.4 Robot Roles (Minimum)	7](#5.4-robot-roles-\(minimum\))

[5.5 Control, Monitoring, and Data	8](#5.5-control,-monitoring,-and-data)

[5.6 Reliability, Survivability, and Swarm Strategy	9](#5.6-reliability,-survivability,-and-swarm-strategy)

[6\. Minimum Mission Set (Acceptance Demonstrations)	9](#6.-minimum-mission-set-\(acceptance-demonstrations\))

[7\. Student-Derived New Innovative Missions	10](#7.-student-derived-new-innovative-missions)

[7.1 Scoring Rules (Innovation)	10](#7.1-scoring-rules-\(innovation\))

[7.2 Google Form Prompts (Public Submission)	10](#7.2-google-form-prompts-\(public-submission\))

[8\. Rubrics (0/1/5 Scoring System)	11](#8.-rubrics-\(0/1/5-scoring-system\))

[8.1 Minimum Requirements Rubric (Build \+ Evidence)	11](#8.1-minimum-requirements-rubric-\(build-+-evidence\))

[8.2 Minimum Mission Rubric (M1–M10; each mission scored separately)	12](#8.2-minimum-mission-rubric-\(m1–m10;-each-mission-scored-separately\))

[8.3 Innovative Mission Rubric (Submission \+ Verification)	13](#8.3-innovative-mission-rubric-\(submission-+-verification\))

[9\. Project Summary Checklist	13](#9.-project-summary-checklist)

[Acknowledgment:	14](#acknowledgment:)

[**BEST Robotics \- NASA HERA \+ CHAPEA Robotics Swarm v2.1	2**](#heading=h.yy6x1onwf34y)

## **Key Takeaway:** {#key-takeaway:}

This document provides all requirements, deliverables, and scoring rubrics for the HERA (Moon) and CHAPEA (Mars) Analog Robotics Swarm project. It is designed for high school teams participating in BEST Robotics, and NASA HUNCH, with sponsorship from NVIDIA and NASA HUNCH. AI/ML features and use of NVIDIA Jetson Nano are strongly encouraged and rewarded.

## **Make sure you look at the Minimum Mission Set (Acceptance Demonstrations) and Student-Derived New Innovative Missions** {#make-sure-you-look-at-the-minimum-mission-set-(acceptance-demonstrations)-and-student-derived-new-innovative-missions}

---

## **1\. Purpose** {#1.-purpose}

Design and prototype a **remotely operated robotic exploration swarm** and an **8 ft × 8 ft portable terrain testbed** to simulate realistic lunar and Martian surface operations. The project enhances NASA analog missions by enabling students to perform exploration activities under mission-like constraints: remote operation, visual immersion, and day/night lighting.

---

## **2\. Mission Context** {#2.-mission-context}

### **HERA (Lunar Analog)** {#hera-(lunar-analog)}

* Adds a lunar-robotics activity layer to HERA’s 45-day isolation missions.  
* Participants remotely operate robots on a small lunar terrain model (physical or VR), maintaining mission protocols and immersion.

### 

### 

### **CHAPEA (Mars Analog)** {#chapea-(mars-analog)}

* Uses the same student platform (terrain \+ swarm) for Mars-like surface operations.  
* Focuses on dust/traction, longer traverses, fault tolerance, and autonomy.

---

## **3\. Overall Scope** {#3.-overall-scope}

* **Develop** an 8 ft × 8 ft portable landscape (physical preferred; VR acceptable).  
* **Create** specialized robotic units (swarm).  
* **Implement** remote control/monitoring interfaces.  
* **Test** through mission scenario procedures.

---

## 

## 

## **4\. System Overview — What You Must Deliver** {#4.-system-overview-—-what-you-must-deliver}

### **4.1 Physical Test Environments (2 Terrains “Think Reversible Sweater )** {#4.1-physical-test-environments-(2-terrains-“think-reversible-sweater-)}

* **D2:** Mars Terrain (CHAPEA Marscape) — 8 ft × 8 ft, reconfigurable with rocks, dunes, and soft soil zones.  
* **D1:** Moon Terrain (HERA Moonscape) — 8 ft × 8 ft, crater-based, reconfigurable.

### **4.2 Robotic Swarm (3+ robots; As required to complete missions)** {#4.2-robotic-swarm-(3+-robots;-as-required-to-complete-missions)}

* **D3:** Swarm of  specialized robots (minimum 3+ with justification).  
* Robots must be **camera/sensor-only driven** (no direct line-of-sight except in garage/service area).

### **4.3 Mission Control \+ Evidence Package** {#4.3-mission-control-+-evidence-package}

* **D4:** Mission control interface, video recordings, data logs, and a test report demonstrating all minimum missions and requirements.

---

## 

## **5\. Requirements** {#5.-requirements}

### **5.1 Terrain (Moon \+ Mars) — Physical Specifications![][image2]** {#5.1-terrain-(moon-+-mars)-—-physical-specifications}

### **5.2 Moon Environmental Features (HERA Moonscape)** {#5.2-moon-environmental-features-(hera-moonscape)}

* LUN-001: Crater-within-crater topology (south pole region).  
* LUN-002: 3D printed rocks and scaled lunar surface materials.

### 

### 

### **5.3 Robotics — General Specifications (All Robots)** {#5.3-robotics-—-general-specifications-(all-robots)}

### **![][image3]**

### **AI/ML Bonus:** {#ai/ml-bonus:}

Including AI and machine learning (e.g., for autonomy, perception, or decision-making) will earn bonus points. Use of an **NVIDIA Jetson Nano** is recommended for advanced AI/ML features.

**Try New Arduino Uno Q**

### **5.4 Robot Roles (Minimum)** {#5.4-robot-roles-(minimum)}

* **ROB-T1 Transportation Robot**

  * Payload: 0.5–1.0 lb  
  * Carry a 10 cm cube (CubeSat prop)  
  * Stable mobile base (no tip-over)  
      
      
* **ROB-T2 Manipulation Robot**

  * Lifting mechanism/arm  
  * Precision placement  
  * Cube transfer (robot↔robot and robot↔terrain)

* **ROB-T3/T4 Support Robot(s)**

  * Specialized function(s) TBD (e.g., tow/recovery, comms relay, mapping beacon, tool delivery, field repair)  
  * Must complement transport/manipulation units

### **5.5 Control, Monitoring, and Data** {#5.5-control,-monitoring,-and-data}

|  | ![][image4] |
| ----- | ----- |
|  |  |
|  |  |
|  |  |

### 

### **5.6 Reliability, Survivability, and Swarm Strategy![][image5]** {#5.6-reliability,-survivability,-and-swarm-strategy}

### ---

## **6\. Minimum Mission Set (Acceptance Demonstrations)** {#6.-minimum-mission-set-(acceptance-demonstrations)}

All missions begin at a **garage area** adjacent to the lander/habitat prop, with a recharge station.![][image6]

---

## 

## 

## **7\. Student-Derived New Innovative Missions** {#7.-student-derived-new-innovative-missions}

### ***7.1 Scoring Rules (Innovation)*** {#7.1-scoring-rules-(innovation)}

* **\+5 points:** First team to submit a new innovative mission that is approved and published.  
* **\+10 points per team:** Each team that completes a *Verified* innovative mission.  
* **Important:** Missions not completed by at least two (2) different schools will have their bonus set to **0**.

### **7.2 Google Form Prompts (Public Submission)** {#7.2-google-form-prompts-(public-submission)}

**Required fields:**

* School Name  
* Team Name  
* Advisor Email  
* Mission Name (public)  
* Environment: Moon / Mars / Either  
* Mission Objective (1 sentence)  
* Props Required (list)  
* Setup Diagram Upload (image/PDF)  
* Step-by-step Procedure (numbered)  
* Success Criteria (measurable)  
* Failure Conditions  
* Safety Hazards \+ Mitigations  
* Evidence Link(s): video \+ photos \+ logs  
* Verification Instructions (how a second school can reproduce/confirm)  
* Checkbox: “I agree this mission will be posted publicly.”

---

## 

## 

## **8\. Rubrics (0/1/5 Scoring System)** {#8.-rubrics-(0/1/5-scoring-system)}

### **8.1 Minimum Requirements Rubric (Build \+ Evidence)** {#8.1-minimum-requirements-rubric-(build-+-evidence)}

### **![][image7]**

### 

### 

### **8.2 Minimum Mission Rubric (M1–M10; each mission scored separately)** {#8.2-minimum-mission-rubric-(m1–m10;-each-mission-scored-separately)}

|  |  |
| ----- | :---- |
| 0 | No evidence / not attempted |
| 1 | Mission completed with major assistance, resets, or unclear success |
| 5 | Mission completed end-to-end under camera-only rule, with logs \+ repeatability (≥2 runs or strong justification) |

### 

### 

### **8.3 Innovative Mission Rubric (Submission \+ Verification)** {#8.3-innovative-mission-rubric-(submission-+-verification)}

**A) Mission Submission (for publication)![][image8]**

---

## **9\. Project Summary Checklist** {#9.-project-summary-checklist}

* 8 ft × 8 ft Moon and Mars terrains, reconfigurable, with ≥3 ft walls and day/night lighting  
* 4 specialized robots (minimum 3 with justification), each ≤12 in³, with required sensors and runtime  
* Bluetooth command/control, camera-only operation, SD card video storage, data logging  
* All 10 minimum missions demonstrated and documented  
* At least one student-derived innovative mission submitted (Google Form) and, if possible, completed by two schools  
* AI/ML features and/or NVIDIA Jetson Nano integration for bonus points  
* All evidence (photos, videos, logs) organized for scoring and review

---

### Acknowledgment: {#acknowledgment:}

This project is sponsored by **NVIDIA** and **NASA HUNCH**. Teams are encouraged to leverage AI/ML and NVIDIA Jetson Nano for advanced capabilities and bonus points.

---