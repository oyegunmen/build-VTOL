This repo contains an easy-to-follow guide and explanation for component selection for building VTOL (Vertical Take-Off and Landing) aircraft, for any application, from hobbyist to industrial-scale builds.

---

## Glossary
| Term | Definition | Context |
| :--- | :--- | :--- | 
| **AUW** | All-Up Weight | Total weight at any moment during the flight or ground operation |
| **TWR** | Thrust-to-Weight Ratio | Total max thrust divided by AUW |
| **KV** | Motor Velocity Constant | RPM a motor spins per 1 volt (no load) |
| **Sag** | Voltage Drop | Temporary decrease in battery voltage under heavy load. |

## 1. Physics & Aerodynamics
Multirotor flight is governed by the interaction between total weight (mg) and vertical force (thrust).

**Fundamental Formulas**
* Newton’s Second Law: $F = ma$
* Hover Requirement: $\text{Thrust} \ge \text{Weight }$

Thrust equal to weight is practically useless for flight as it leaves no margin for control, wind resistance, or maneuvering.

### TWR Matrix
A 2:1 TWR is the industry standard and "golden rule" for safe, stable, and controllable operation. It allows you to hover at 50% throttle, leaving a "safety buffer" for maneuvering and wind resistance.

| Application | Target TWR | Hover Throttle | Performance Characteristics |
| :--- | :--- | :--- | :--- |
| **Basic** | 2:1 | 50% | Standard for stable, predictable flight. |
| **Agriculture** | 1.5:1 – 2.5:1 | 40% – 70% | Maximizes payload capacity; very low agility. |
| **Delivery Drones** | 2.5:1 – 3:1 | 30% – 40% | Balanced for safety and heavy cargo endurance. |
| **Photography** | 2:1 – 2.5:1 | 40% – 50% | Smooth throttle response and minimal vibration for gimbals. |
| **Mapping & Survey** | 2:1 – 2.5:1 | 40% – 50% | Stable hover for high-resolution LiDAR and multispectral data. |
| **Freestyle** | 4:1 – 6:1 | 15% - 25% | High agility for "punch-outs" and recovering from flips, rolls, and acrobatics |
| **FPV Racing** | 4:1 – 10+ | 10% - 25% | Rapid acceleration, quick manoeuvres and smooth turns. |

**Disk Loading**: Large propellers moving air slowly are more efficient than small propellers moving air quickly. This is why endurance drones use the largest props their frame allows.

## 2. Propulsion System
The motor and Electronic Speed Controller (ESC) must be selected for thermal safety.

*   **Motor Size (e.g., 2207):** The first two digits are diameter (torque); the last two are height (power/heat dissipation).
*   **ESC Safety:** Safety margin of **20-50 %** is recommended depending upon application.

## 3. Energy Storage Systems
The battery is the heaviest component and dictates total flight time (endurance).

| Feature | LiPo (Lithium Polymer) | Li-ion (Lithium-Ion) |
| :--- | :--- | :--- | 
| Best For | Racing, Heavy Lifting (High "C" rating) | Long-range, Mapping (High density) |
| Pros | High burst power; less "sag" | Longer flight times (30–60 mins) |

**Voltage (S-Count)**: Higher voltage (e.g., 6S or 12S) is more efficient because it reduces current draw ($I$), leading to less heat ($I^2R$) and allowing thinner wires.

**Wiring Standards**: Use silicone-insulated wire.
* XT60: Standard 5" FPV (60A).
* AS150: Industrial 12S+ builds; includes an "Anti-Spark" resistor to prevent voltage spikes when plugging in.

## 4. Flight Controller (FC) & Frame
The FC is the "brain", processing sensor data via PID (Proportional-Integral-Derivative) control loop to stabilize craft.

* **Processor Evolution**
    * **F4:** Legacy Budget workhorse.
    * **F7:** Highly popular; universal signal inversion on all UART ports.
    * **H7:** State-of-the-art; required for ArduPilot/PX4 and complex path planning.
* **AutoPilot** - Firmware required to be installed on FC
    * **[Ardupilot](https://ardupilot.org/)**
    * **[PX4](https://px4.io/):**
    * **[Betaflight](https://betaflight.com/):**    
* **Vibration:** Use silicone "gummies" to **soft-mount** the FC. This reduces high-frequency motor noise that confuses the sensors.
* **Frame Material**: Carbon Fiber is preferred for its anisotropic properties (strength can be tuned by layer direction). Avoid frame flex, which causes "asynchronous vibration" and motor overheating.

## 5. Radio Link & Communication
* **[MAVLink](https://mavlink.io/):** Open-source, standard communication language used between the drone and Ground Control Stations (GCS) for telemetry and mission planning.
* **[ExpressLRS](https://www.expresslrs.org/):** Open-source, ultra-low latency (up to 1000Hz). 2.4GHz for racing; 900MHz for long-range radio control link.

## 6. Specialized Sensors & Payloads
*   **GPS:** **u-blox M9N** tracks 4 constellations for faster "cold starts".
*   **RTK:** Essential for centimeter-level accuracy in surveying/agriculture.
*   **Lidar:** Used for **Terrain Following** (constant height over crops) and obstacle avoidance.

## 7. Build Workflow Example
Follow these steps in order to avoid component incompatibility.
1.  **Requirements:** Define functional requirements of the VTOL.
2.  **TWR Goal**: Calculate required thrust for given AUW.
3.  **Prop/Motor/ESC Selection:** Determine type and size of propulsion system for target TWR.
4.  **Verify:** Use [eCalc](https://www.ecalc.ch) to verify your calculations.

## Resources
*   [ArduPilot Documentation](https://ardupilot.org/ardupilot/)
*   [Betaflight Wiki)](https://betaflight.com/docs/wiki)
*   [eCalc Flight Simulator](https://www.ecalc.ch)
*   [Mavlink Docs](https://mavlink.io/en/)
*   [PX4 Docs](https://docs.px4.io/main/)