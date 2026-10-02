# Low-Poly 3D Hamster Habitat Simulator

Build a polished, interactive browser simulation with autonomous hamsters and a self-sustaining population. Return **only one complete `index.html`** containing all HTML, CSS, and JavaScript.

Use **Three.js and OrbitControls via public CDN ES modules**. No build system, backend, external models, textures, images, JSON files, or other local assets. The file must work through a simple local HTTP server. Deliver a complete playable simulation, not pseudocode or a static prototype.

## 1. Scene and Habitat

- Create a full-viewport, responsive scene with antialiasing, soft shadows, ambient/hemisphere light, directional sunlight, tone mapping, and a capped pixel ratio. Use a cohesive low-poly palette, rounded silhouettes, character details, and flat shading where appropriate.
- Generate everything procedurally: cage tray, transparent or wire walls, supports, entrance, raised platform with a ramp, running wheel, food bowl, water bottle, sleeping house, tunnel, and chew toy.
- Add at least **150 bedding pieces** with varied placement, rotation, scale, and color, using shared resources or instancing.
- Implement a smooth day/night cycle, approximately 180 simulation seconds, changing background and lighting and influencing sleep behavior.

## 2. Hamsters and Autonomous Behavior

- Start with **5 hamsters**, each with a name, distinct palette, proportions, movement traits, and personality. Build separate body, head, ears, eyes, nose, four legs, and tail from primitive geometry.
- Use an explicit state machine covering idle, wandering, seeking/eating food, seeking/drinking water, seeking/using the wheel, exploration, sleep, socializing, seeking a mate, courtship, and dying.
- Decisions must follow evolving hunger, thirst, energy, curiosity, social need, age, and health. Eating/drinking satisfy needs only when resources are available; sleep restores energy. Prioritize survival over optional activities.
- Navigate with smooth acceleration, turning, arrival, obstacle avoidance, and separation. Stay inside the cage, avoid objects and other hamsters, and physically use the ramp to reach the platform. No teleportation, floating, obstacle clipping, or predetermined animation paths masquerading as autonomous behavior.
- Animate according to state and speed: alternating legs, body bob and head movement, eating head dips, drinking at the nozzle, sleeping breaths, and faster running legs. Social encounters briefly face partners toward each other with small gestures before releasing both agents.

## 3. Habitat Interactions

- **Wheel:** approach its entrance, enter, align, run, and leave after a variable duration. Keep the hamster correctly positioned while wheel rotation matches its running speed. Allow exactly **one occupant**; others wait or choose another activity.
- **Food and water:** show finite pellets and a visible water level that decrease with consumption. Empty resources cannot satisfy needs; agents must abandon unsuccessful attempts.
- **Sleep:** allow at most **two hamsters inside the house**. Provide safe alternative resting places when full.
- Add temporary visual feedback such as crumbs, droplets, sleep particles, and wheel dust. Interactions must be visible in the 3D world, not represented only by UI text.

## 4. Life Cycle and Population Balance

Implement visible **birth, growth, reproduction, aging, and death**. Five is the starting population, not a permanent count.

**Design and tune the balancing logic yourself** so the habitat runs unattended across many generations without overcrowding or lasting extinction. Choose suitable lifespans, maturation and pregnancy durations, litter sizes, breeding cooldowns, resource supply, a recovery threshold, a target population range, and a hard cap. Keep these in one configuration object and briefly explain the balance in code comments.

- Track unique IDs, sex, age/life stage, health, lifespan, parents, and generation. Start with varied ages and a viable mix of breeding adults. Offspring receive names, inherit some parental traits with variation, visibly grow, and develop autonomous behavior; seniors show subtle age-related changes.
- Reproduction requires compatible healthy adults, adequate energy/resources, completed cooldowns, and available capacity. Show brief non-explicit courtship, pregnancy, birth near a parent or nest in a safe location, and recovery. Track pregnancy separately from activity so normal behavior continues. Prevent duplicate pregnancies and births.
- All hamsters eventually die of old age. Prolonged critical hunger/thirst may also reduce health and cause death; brief shortages must not kill instantly. Show a gentle death transition and log the cause. Release occupancy, partner links, pending births, and reservations; remove dead agents from active logic and selection, handle the follow camera safely, and clean up visuals.
- Reduce fertility near the upper target and encourage eligible breeding when numbers are low. Account for maturation, pregnancy, and expected deaths. Reserve whole litters before pregnancy: **living population + reserved newborn/rescue slots must never exceed the cap**, including simultaneous events. Fulfill or cancel reservations exactly once. Use cooldowns and stable thresholds to avoid repeated booms and crashes.
- If natural recovery becomes impossible, including no viable breeding pair or zero survivors, use limited, logged rescue/adoption arrivals through the entrance, respecting capacity and a cooldown. Recovery must work even at zero agents and complete within a bounded simulation-time window. Normal population renewal should primarily come from reproduction.
- Enable automatic care by default: visibly replenish finite food and water in controlled, logged amounts to support the target population. Shortages still affect needs and breeding. Keep manual refills available.

Do not balance by arbitrarily killing excess animals, making survivors immortal, or secretly resurrecting them. Tune the pace so a practical demonstration shows births, growth, and natural deaths while allowing time for ordinary behaviors.

## 5. Interface, Camera, and Statistics

Create a compact, responsive HTML/CSS overlay that complements the scene.

- Raycast to select and highlight any hamster. Show name, state, needs, health, age/life stage, sex, parents/generation, pregnancy/cooldown, distance traveled, wheel runs, and food eaten. Avoid permanent floating DOM labels over every animal.
- Show simulation time, FPS, food/water remaining, wheel usage, sleepers, living population/target range, age groups, pregnancies/reserved slots, births, deaths, rescue arrivals, automatic care status, and the reason breeding is encouraged or restricted.
- Provide pause/resume, **0.5x / 1x / 2x / 4x** speed, food/water refills, simulation reset, smooth follow-camera with user rotation, and smooth camera reset.
- Keep a bounded event feed generated from actual activities, births, deaths, care, and population interventions.
- Persist lifetime wheel runs, food consumed, simulated time, births, deaths, rescue arrivals, and peak population in `localStorage`; include a clear-statistics button. Do not persist the whole scene.

## 6. Reliability and Completion

- Use one simulation clock for behavior, movement, animation, needs, lifecycle, and automatic care. Speed affects all systems consistently; pause freezes simulation while rendering and camera controls remain usable.
- Reset without reloading: restore five hamsters and initial resources; clear occupancy, pregnancies, reservations, temporary effects, timers, and per-run counters. Preserve lifetime statistics until explicitly cleared.
- Organize code into clear components for agents, habitat objects, population, particles, and simulation management; separate logic from rendering where practical.
- Target approximately **60 FPS on a modern desktop**, including the configured maximum population. Reuse vectors, geometry, and materials; avoid unnecessary per-frame allocation and dispose temporary resources correctly.
- Prevent escapes, invalid coordinates, stuck agents, state deadlocks, endless navigation attempts, occupancy conflicts, duplicate lifecycle events, stale camera targets, and memory leaks. Add recovery fallbacks.
- Verify all interactions and controls, plus unattended operation over multiple generations and random seeds. Cover simultaneous pregnancies near capacity, shortages, loss of the last fertile pair, zero population, death while using an object, resizing, speed changes, pause/resume, and reset during pregnancy. Confirm stable population recovery and no cap violations or stale events.

All systems must work together in the final file. Visual polish, visible autonomous behavior, and sustainable population dynamics are required; do not omit features because of implementation complexity.
