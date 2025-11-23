Control Bootcamp: Overview & Feedback Control

Course Source: Steve Brunton - Control Bootcamp

Topic: Introduction to Control Theory, Feedback, and State-Space Analysis.

1. Control Types Classification

Understanding the different ways we can manipulate a system is fundamental to control theory.

Passive Control

Definition: Relies on the system's natural physical properties (mechanics, aerodynamics, materials) to maintain stability or performance, without external energy or computation.

Example:

Fins on an arrow: They aerodynamically stabilize the flight path naturally.

Car suspension: Springs and dampers absorb shock mechanically without sensors.

Active Control

Requires external energy (actuators) and a controller to influence the system.

A. Open Loop

Definition: The controller executes a pre-defined action without knowing the actual outcome. It does not measure the output.

Example: A Microwave. You set it for 2 minutes. It heats regardless of whether the food is frozen or burning. It assumes the internal model (Time = Heat) is correct.

B. Closed Loop (Feedback)

Definition: Uses sensors to measure the actual output ($y$), compares it to the desired target, and adjusts the input ($u$) continuously to minimize error.

Example: Cruise Control. If the car goes uphill (disturbance), the sensor detects the speed drop, and the computer automatically applies more gas to maintain the target speed.

2. Why Feedback? (The Core Benefits)

The lecture highlights four main reasons why Closed Loop (Feedback) is superior for complex systems:

Uncertainty: Handles model inaccuracies. Real-world systems rarely match our mathematical models perfectly ($A$ and $B$ are imperfect). Feedback corrects for these errors.

Instability: Stabilizes naturally unstable systems (e.g., balancing an inverted pendulum or a fighter jet).

Disturbances: Rejects external noise or forces ($d$) that push the system off track (e.g., wind gusts on a drone).

Efficiency: Achieves performance goals with optimal energy usage, rather than applying maximum force blindly.

3. Mathematical Formulation & Physical Intuition

This section translates the mathematical equations of State-Space control into physical intuition.

Step 1: State-Space System Dynamics (Physics of the System)

$$\dot{x} = Ax + Bu$$

Intuition: This equation is about predicting the future. It tells us how the system state will evolve in the next instant.

Breakdown:

$x$ (State Vector): The "full picture" of the system. E.g., for a drone: [position, velocity, angle, angular_velocity].

$\dot{x}$ (Rate of Change): The trend of change. Is the velocity increasing or decreasing?

$Ax$ (Internal Dynamics - System "Inertia"):

How would the system move if there were no control input ($u=0$)?

Example: If a spring is stretched ($x \neq 0$), it will pull itself back due to physics ($A$), even without you pushing it.

$Bu$ (Control Effort - Your Ability to Intervene):

$u$ is your command (e.g., voltage to the motor).

$B$ describes how powerful the actuators are. For the same voltage, a higher torque motor means a larger $B$, resulting in a greater impact on the system state.

Step 2: Output Equation (The Sensor's Perspective)

$$y = Cx$$

Intuition: The gap between reality and measurement. The "God view" sees the complete $x$, but engineers can only see what sensors report as $y$.

Breakdown:

$C$ (Observation Matrix): The sensor's "filter".

Example: Suppose $x$ contains [position, velocity], but you only have a GPS sensor. You can measure position, but not velocity directly. In this case, $y$ is only a part of $x$.

Full State Feedback assumption ($y=x$): In this lecture's derivation, we make an idealized assumption: our sensors are perfect and can measure every element of $x$ directly.

Step 3: Control Law (The Brain's Strategy)

$$u = -Ky$$

Intuition: The error correction mechanism. If the system deviates, push in the opposite direction.

Breakdown:

Negative Sign ($-$): The core of negative feedback. If the robot drifts left ($y$ is positive), I must push right ($u$ is negative). Without this sign, the error would grow indefinitely (positive feedback), causing the system to crash.

$K$ (Gain Matrix - The Controller's "Personality"):

Large $K$: "Aggressive". Corrects violently for small errors. Fast response, but risks overshooting (oscillation).

Small $K$: "Conservative". Gently nudges the system even if errors are large. Slow response, but smoother.

Goal: The core task of control theory is to calculate this perfect matrix $K$.

Step 4: Closed-Loop Dynamics (The "Hacked" System)

$$\dot{x} = (A - BK)x$$

Intuition: Altering the laws of physics. This is the magic of control theory.

Breakdown:

The original system is primarily governed by $A$ (natural physics).

We embed the controller into the system loop. To the outside world, the robot's physical characteristics have changed! It no longer follows $A$, but follows $(A-BK)$.

Why is this magic?

An inverted pendulum's natural $A$ dictates that it must fall (unstable).

If you design a good $K$, the mathematical properties of the new matrix $(A-BK)$ dictate that it must stay upright (stable).

Conclusion: We reshape the hardware's behavior ($A$) through software ($K$).
