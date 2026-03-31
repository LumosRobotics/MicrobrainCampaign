# MicroBrain: A Modular Embedded Ecosystem — No Soldering Required

## A tiny, powerful core board and a growing family of plug-and-play shields for robotics, drones, and automation.

---

## The Idea

Most embedded projects start the same way: breadboard, rats nest of jumper wires, a soldering iron, and hours of debugging connections before a single line of firmware is written. We think that's the wrong place to spend your time. The rats nests and solder joints have always been a pet peeve of mine — they add complexity, reduce reliability, and make it harder to iterate on the actual application. Speed to prototype should be about the firmware and the system design, not the hardware assembly.

**MicroBrain** is a compact, capable microcontroller module paired with a family of compatible shields and peripheral modules. Snap them together, connect over CAN FD, and start building — no soldering required.

Whether you're building a robot, a sensor node, a motion control system, or a field instrument, MicroBrain gives you a clean, modular foundation that grows with your project.

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
- **JST connector 1** — CAN FD bus connection
- **JST connector 2** — two GPIO pins with I2C and UART support, for peripheral modules
- Designed for size: small enough to embed anywhere

The mezzanine connector is the heart of the shield ecosystem. It carries power and the full GPIO set, so shields can use any combination of UART, SPI, and I2C without additional wiring.

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

---

## The Toolchain: lumos

Hardware that's easy to connect should be equally easy to program. That's where `lumos` comes in.

`lumos` is a command line tool that bundles everything you need to go from new project to running firmware:

- **Project creation** — scaffold a new firmware project in seconds
- **Build** — compiles your code using a bundled ARM GCC toolchain, no manual compiler setup required
- **Flash** — program your MicroBrain directly from the command line

No hunting for the right toolchain version, no CMake configuration from scratch, no environment variables to wrangle. Clone, create, build, flash. If you prefer to set up your own toolchain or integrate with an existing build system, that is of course also a viable option - standard ARM GCC and CMake workflows are fully supported, and configuration files are included for manual builds.

If you prefer to use your own toolchain or integrate with an existing build system, `lumos` gets out of the way — standard ARM GCC and CMake workflows are fully supported.

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

### Why not just put the MCU on every sensor board?

The obvious alternative is to integrate the microcontroller and CAN transceiver directly onto each sensor board — one board for IMU sensing, one for distance measurement, one for motor control. It seems simpler at first glance. In practice, it creates a different set of problems.

**Cost adds up fast.** Every board in that approach carries its own STM32, its own CAN transceiver, its own decoupling network, crystal, and supporting passives. With the MicroBrain approach, you buy one MCU board and pair it with whichever shield you need. The shield itself is simpler and cheaper — it only has to house the sensor and its support circuitry.

**Firmware becomes fragmented.** When the MCU is baked into every sensor board, you end up maintaining separate firmware projects for each one. With MicroBrain, there is one BSP, one set of peripheral drivers, one build system. The MCU environment is identical across every node in your system, so code and libraries transfer directly.

**Reconfiguration requires new hardware.** If your application changes — say you need distance sensing where you previously needed an IMU — you replace the shield and update the firmware. With integrated boards, you replace the entire node, MCU included.

**Iteration on sensor design is harder.** Designing a PCB with an MCU on it is significantly more involved than designing a sensor breakout. Power planes, impedance-controlled traces for high-speed signals, debug interfaces, and boot circuitry all live on the MCU board. A shield only needs to get the sensor right. Separating the two means you can iterate on the sensor design without touching the MCU layout — and vice versa.

**Mixed configurations are possible.** A MicroBrain node can run a shield *and* have peripheral modules attached via the JST connector simultaneously. Need motor control and a barometer on the same node? That's one MicroBrain, one StepperMotorDriver Shield, and one Barometer Module. With integrated boards, combining capabilities on a single node means custom hardware.

The modular approach costs a little more board area per node. In exchange, you get flexibility, lower per-sensor cost, simpler firmware, and a system that can be reconfigured without a soldering iron or a new PCB spin.

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

## Stretch Goals

As the campaign grows, we plan to expand the ecosystem with additional shields and peripheral modules. Community input will shape what gets built next.

---

## About the Project

I'm a Swedish embedded systems engineer with a background in automotive — an industry that demands rigorous, reliable hardware and leaves little room for shortcuts. Over the years I've worked on the kinds of systems where correctness isn't optional, and that discipline has shaped how I think about hardware design.

In my spare time, the projects have always leaned toward robotics. Building things that move, sense, and react. And for just as long, I've run into the same frustration: the gap between having an idea and having a working prototype is filled with soldering, custom PCBs, and toolchain setup — not firmware, not system design, not the interesting parts.

To me, the friction that exists in bringing a system to life is not only lost time, the friction prevents some projects from ever happening. It also stifles creativity. I myself am a fairly lazy engineer — I want to spend my time building the fun parts, and focus on what I am really good at, not debugging a wiring issue or hunting for the right version of a toolchain.

MicroBrain is the system I always wanted but never had. A small, capable edge node I could drop into any robotic system, snap on the sensor or actuator I needed, and get straight to the firmware. No bespoke hardware for every project. No rewiring when requirements change. Just a clean, modular foundation that gets out of the way and lets you build.

This campaign is how I'm taking it from a personal tool to something the broader community can use.

---

*Questions? Reach out via the Crowd Supply messaging system, leave a comment below or email me in person at [your email address].*
