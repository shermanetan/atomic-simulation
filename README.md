# atomic-simulation

This notebook covers key concepts of atomistic simulation using Python. The objective by the end of this notebook is to program a simple molecular dynamics model and use it to simulate aspects of irradiation damage.

An atomistic simulation consists, in the simplest sense, of a box of particles, a rule for how those particles interact with one another, and a set of rules for how the state of the system evolves over time. Each of these concepts is addressed in turn.

## A simulation of irradiation damage in RPV steel
### A collision cascade
Firstly, it should be decided what should be simulated. For this example, irradiation damage to a block of pressure vessel steel, exposed to energetic neutrons in a light water nuclear fission reactor, is considered. Neutrons are released from the fissile uranium dioxide fuel in the core of the reactor and pass into the crystal lattice of the pressure vessel steel. Many of these neutrons pass through the wall of the pressure vessel without causing damage, but some collide with the nuclei of the atoms that make up the pressure vessel. These collisions transfer large amounts of kinetic energy to these atoms, setting them in motion at high speed. The disturbed atoms then collide with other atoms, initiating further collisions, and so on. This creates a significant local disturbance (in a region a few hundred nanometers across) in the crystal lattice, in a phenomenon known as a collision cascade. After only a few nanoseconds, the disturbance dissipates, and, remarkably, most of the damage heals, restoring the perfect crystal. However, a few atoms remain displaced from their original lattice sites, leaving behind point defects. These are vacancy defects (where a lattice site is unoccupied) and interstitial defects (where an atom sits outside of a lattice site). These point defects move over time, possibly forming clusters, which in turn can obstruct the motion of dislocations, leading to embrittlement of the reactor pressure vessel.

### The role of simulation
Understanding the details of the damage process is crucial. Few experimental techniques have the required time and spatial resolution to directly observe irradiation damage, which makes simulations particularly valuable for studying such processes.

### Approximating the phenomenon
Since directly simulating a collision cascade is too complex, approximations are necessary for the simulation. Moreover, by simulating a collision cascade, it is possible to simplify the situation, making it easier to identify the most important aspects of the real problem.

In this case, the following approximations are made to make the simulations tractable:

1) **Incoming neutron simulation:** Rather than simulating the incoming neutron directly, the simulation starts when an atom, known as the primary knock-on atom (PKA), is set in motion. Simulating a large amount of material and many neutrons would be needed to observe a single cascade, as incoming neutrons rarely initiate a cascade. Additionally, to simulate the collision between a neutron and an atom, the simulation would need to capture nucleon-nucleon interactions, which is beyond the scope of this model.

2) **Microstructure:** The simulation ignores microstructure, opting for a small block of perfect single crystal. This simplification allows longer simulation times and facilitates the extraction of details that might be obscured by features such as grain boundaries or dislocations. However, these features are important in determining the level of damage in real materials, as they can act as defect sinks, for example. The complication of dealing with crystal surfaces is avoided by employing *periodic boundary conditions*.

3) **Steel Composition:** The simulation ignores the complexity of steel composition and models a block of pure iron in the body-centered cubic (BCC) phase. Although real steels contain many other elements, it is assumed that the dominant effect on damage is determined by the arrangement of iron atoms in the steel.

### The Simulation
The simulation will set up a small block of pure, defect-free, BCC Fe crystal and initiate the motion of one Fe atom to represent the PKA.
