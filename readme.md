This repo contains an easy-to-follow guide and explanation for component selection for building VTOL (Vertical Take-Off and Landing) aircraft, for any application.

## Glossary

| Acronym | Term | Definition |
| :--- | :--- | :--- | 
| **AUW** | All-Up Weight | Total weight at any moment during the flight or ground operation |
| **TWR** | Thrust-to-Weight Ratio | Total max thrust divided by AUW |
| **KV** | Motor Velocity Constant | RPM a motor spins per 1 volt (no load) |
| **Sag** | Voltage Drop | Temporary decrease in battery voltage under heavy load. |
| **MCU** | Microcontroller Unit | The "brain" chip on the FC (e.g., STM32 F4, F7, H7) |
| **IMU** | Inertial Measurement Unit | Combined Gyroscope and Accelerometer for orientation. |
| **PDB** | Power Distribution Board | Manages high current flow from battery to multiple ESCs. |
| **mAh** | Milliampere-hour | Unit of electrical charge; indicates battery capacity. |
| **AWG** | American Wire Gauge | Standard for wire thickness; Lower number = Thicker wire. |
| **BEC** | Battery Eliminator Circuit | Regulates high battery voltage down to 5V/12V for electronics. |

## 1. Physics & Aerodynamics
Multirotor flight is governed by the interaction between total weight (mg) and vertical force (thrust).

**Fundamental Formulas**
* Newton’s Second Law: $F = ma$
* Hover Requirement: $\text{Thrust} \ge \text{Weight }$

Thrust equal to weight is practically useless for flight as it leaves no margin for control, wind resistance, or maneuvering.

**Disk Loading**: Large propellers moving air slowly are more efficient than small propellers moving air quickly. This is why endurance drones use the largest props their frame allows.

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

| Action | Effect 1 | Effect 2 | Result |
| :--- | :--- | :--- | :--- |
| **🔼 Propeller Diameter** | 🔼 Efficiency (at hover) | 🔻 Response Speed | Better for endurance; slower for racing. |
| **🔼 Propeller Pitch** | 🔼 Top Speed | 🔻 Motor Heat | Higher velocity but higher current draw. |
| **🔼 Stator Diameter** | 🔼 Torque | 🔻 Efficiency | Better for swinging large, heavy props. |
| **🔼 Stator Height** | 🔼 Power Handling | 🔼 Weight | Better heat dissipation for heavy loads. |

## 2. Propulsion System
The motor and Electronic Speed Controller (ESC) must be selected for thermal safety.

*   **Motor Size (e.g., 2207):** The first two digits are diameter (torque); the last two are height (power/heat dissipation).
*   **ESC Protocols:** The protocol determines how fast and accurately the FC communicates with the motors.
    *   **PWM / Oneshot125:** Analog legacy protocols; high latency.
    *   **DShot (300/600/1200):** Modern digital standard. DShot600 is the industry sweet spot for reliability and performance.
*   **ESC Safety:** Safety margin of **20-50 %** is recommended depending upon application.

| Action | Effect 1 | Effect 2 | Result |
| :--- | :--- | :--- | :--- |
| **🔼 Motor KV** | 🔼 RPM | 🔻 Torque | Prioritize speed over lifting capacity. |

> 🛡️ **Motor Heat:** Should never exceed 80°C (176°F) to avoid demagnetizing the bells.

> 🛡️ **ESC Cooling:** In high-current builds, mount ESCs on the arms to benefit from propeller wash (active airflow) or add dedicated heatsinks.

## 3. Energy Storage Systems
The battery is the heaviest component and dictates total flight time (endurance).

| Feature | LiPo (Lithium Polymer) | Li-ion (Lithium-Ion) |
| :--- | :--- | :--- | 
| Best For | Racing, Heavy Lifting (High "C" rating) | Long-range, Mapping (High density) |
| Pros | High burst power; less "sag" | Longer flight times (30–60 mins) |

**Voltage (S-Count)**: Higher voltage (e.g., 6S or 12S) is more efficient because it reduces current draw ($I$), leading to less heat ($I^2R$) and allowing thinner wires.

**Wiring Standards**: Higher current draws require thicker copper to prevent the wires from acting like a fuse and melting. Use silicone-insulated wire.

| Action | Effect 1 | Effect 2 | Result |
| :--- | :--- | :--- | :--- |
| **🔼 Battery Voltage (S)** | 🔼 Efficiency | 🔻 Current ($I$) | Reduces heat in wires ($I^2R$ losses). |
| **🔼 mAh** | 🔼 Flight Time | 🔼 Weight | Increases Flight Time. |
| **🔼 Current (Amps)** | 🔻 AWG Number | 🔻 Wire Thickness | Higher current draws require thicker copper. |

> 🛡️ Use Anti-Spark connectors (XT90-S/AS150) to protect FC/ESC capacitors from voltage spikes.

> 🛡️ **Safety Margin:** The 0.8 (80%) rule ensures you land before the battery voltage "sags" to a dangerous level, preventing permanent cell damage.

## 4. Flight Controller (FC) & Frame
The FC is the "brain" of multirotor, processing sensor data via PID (Proportional-Integral-Derivative) control loop to stabilize the craft. It uses Sensor Fusion to combine data from the IMU (Gyro/Accel), Magnetometer (Compass), and Barometer (Altitude) into a single orientation estimate.

* **MCU Selection**
    * **F4:** Legacy Budget workhorse.
    * **F7:** Highly popular; universal signal inversion on all UART ports.
    * **H7:** State-of-the-art; required for ArduPilot/PX4 and complex path planning.
* **Build vs Buy**
    * **DIY:** You can build a custom FC by integrating a standalone MCU (Teensy/STM32) with an external IMU and Magnetometer via I2C or SPI buses. This is common in research and prototyping (see [dRehmFlight](https://github.com/nickrehm/dRehmFlight)).
    * **Off-the-Shelf:** All-in-one boards that include the MCU, sensors, and often a PDB or OSD chip. Best for rapid deployment and standardized reliability.
* **AutoPilot** - Firmware required to be installed on FC
    * **[Ardupilot](https://ardupilot.org/):** Best for Industrial/GPS Missions. License: GPLv3.
    * **[PX4](https://px4.io/):** Best for Research & Enterprise. Professional-grade codebase, modular, and optimized for H7. License: BSD 3-Clause (Permissive).
    * **[Betaflight](https://betaflight.com/):** Best for FPV Racing & Freestyle. Prioritizes flight feel and low latency over GPS autonomy. License: GPLv3.
    * **[dRehmFlight](https://github.com/nickrehm/dRehmFlight):** Best for Hobbyist Prototyping. A simplified C++ code for Teensy MCUs; great for learning raw flight control logic.       
* **Vibration:** Use silicone "gummies" to **soft-mount** the FC. This reduces high-frequency motor noise that confuses the sensors.
* **Frame Material**: Carbon Fiber is preferred for its anisotropic properties (strength can be tuned by layer direction). Avoid frame flex, which causes "asynchronous vibration" and motor overheating.

## 5. Radio Link & Communication
* **[MAVLink](https://mavlink.io/):** Open-source, standard communication language used between the drone and Ground Control Stations (GCS) for telemetry and mission planning.
* **[ExpressLRS](https://www.expresslrs.org/):** Open-source, ultra-low latency (up to 1000Hz). 2.4GHz for racing; 900MHz for long-range radio control link.

> 🛡️ Set a Failsafe to "Drop" or "RTH" (Return to Launch) so the drone doesn't fly away if the signal is lost.

> 🛡️ **Frequency Hopping:** Modern protocols like ELRS use Spread Spectrum technology to prevent "hijacking" or signal jamming in noisy environments.

> 🛡️ **Unique ID/Binding:** Ensure every craft has a unique Binding Phrase so other pilots cannot accidentally control your drone.

> 🛡️ **Telemetry Signing:** MAVLink can sign messages to verify the sender's identity and prevent unauthorized data injection (spoofing).

> 🛡️ **Encryption:** MAVLink is not designed for encryption; it does not hide your data from observers. If a build requires communication to be hidden (private), you must ensure the data is encrypted in transit via a secure telemetry link or specialized hardware so third parties cannot read your mission data.

## 6. Ground Control Stations
The software bridge between the pilot and the machine.
| Software | Best For |
| :--- | :--- | 
| **Mission Planner** | Advanced ArduPilot tuning | 
| **QGroundControl** | Modern UI for PX4/ArduPilot | 
| **Betaflight Configurator** | PID tuning for racing quads | 

## 7. Specialized Sensors & Payloads
| Application | Required Sensors | Purpose |
| :--- | :--- | :--- | 
| **FPV Racing** | VTX, Camera, FPV Goggles | Pilot immersion & control |
| **Navigation** | GNSS, IMU, Telemetry | Waypoint missions & RTH |
| **Mapping** | Lidar, High-Res Camera | 3D Modeling & Terrain Following |
| **Agriculture** | RTK, Lidar, Flow Meter | Cm-level precision & obstacle avoidance |

## 7. Build Workflow
Follow these steps in order to avoid component incompatibility.
1.  **Requirements:** Define functional requirements of the VTOL.
2.  **Estimate AUW:**: Calculate the sum of payload, frame, and electronics.
3.  **Determine TWR:** Choose a target based on the application (e.g., 2:1 for Basic Quad).
4.  **Select Propeller:** Choose the largest prop the frame can physically hold for maximum efficiency.
5.  **Match Motor KV:** Use low KV for high-voltage (6S/12S) efficiency or high KV for low-voltage (4S) speed.
4.  **ESC & Wire:** Ensure they can handle atleast 120% of max motor draw.
4.  **Simulate:** Verify your selected parts using [eCalc](https://www.ecalc.ch).

## Resources
*   [ArduPilot Documentation](https://ardupilot.org/ardupilot/)
*   [Betaflight Wiki)](https://betaflight.com/docs/wiki)
*   [eCalc Flight Simulator](https://www.ecalc.ch)
*   [Mavlink Docs](https://mavlink.io/en/)
*   [PX4 Docs](https://docs.px4.io/main/)
*   [dRehmFlight](https://github.com/nickrehm/dRehmFlight)