# Smart Modular Workstation for Disaster Relief

A submission to the VYOMA Designathon, Problem Statement 4, built by a four-person team from the Department of Aerospace Engineering at RV College of Engineering.

**Team:** Akula Uday Kiran, Kiran V Airani, Shanthosh K V, Tejas L

## What this project is

This is a portable, foldable, modular workstation designed to be air-dropped into disaster zones such as floods and earthquakes, where communication infrastructure has failed and local resources are severely strained. The idea is simple to state and hard to execute well: get a self-contained command post to the ground fast, have it survive the drop, and have it immediately useful for the people running the rescue.

The workstation is delivered by helicopter or large drone, descends under parachute, cushions its landing with automotive-style airbags and CO2 canisters, and then activates on touchdown. Once it is down, it deploys solar panels, establishes satellite and ham radio communication, broadcasts GPS coordinates, and becomes a mobile command center that relief teams can use to coordinate search, medical response, and drone-based aerial assessment.

We split the system into four functional modules, each self-contained enough to be reasoned about independently but designed to work together as one assembly once deployed.

## Module breakdown

### Module 1: Power and Communication Unit

This is the base module, and we deliberately put it at the bottom of the stack so that the heaviest single component (the battery) keeps the whole assembly's center of gravity low. A low center of gravity matters a lot here since this thing is going to hit the ground at speed and then potentially sit on uneven, waterlogged, or rubble-strewn terrain.

- **Battery system**: A Lithium-Polymer pack chosen for energy density and its flexible form factor, estimated at 30.71 kg with 3,685 to 4,914 Wh of capacity depending on cell energy density (150 to 200 Wh/kg). At an average draw of 100 W across the Raspberry Pi, IoT dashboard, and communication systems, that gives an endurance of roughly 36.85 to 49.14 hours, call it a day and a half to two days on battery alone before the solar array or manual generator needs to pick up the slack. The battery is protected by the descent system's airbags during a 20 m/s landing, so we are not expecting meaningful capacity loss from the impact itself.
- **IoT-enabled Raspberry Pi 5**: Handles real-time monitoring of power status, GPS location, and occupancy data through a dashboard interface. The Pi itself weighs 50 grams, 74 grams with its casing.
- **Antenna and ham radio system**: PET plastic housing with copper wiring, 15 grams for the antenna and roughly 730 grams for the ham radio housing. This is the long-range communication backbone, which matters most when cell towers and normal infrastructure are down.
- **I/O ports**: Four USB interfaces exposed through the Pi 5 for connecting peripherals or charging field equipment.
- **Manual generator**: A hand-crank backup, roughly 13.3 kg, made primarily of PET plastic, so that power generation does not depend entirely on sunlight or the primary battery.
- **Satellite connectivity**: Provides GPS and intermittent satellite communication for tracking and navigation even when terrestrial networks are gone.

### Module 2: Medical and Sustenance Unit

This module covers the immediate human needs once the workstation is on the ground.

- **Medical supplies**: First-aid and emergency response materials, stocked in an organized internal compartment layout.
- **Dehydrated food packs**: Long-shelf-life rations in compact PET compartments, roughly 441 grams for the compartment itself.
- **Water purification system**: A filter unit built from PET plastic, 2,756 grams dry weight, designed to take contaminated water and make it potable.

### Module 3: Survival Kit

This module is aimed at field survival and search capability.

- **Essential tools**: Utility knife, rope (neoprene, 70 grams), fire starters, hand warmers, glow sticks, torches, a flare gun (assumed titanium alloy construction, 540 grams), life jackets, and protective masks.
- **Thermal imaging**: An infrared heat-sensing camera module, roughly 300 grams, intended to help locate survivors who are trapped or otherwise hidden from direct view, under rubble or foliage for instance.

### Module 4: Deployment Mechanism

This is the module that has to work correctly on the first try, since there is no second attempt at a safe landing.

- **Descent system**: Automotive airbag mechanisms, a parachute, CO2 canisters, and inflatable cushions work together to bring the workstation down safely. The Raspberry Pi, using onboard GPS and IMU sensors, provides real-time navigation data during descent to guide the workstation toward the intended drop zone. The parachute reduces terminal velocity to approximately 18 m/s, and the airbag and CO2 canister system manages the final landing phase, rated to protect the payload at impact speeds up to 20 m/s. After the mission, the system is designed to be disassembled manually and retrieved by ground vehicle or aircraft, which is one of the reasons we kept the modular architecture rather than building a monolithic sealed unit.
- **Solar panel array**: Foldable and lightweight, deployed after landing to provide sustainable power for extended operation beyond the initial battery endurance window.

## Key design considerations

- **Modular architecture** for ease of transport, maintenance, and reconfiguration in the field.
- **Structural stability** through a deliberately low center of gravity, achieved mainly by placing the battery at the base.
- **Hybrid power system** combining solar, battery, and manual hand-crank generation so that no single point of failure takes out the power supply.
- **Real-time telemetry** through the IoT dashboard for monitoring operational parameters remotely.
- **Robust engineering**, validated through finite element simulation of impact scenarios rather than just asserted.
- **Operating environment**: rated for a temperature range of -10°C to 40°C, and built to withstand high humidity, potential water submersion, dust, and debris. PET plastic and aluminum were chosen specifically for corrosion resistance and durability given that this thing may sit in a flood zone or a dusty collapsed structure for days.

## Computational analysis

We did not want to just claim the descent and impact numbers worked, so we ran the actual calculations and simulations behind them.

**Terminal velocity without parachute.** Using the standard terminal velocity relation with an estimated total mass of 75 kg, air density of 1.225 kg/m³, a cross-sectional area of 0.217 m² without the parachute, and a drag coefficient of 0.6, the free-fall terminal velocity comes out to approximately 30.67 m/s. That is far too fast for a safe landing on its own, which is exactly why the parachute stage exists.

**Terminal velocity with parachute deployed.** With the parachute open, using an effective drag coefficient of 1.75 and an effective parachute area of 1.8237 m², the terminal velocity drops to approximately 6.19 m/s. This is close to the 6 m/s figure that came out of our CFD simulation of the descent phase, which we take as reasonable cross-validation between the analytical hand calculation and the simulation.

**Impact force analysis.** For a controlled descent scenario at 10 m/s with a 0.5 m stopping distance provided by the airbags, we get a deceleration of 100 m/s² and a resulting impact force of 7,500 N on a 75 kg system. Our structural analysis was run assuming a design impact force of 30,000 N, which gives a factor of safety of 4 against the estimated actual landing load.

**CFD analysis of the descent phase.** We modeled the descent at an estimated terminal velocity of 31 m/s (the unmitigated, no-parachute case) as an input condition, and the simulation confirmed the parachute and airbag combination brings the effective landing velocity down to roughly 6.2 m/s, consistent with the hand-calculated value above.

**Static structural analysis.** We evaluated three idealized impact scenarios on the outer casing, assuming the impact load lands at the corners since that is the most likely first-contact geometry for a dropped rectangular body:

- **One corner impact**: maximum deformation of approximately 0.7985 mm, within acceptable limits.
- **Two corner impact**: maximum deformation of approximately 1.829 mm, also within limits.
- **All four corner impact**: maximum deformation of approximately 1 mm, also within limits.

All three cases stayed within acceptable deformation bounds, which gave us confidence that the casing design could survive a realistic range of landing orientations rather than just the single best-case scenario.

## Mission use case

The scenario we designed around: a major natural disaster hits a remote region, communication infrastructure is down, and local resources are stretched thin. The workstation is air-dropped from a helicopter at 1000 meters altitude. During descent, the onboard navigation system keeps it tracking toward the intended drop site. The parachute slows the fall to about 18 m/s, and the airbags absorb the final landing shock at speeds up to 20 m/s.

On touchdown, the system activates automatically. The solar array deploys, the power module establishes a satellite link, and it starts broadcasting GPS coordinates while enabling emergency communication over ham radio. From there, relief teams can use it as a mobile command center: coordinating drone-based aerial surveys through the IoT dashboard, distributing medical supplies from Module 2, purifying water on site, and using the thermal imaging camera from Module 3 to locate survivors who are not visible by eye, under debris or vegetation for example.

## Conclusion

This project was our attempt at an integrated answer to a genuinely hard systems problem: how do you deliver a useful, self-sufficient command post into a disaster zone without any ground infrastructure, have it survive an uncontrolled-ish landing, and have it immediately operational. The modular architecture, hybrid power system, and validated descent and structural design were all decisions made to keep the system realistic rather than aspirational, and the engineering calculations and simulations in this document were done to back that up rather than just assert it. It reflects a genuinely interdisciplinary effort across power systems, structures, CFD, and embedded systems, which is exactly the kind of problem we wanted to take on for this designathon.
