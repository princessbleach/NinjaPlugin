# Ninja Plugin – Handover Documentation


**Asset Name:** Ninja Plugin  
**Asset Type:** Playable Character System (Locomotion, Combat, VFX)  
**Author:** Zoe Efstathiou  
**Contact:** 2423029@students.uca.ac.uk  
**Client** Eric Chen  
**Delivery Date:** 25.11.2025 

---

## 1. Asset Overview

The Ninja Plugin provides a fully implemented, ready-to-use playable character for Unreal Engine 5.6.1. It includes a complete skeletal mesh, materials, locomotion system, basic combat system, and two VFX.

### Features
- **Character Asset**
  - Ninja skeletal mesh and materials  
  - Weapon socket setup  
- **Locomotion System**
  - Idle, walk, run, sprint  
  - Jump  
  - Dodge / roll  
- **Combat System**
  - Equip / unequip sword  
  - Single attack + combo continuation  
  - Shuriken throw animation (no projectile logic included)  
- **Visual Effects**
  - Sword slash Niagara effect  
  - Sprint wind-line post-process effect  
- **Demo Level**
  - Basic environment with on-screen text to showcase plugin.

---
  <div style="display: flex; gap: 10px;">
  <img src="https://github.com/princessbleach/NinjaPlugin/blob/main/locomotion.gif?raw=true" width="400"/>
  <img src="https://github.com/princessbleach/NinjaPlugin/blob/main/attack.gif?raw=true" width="400"/>
</div>

*Figure 1 and 2. Demo of Locomotion and Sword Attacks.*


---


## 2. How to Install

### 2.1 Installing the Plugin
1. Place the plugin folder into:  
   `YourProject/Plugins/`
2. Launch or restart Unreal Engine.  
3. Enable the plugin via:  
   **Edit → Plugins → Installed → Ninja Character Plugin**
4. If you do not have this folder, create one.

### 2.2 Adding the Character to a Level
1. Navigate to:  
   `Plugins/NinjaPlugin/Ninja`
2. Drag **BP_Ninja** into the level.  
3. Set the character as the default pawn:  
   **Project Settings → Maps & Modes → Default Pawn Class → BP_Ninja**

### 2.3 Input Requirements
The plugin uses the following input mappings:
- **WASD** – Movement  
- **Space** – Jump  
- **Left Shift** – Sprint  
- **Q** – Dodge  
- **E** – Equip / Unequip  
- **Left Mouse Button** – Attack  
- **Right Mouse Button** – Shuriken Throw Animation  

### 2.4 Exposed Parameters
Editable inside **BP_NinjaCharacter**:
- Walk/run/sprint speeds  
- Dodge distance  
- Combo timing values  
- Slash VFX intensity  
- Sprint wind-line post-process intensity  

---

## 3. Customization Guide

### Input

If you wish to change the input key binds, navigate to the `Input` folder and then `IMC_Default`. Here you can change them.

### Material Instances

**Slash VFX Customization**
 
User parameters can be found for opacity power and colour. 


**Sprint Post Process Material**

Material Instances are included for adjusting:
- Intensity
- Alpha
- Radius
- Density



<img src="https://github.com/princessbleach/NinjaPlugin/blob/main/Screenshot%202025-11-24%20135719.png?raw=true" width="600">

*Figure 3. Global Parameters Example.*

The parameters can be found in **Global Scalar Parameter Values**
Both material instances can be found in the `Instances` folder in `FX`.



---

## 4. How the Logic Works


### Locomotion System
The locomotion system is built using a standard state machine with states for:
- Idle / Walk / Run
- Sprint
- Jump (Start → Loop → End)
- Dodge / Roll

The Ninja BP sets simple conditions:
- Sprint activates when the Sprint input is held and the character is not dodging.
- Jump triggers when the character becomes airborne.
- Dodge triggers a short forward launch and temporarily disables input so the movement cannot be interrupted.

Layered Blend per Bone is used to allow the upper body to play separate animations (holding the sword or attacking) while the lower body continues normal locomotion. This is done within the animation blueprint.

---

### Sprint System
The sprint input triggers:
- A speed increase (600 → 900)
- The sprint post-process effect (wind lines)

When the input is released:
- Speed returns to normal
- The post-process effect is faded out

The logic simply checks if the character is dodging; if not, sprinting becomes active.

---

### Dodge / Roll
Pressing the dodge key:
- Triggers the dodge animation
- Launches the character forward by a fixed amount
- Disables input briefly so the animation and movement play cleanly
- Re-enables input at the end

This was implemented in this way as the animation does not have root motion. If you wish to use a root motion roll animation, the launch character node can be removed as this is to accommodate the location without root motion.
The logic also prevents dodging while sprinting or performing other actions.

---

### Combat System (Light Combo)

The combat system is within an actor component connected to the Ninja BP.

The attack input follows a simple two-step combo logic:
1. **First click** → plays Attack 1
2. **Second click within the combo window** → plays Attack 2

Blueprint logic checks:
- Is the sword equipped?
- Is the character already attacking?
- What the current combo count is

A short timer resets the combo if the player does not continue it.
An animation notify is used within the montages to create an attack window.

---

### Weapon Equip System
Pressing Equip toggles the weapon:

- If unequipped → a sword actor is spawned and attached to the hand socket
- If equipped → the existing sword actor is destroyed

A boolean is used in the AnimBP to switch between normal locomotion and the “holding sword” upper-body pose.

---

### Shuriken Throw (Animation Only)
The right-click input checks that the character is not sprinting or dodging.

If allowed:
- A shuriken-throw animation montage plays
- A short delay resets the action state

Only the animation is included — no projectile logic.

---

### VFX

**Sprint PPM**
A simple post-process material is used to generate directional lines while sprinting:

- A panning texture and gradient mask create the streak effect
- A single **Alpha** parameter controls visibility
- Alpha is driven by the sprint blueprint (1.0 when sprinting, 0 when not)


**Sword Slash VFX**
- Spawns during attack animations via notify
- Attached from the sword base to sword tip sockets. 
- Uses exposed parameters for colour, lifetime, and scale

---




