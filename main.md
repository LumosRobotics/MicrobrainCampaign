# MicroBrain: A Modular Embedded Ecosystem — No Soldering Required

## A tiny, powerful core board and a growing family of plug-and-play shields for robotics, drones, and automation.

---

## The Idea

Most embedded projects start the same way: breadboard, rats nest of jumper wires, a soldering iron, and hours of debugging connections before a single line of firmware is written. We think that's the wrong place to spend your time. The rats nests and solder joints have always been a pet peeve of mine — they add complexity, reduce reliability, and make it harder to iterate on the actual application. Speed to prototype should be about the firmware and the system design, not the hardware assembly.

**MicroBrain** is a compact, capable microcontroller module paired with a family of compatible shields and peripheral modules. Snap them together, connect over CAN FD, and start building — no soldering required.

Whether you're building a robot, a sensor node, a motion control system, or a field instrument, MicroBrain gives you a clean, modular foundation that grows with your project.

---

## Why Not Just Use an Arduino?

Fair question. The embedded world does not need another general-purpose development board with a USB port and an LED. MicroBrain is not that.

Arduino and its clones are designed to sit on a desk during development. They are optimised for ease of programming, broad library support, and experimentation. That is genuinely useful — but it is a different problem than the one MicroBrain solves.

MicroBrain is designed to be **deployed inside a system** and stay there. The differences that follow from that are not cosmetic:

**The communication bus is different.** Arduino boards talk over USB, WiFi, or Bluetooth — protocols that are fine for a bench setup but poorly suited to the inside of a robot or a machine. MicroBrain uses CAN FD: a differential, noise-tolerant, multi-node bus that was built for exactly the environments where motors are spinning, cables are flexing, and electrical interference is a fact of life. It is the same bus that runs inside every modern car.

**The form factor is different.** Most Arduino-family boards are sized for a breadboard, not a robot frame. MicroBrain is sized to disappear into a system — 17.2 × 17.2 mm, with mounting holes and JST connectors that route cleanly through a cable chain.

**The MCU is different.** The STM32G0 is a production-grade microcontroller used in commercial and industrial products. It is not a hobbyist chip dressed up as one. The toolchain that supports it — real GCC, real CMake, real debuggers — is the same toolchain used in professional embedded development.

**The shield connector is different.** FeatherWings, Arduino shields, and most expansion boards connect via through-hole headers that must be soldered. MicroBrain's mezzanine connector mates directly with no soldering, no alignment fuss, and no solder joints to work loose under vibration.

**The intent is different.** Arduino asks: *how do I make embedded programming accessible?* MicroBrain asks: *how do I make a deployable, distributed embedded system fast to build and easy to reconfigure?* Those are not the same question, and they lead to very different hardware.

If you want to learn to program microcontrollers, Arduino is a great choice. If you want to build a robot, a drone, or a machine with multiple smart nodes that communicate reliably and fit where you need them — that is what MicroBrain is for.

---

## The MicroBrain Module

The core of the ecosystem. The MicroBrain is a tiny PCB built around the **STM32G0 microcontroller** paired with a **CAN FD transceiver**, designed to be the edge brain of any distributed system.

MicroBrain is meant to live *on the edge* — tucked into a corner of a robot chassis, strapped to a drone arm, or mounted inside a machine enclosure. Each node sits close to its sensors and actuators, handles local processing, and reports back over CAN FD. The result is a clean, distributed architecture: instead of one central board drowning in wires, you place small intelligent nodes exactly where they're needed.

Size was a first-class design constraint throughout the ecosystem. The MicroBrain and its shields are compact enough to fit into spaces that would rule out most development boards — inside a joint of a robot arm, along the frame rail of a small drone, or recessed into a housing with millimetres to spare. Mounting holes are included on every board. The CAN FD and peripheral connections are on JST connectors, so cables can be routed cleanly without bulk at the board edge. The goal is a module that disappears into your system rather than dictating its shape.

**Key features:**
- STM32G0 microcontroller — efficient, capable, well-supported
- CAN FD transceiver for robust, high-speed multi-node communication
- **Board-to-board mezzanine connector** for attaching compatible shields directly underneath
  - 5V power rail
  - 12 GPIOs including UART, SPI, and I2C
- **JST connector 1** — CAN FD bus connection, also carries 5V power input
- **JST connector 2** — two GPIO pins with I2C and UART support for peripheral modules, also carries 5V power input
- Designed for size: small enough to embed anywhere

The mezzanine connector is the heart of the shield ecosystem. It carries power and the full GPIO set, so shields can use any combination of UART, SPI, and I2C without additional wiring.

**Power** is supplied over 5V on either JST connector — whichever is most convenient for your wiring. In a CAN FD network, power typically comes in on the CAN connector alongside the bus signals, so a node is fully powered and connected with a single cable. Alternatively, if a node is running a peripheral module without being part of a CAN network, it can be powered from the peripheral JST connector instead. Either way, no dedicated power connector is needed.

---

## What Is CAN FD?

CAN (Controller Area Network) is a communication bus originally developed for automotive applications in the 1980s. It was designed to let many microcontrollers and devices talk to each other over a simple two-wire bus without a central host — and to do so reliably in electrically noisy environments like cars and industrial machinery. Decades later, it remains the backbone of nearly every modern vehicle and a large portion of industrial and robotics systems, largely because it is robust in ways that protocols like UART, SPI, and I2C simply are not.

CAN FD (Flexible Data-rate) is the modern evolution of the standard. It lifts two of the original CAN limitations: the data payload per message was expanded from 8 bytes to up to 64 bytes, and the transmission speed can step up to 8 Mbps during the data phase of each frame. For practical purposes this means faster, denser communication between nodes while retaining everything that made classic CAN dependable — differential signalling for noise immunity, hardware-level error detection, automatic retransmission, and the ability to add or remove nodes from the bus without disrupting the rest of the network.

For a distributed robotics or drone system, these properties matter. A CAN FD bus can run across the length of a robot frame or through a cable chain without signal integrity issues that would plague I2C or UART at the same distances. Multiple MicroBrain nodes share the same two-wire bus — each with its own address, each independently powered — and the system keeps working even if one node loses power or is disconnected. This is the same fundamental architecture used in every modern car, and it translates directly to robust distributed embedded systems outside of automotive.

---

## The Shield Ecosystem

Shields plug directly onto the MicroBrain via the mezzanine connector. No soldering, no wiring, no guessing which pin goes where.

### StepperMotorDriver Shield

Drive smaller stepper motors directly from the MicroBrain. Perfect for precise positioning in robotics, camera sliders, lab automation, or any application that needs controlled linear or rotational motion.

- Compact stepper motor driver matched to the MicroBrain footprint
- Direct mezzanine connection — no external wiring required
- Ideal for NEMA 14 and similar small stepper motors

---

### ToF (Time-of-Flight) Shield

Add long-range distance sensing to your project with a **ST Time-of-Flight sensor** capable of measuring distances up to **6 meters** with an **8×8 sensing grid**.

- Up to 6m range
- 8×8 zone grid for spatial awareness and simple depth mapping
- Connected over SPI via the mezzanine connector
- Suitable for obstacle detection, presence sensing, and gesture recognition

---

### IMU Shield

Six degrees of freedom in a shield. The IMU Shield carries a **6-DoF IMU** (3-axis accelerometer + 3-axis gyroscope) for motion tracking, orientation sensing, and vibration analysis.

- 6-DoF IMU: accelerometer + gyroscope
- Connected over SPI via the mezzanine connector
- Useful for robotics, stabilisation, activity detection, and data logging

---

## The Peripheral Module Ecosystem

Peripheral modules connect via the **JST I2C connector** on the MicroBrain — a separate port from the mezzanine connector used by shields. This means a shield and a peripheral module can be used at the same time on the same MicroBrain node, without any conflict. A MicroBrain running a StepperMotorDriver Shield can simultaneously have a barometer attached for altitude feedback. A node with an IMU Shield can drive a small OLED display for local status output. The two ecosystems complement each other rather than compete for the same port.

### OLED Screen Module

A compact module designed for **1.54" OLED displays**. Add a local display to your project for status readouts, sensor data, or simple UI — all over I2C.

- Compatible with 1.54" OLED screens
- I2C connection via JST cable to the MicroBrain
- No soldering, no breakout boards

---

### Barometer Module

Add atmospheric pressure sensing to any MicroBrain setup with a simple cable connection.

- Barometric pressure sensor
- I2C connection via JST cable
- Useful for altitude sensing, weather monitoring, and environmental logging

---

## Place Intelligence Anywhere

One of the most powerful things about the MicroBrain's small footprint is where it lets you put compute and sensing. In larger systems — a multi-joint robot arm, a drone, an autonomous vehicle, a machine tool — the challenge is often not capability but placement. You know what you want to measure or control, but getting signals back to a central board means long cable runs, noise pickup, and connectors that work loose.

With MicroBrain, the intelligence goes to the data. A MicroBrain with a ToF Shield mounted at the tip of a robot arm gives you distance sensing exactly where the arm reaches, without routing fragile sensor signals across the full cable chain. A MicroBrain with an IMU Shield strapped to a drone frame captures motion data right at the point of interest. A barometer module in the nose of a UAV gives you altitude data with a short JST cable to a MicroBrain mounted nearby — no long wire runs, no signal integrity issues.

Each node processes its own sensor data locally and reports over CAN FD, a bus designed for exactly this kind of distributed, noise-tolerant communication. The central controller receives clean, processed data rather than raw signals from sensors several metres away. The result is a system that is easier to build, easier to debug, and more reliable in operation.

The form factor makes this practical rather than theoretical. A MicroBrain node with a shield is small enough to tuck into spaces that would fit nothing else, and the JST connectors mean it can be installed and removed without tools.

### A Worked Example: Three Nodes on One Bus

To make this concrete, consider a small robot with three MicroBrain nodes on a shared CAN FD bus running at **1 Mbps**.

**Node 1 — ToF Shield** (obstacle detection, 15 Hz)
The 8×8 distance grid produces 64 measurements at 16 bits each — 128 bytes per reading. CAN FD's maximum payload is 64 bytes, so each ToF update is split across **2 frames**. At 15 Hz that is **30 frames/second** from this node.

**Node 2 — IMU Shield** (motion tracking, 200 Hz)
Three axes of accelerometer and three of gyroscope, each 16-bit, gives 12 bytes of sensor data. Add a 4-byte sequence counter and the whole update fits comfortably in **1 frame of 16 bytes**. At 200 Hz that is **200 frames/second**.

**Node 3 — StepperMotorDriver Shield** (position control, 100 Hz)
Each control cycle produces one outbound command frame (target position + speed, 8 bytes) and one inbound status frame (current position + flags, 8 bytes) — **2 frames per cycle**, or **200 frames/second** at 100 Hz.

| Node | Payload | Frames/update | Update rate | Frames/sec |
|------|---------|--------------|-------------|------------|
| ToF | 128 bytes (2 × 64 B) | 2 | 15 Hz | 30 |
| IMU | 16 bytes (1 × 16 B) | 1 | 200 Hz | 200 |
| Stepper cmd + status | 8 + 8 bytes (2 × 8 B) | 2 | 100 Hz | 200 |
| **Total** | | | | **430 frames/sec** |

At 1 Mbps, each bit takes 1 µs. A CAN FD frame is not just the raw data — it includes an identifier, control fields, CRC, acknowledgement, and end-of-frame sequence, plus bit stuffing overhead. Accounting for all of this:

| Frame size | Approximate bus time |
|------------|----------------------|
| 64 bytes | ~700 µs |
| 16 bytes | ~220 µs |
| 8 bytes | ~140 µs |

Multiplying through:

| Node | Frames/sec | Frame size | Bus time/sec | Utilisation |
|------|------------|------------|-------------|------------|
| ToF | 30 | 64 B | 21,000 µs | 2.1% |
| IMU | 200 | 16 B | 44,000 µs | 4.4% |
| Stepper | 200 | 8 B | 28,000 µs | 2.8% |
| **Total** | **430** | | **93,000 µs** | **~9.3%** |

Three active sensor and actuator nodes consume roughly **one tenth of the available bus bandwidth**. The remaining ~90% is headroom for more nodes, faster update rates, a central controller sending commands back down the bus, or diagnostic and logging traffic — all without touching the bus configuration.

### A Second Example: 20 Stepper Motor Nodes

Consider a larger machine — a multi-axis system, a robotic gantry, or a modular conveyor — with **20 MicroBrain nodes** each running a StepperMotorDriver Shield. Every node exchanges one 8-byte command frame and one 8-byte status frame per control cycle: **2 frames per node per cycle**.

At **100 Hz** across all 20 nodes:

| | Value |
|---|---|
| Nodes | 20 |
| Frames per node per cycle | 2 (cmd + status) |
| Total frames per cycle | 40 |
| Update rate | 100 Hz |
| Total frames/sec | 4,000 |
| Bus time per 8-byte frame | ~140 µs |
| Total bus time/sec | 560,000 µs |
| **Bus utilisation** | **56%** |

56% utilisation — over half the bus consumed, but the system is still comfortably within headroom. The remaining 44% is available for supervisory traffic, fault reporting, or additional sensor nodes on the same bus.

**What is the maximum achievable update rate?**

At 1 Mbps there are 1,000,000 µs of bus time available per second. Each full cycle across all 20 nodes costs 40 × 140 µs = 5,600 µs. At 100% theoretical utilisation that allows 1,000,000 / 5,600 ≈ **178 cycles/sec**. Leaving a practical headroom margin of ~20%, a sustainable maximum sits around **~140 Hz** for all 20 nodes simultaneously — still well above the 100 Hz baseline.

For comparison, a classic CAN 2.0 bus at 500 kbps with 8-byte frames carries roughly half the throughput of this configuration and lacks the larger payload options that make sensor data efficient to transmit. CAN FD at 1 Mbps handles a 20-node motion control system with room to spare.

---

## The Toolchain: lumos

Hardware that's easy to connect should be equally easy to program. That's where `lumos` comes in.

`lumos` is a command line tool that bundles everything you need to go from new project to running firmware:

- **Project creation** — scaffold a new firmware project in seconds
- **Build** — compiles your code using a bundled ARM GCC toolchain, no manual compiler setup required
- **Flash** — program your MicroBrain directly from the command line

No hunting for the right toolchain version, no CMake configuration from scratch, no environment variables to wrangle. Clone, create, build, flash. If you prefer to set up your own toolchain or integrate with an existing build system, standard ARM GCC and CMake workflows are fully supported — configuration files are included for manual builds.

The lumos tool can already be downloaded [here](www.todo.com) (TODO: Add real link) and is in active development. It is available for Windows, macOS, and Linux.

### The FlashDongle

Flashing the MicroBrain requires one piece of hardware: the **FlashDongle**. To save onboard space, I decided to offload the flashing hardware to an external dongle rather than integrating it onto the MicroBrain itself. 
It's a small USB-C dongle with a cable that connects directly to the MicroBrain's flashing connector. Plug it into your computer, run `lumos flash`, and your firmware is on the board.

No dedicated programmer, no SWD debugger, no proprietary flashing hardware. Just a dongle that lives on your keychain or in your laptop bag, ready whenever you need it. The FlashDongle is the only external tool required to go from compiled firmware to a running MicroBrain node. For those that already have USB to UART bridges laying around, the flashing connector is a standard pinout that can be adapted to other hardware as well.

---

## Why Modular?

Embedded hardware projects often accumulate complexity faster than value. You end up with a different PCB for every project, stacks of components that don't talk to each other, and wiring that takes longer to debug than the firmware.

The MicroBrain ecosystem is designed around a different philosophy:

- **One core board** that handles compute and communication
- **Shields for capability** — add exactly what you need, nothing more
- **Peripheral modules for I2C devices** — displays, sensors, and more on a simple cable
- **CAN FD for multi-node systems** — connect multiple MicroBrain nodes over a robust, noise-tolerant bus
- **No soldering required at any step**

Every module in the ecosystem is physically and electrically compatible. Mix and match as your application requires. Reuse modules across projects. Start small, scale up.

### One of each, done well

The ecosystem is deliberately curated. There is one IMU shield, not four variants with marginally different specs and a comparison table to wade through. One ToF shield. One barometer module. Each one chosen to cover the use case well, documented clearly, with known driver support and known behaviour in lumos firmware.

This is a conscious trade-off. A platform that offers six IMUs gives you coverage but also gives you a decision to make before you have written a line of code — and if you pick the wrong one, you are on your own. MicroBrain makes that decision for you. The IMU shield does what an IMU should do. The ToF shield does what a ToF sensor should do. You know what you are getting, you know it works with the rest of the ecosystem, and you can focus on building your system rather than evaluating components.

When a genuinely better part becomes available, or when a specific use case demands a different option, the ecosystem will expand — but intentionally, not by accumulation. Breadth for its own sake is complexity in disguise.

### Why not just put the MCU on every sensor board?

The obvious alternative is to integrate the microcontroller and CAN transceiver directly onto each sensor board — one board for IMU sensing, one for distance measurement, one for motor control. It seems simpler at first glance. In practice, it creates a different set of problems.

**Cost adds up fast.** Every board in that approach carries its own STM32, its own CAN transceiver, its own decoupling network, crystal, and supporting passives. With the MicroBrain approach, you buy one MCU board and pair it with whichever shield you need. The shield itself is simpler and cheaper — it only has to house the sensor and its support circuitry.

**Firmware becomes fragmented.** When the MCU is baked into every sensor board, you end up maintaining separate firmware projects for each one. With MicroBrain, there is one BSP, one set of peripheral drivers, one build system. The MCU environment is identical across every node in your system, so code and libraries transfer directly.

**Reconfiguration requires new hardware.** If your application changes — say you need distance sensing where you previously needed an IMU — you replace the shield and update the firmware. With integrated boards, you replace the entire node, MCU included.

**Iteration on sensor design is harder.** Designing a PCB with an MCU on it is significantly more involved than designing a sensor breakout. Power planes, impedance-controlled traces for high-speed signals, debug interfaces, and boot circuitry all live on the MCU board. A shield only needs to get the sensor right. Separating the two means you can iterate on the sensor design without touching the MCU layout — and vice versa.

**Mixed configurations are possible.** A MicroBrain node can run a shield *and* have peripheral modules attached via the JST connector simultaneously. Need motor control and a barometer on the same node? That's one MicroBrain, one StepperMotorDriver Shield, and one Barometer Module. With integrated boards, combining capabilities on a single node means custom hardware.

The modular approach costs a little more board area per node. In exchange, you get flexibility, lower per-sensor cost, simpler firmware, and a system that can be reconfigured without a soldering iron or a new PCB spin.

---

## How Does It Compare?

Several modular embedded ecosystems exist. Here is how MicroBrain stacks up against the most relevant ones.

| | **MicroBrain** | Adafruit Feather M4 CAN | SparkFun MicroMod | Seeed XIAO + Grove | MIKROE mikroBUS + Click |
|---|---|---|---|---|---|
| **MCU** | STM32G0 | ATSAME51 Cortex-M4 | Swappable (RP2040, STM32, ESP32…) | RP2040 / ESP32 / nRF52840 | Host-dependent (STM32, PIC, AVR…) |
| **CAN FD** | Yes, native | CAN 2.0 only | No active CAN FD board | No | Yes, via Click add-on board |
| **Shield/expansion connector** | Board-to-board mezzanine | Soldered stacking headers | M.2 board-to-board | Grove 4-pin JST cables | 2×8 pin headers |
| **Peripheral modules** | JST connector (I2C/UART) | No dedicated port | No dedicated port | Grove cables | No dedicated port |
| **Shield + peripheral simultaneously** | Yes | No | No | Limited | No |
| **Solderless out of the box** | Yes | No (headers must be soldered) | Yes (M.2 screw mount) | Yes (since late 2024) | Yes (Click boards press on) |
| **Form factor** | 17.2 × 17.2 mm | 52 × 23 mm | 22 × 22 mm module + carrier | 21 × 18 mm + expansion board | 25 × 29 mm Click + host board |
| **Designed for edge deployment** | Yes | No | No | No | No |
| **Integrated toolchain** | Yes (lumos) | Arduino IDE / CircuitPython | Arduino IDE | Arduino IDE / MicroPython | MIKROE SDK / third-party |
| **Primary target** | Robotics, drones, edge nodes | Maker / portable IoT | Prototyping / education | Wearables / compact IoT | Industrial / rapid prototyping |

A few notes on the comparison:

**CAN FD** is a meaningful dividing line. CAN FD is the standard communication bus in modern automotive and robotics systems — it offers higher bandwidth and more reliable framing than classic CAN 2.0. Among the boards listed, only MIKROE offers CAN FD, and only via an additional Click board rather than natively. MicroBrain includes the CAN FD transceiver on the core module itself.

**Mezzanine connectors** are not the same as soldered pin headers. The Feather ecosystem stacks Wings on top of the host board via through-hole headers that must be soldered during assembly. MicroBrain's board-to-board mezzanine connector requires no soldering — shields mate directly and can be swapped without tools.

**Edge deployment** is a design intent, not just a form factor claim. MicroBrain is sized and connected to live inside a system permanently, not on a desk during prototyping. The JST connectors, mounting holes, and compact footprint are deliberate choices toward that goal.

---

## Who Is This For?

- Robotics builders who want reliable motion control and sensing without bespoke PCBs
- Engineers prototyping embedded systems who want to move fast
- Makers and researchers who need sensor nodes that are easy to assemble and reconfigure
- Students learning embedded systems without the barrier of soldering

---

## What's in the Campaign

| Item | Description |
|------|-------------|
| MicroBrain | Core STM32G0 + CAN FD module |
| FlashDongle | USB-C UART flashing dongle |
| StepperMotorDriver Shield | Stepper motor driver shield |
| ToF Shield | 6m range, 8×8 grid time-of-flight shield |
| IMU Shield | 6-DoF accelerometer + gyroscope shield |
| OLED Screen Module | 1.54" OLED display peripheral module |
| Barometer Module | I2C barometric pressure peripheral module |

Modules will also be available individually and as starter bundles.

---

## Current Status

The MicroBrain ecosystem is currently in **active development**. We are running this campaign to fund final hardware bring-up, testing, and initial production.

Backers will receive production-quality boards, not prototypes.

---

## What Comes Next

The modules launching with this campaign are the foundation, not the ceiling. The connector standard, the JST pinout, and the lumos toolchain are all designed with expansion in mind. Here is what is already on the roadmap.

### Positioning & Location

**GPS Module** — A peripheral module with a compact GPS receiver for outdoor positioning. Useful for drones, field robots, and any system that needs to know where it is. Connects over UART via the JST connector.

**UWB (Ultra-Wideband) Module** — UWB enables centimetre-accurate relative positioning between nodes, indoors and outdoors alike. A UWB peripheral module would let multiple MicroBrain-equipped devices locate each other precisely — useful for swarm robotics, indoor navigation, and proximity sensing where GPS falls short.

### Display & Visual Output

**Additional OLED sizes and types** — Beyond the 1.54" module launching with the campaign, more screen sizes and panel types are planned to cover a wider range of enclosure constraints and readability requirements.

**LED Driver Shield** — Controllable RGB LEDs or LED arrays for status indication, lighting effects, or signal output. Useful in any system where visual feedback matters — from status indicators on a robot to addressable lighting on a custom installation.

### Human Input

**Button Module** — A simple, clean HID peripheral module for tactile input. Mount it wherever the user needs to interact with the system, connected back to the MicroBrain over the JST cable.

**Encoder Module** — Rotary encoder input for menu navigation, parameter adjustment, or any application that needs relative rotational input. Pairs naturally with a display module for a compact local UI.

**Joystick Module** — Analog joystick input for manual control of robots, gimbals, or any actuated system. Connects over the JST peripheral connector alongside whatever shield is already running.

### The Bigger Picture

Every module added to the ecosystem uses the same connectors, the same power rail, and the same lumos toolchain. A GPS module brought up on one project carries directly to the next. A joystick module developed for a robot controller works without modification on a drone ground station. The investment in learning the ecosystem compounds across every project you build with it.

Community feedback will shape the priority and order of what gets built. If there is a module or shield you need that is not listed here, let us know.

---

## About the Project

I'm a Swedish embedded systems engineer with a background in automotive — an industry that demands rigorous, reliable hardware and leaves little room for shortcuts. Over the years I've worked on the kinds of systems where correctness isn't optional, and that discipline has shaped how I think about hardware design.

In my spare time, the projects have always leaned toward robotics. Building things that move, sense, and react. And for just as long, I've run into the same frustration: the gap between having an idea and having a working prototype is filled with soldering, custom PCBs, and toolchain setup — not firmware, not system design, not the interesting parts.

To me, the friction that exists in bringing a system to life is not only lost time, the friction prevents some projects from ever happening. It also stifles creativity. I myself am a fairly lazy engineer — I want to spend my time building the fun parts, and focus on what I am really good at, not debugging a wiring issue or hunting for the right version of a toolchain.

MicroBrain is the system I always wanted but never had. A small, capable edge node I could drop into any robotic system, snap on the sensor or actuator I needed, and get straight to the firmware. No bespoke hardware for every project. No rewiring when requirements change. Just a clean, modular foundation that gets out of the way and lets you build.

This campaign is how I'm taking it from a personal tool to something the broader community can use.

---

*Questions? Reach out via the Crowd Supply messaging system, leave a comment below or email me in person at [your email address].*
