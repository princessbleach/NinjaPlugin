# Ninja Plugin – Handover Documentation


**Asset Name:** Ninja Plugin  
**Asset Type:** Playable Character System (Locomotion, Combat, VFX)  
**Author:** Zoe Efstathiou  
**Contact:** 2423029@students.uca.ac.uk  
**Client** Eric Chen  
**Delivery Date:** 25.11.2025 

---

## 1. Asset Overview

The Ninja Character Plugin provides a fully implemented, ready-to-use playable character for Unreal Engine 5.6.1. It includes a complete skeletal mesh, materials, locomotion system, basic combat system, and two Niagara VFX.

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

---

## 2. Integration Guide

### 2.1 Installing the Plugin
1. Place the plugin folder into:  
   `YourProject/Plugins/`
2. Launch or restart Unreal Engine.  
3. Enable the plugin via:  
   **Edit → Plugins → Installed → Ninja Character Plugin**

### 2.2 Adding the Character to a Level
1. Navigate to:  
   `Plugins/NinjaCharacter/Blueprints/`
2. Drag **BP_NinjaCharacter** into the level.  
3. (Optional) Set the character as the default pawn:  
   **Project Settings → Maps & Modes → Default Pawn Class → BP_NinjaCharacter**

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

### 3.1 Materials
Material Instances are included for adjusting:
- Base colour  
- Roughness  
- Metallic values  

### 3.2 Locomotion Tuning
Editable character movement variables:
- Max walk speed  
- Sprint speed multiplier  
- Jump Z velocity  
- Dodge launch strength  

### 3.3 Combat Adjustments
- Combo windows controlled via animation notifies  
- Attack animations can be swapped or extended  
- Sword slash effect has parameters for:
  - Opacity  
  - Colour  
  - Lifetime  

### 3.4 VFX Customization
**Sword Slash (Niagara):**
- User parameters for scale, colour, lifetime  

**Sprint Wind Lines:**
- Editable intensity  
- Blend in/out duration  
- Colour tint  

---

## 4. Technical Documentation

### 4.1 Locomotion System
Implemented through an Animation Blueprint using:
- A movement blendspace (idle → walk → run)  
- Sprint state override  
- Jump start, loop, and land states  
- Dodge state triggered via input and root-motion curve  

Transitions use:
- Character velocity  
- IsFalling boolean  
- Sprinting and dodging boolean variables  

### 4.2 Combat System
Blueprint logic handles:
- Weapon state (equipped / unequipped)  
- Attack montage playback  
- Combo progression triggered within notify windows  
- Shuriken throw: animation only  

### 4.3 Niagara Effects
**Sword Slash**
- Ribbon or sprite-based slash emitted from sword socket  
- Triggered by an animation notify  
- Controlled via exposed parameter set  

**Sprint Wind-Line Effect**
- Niagara + post-process material  
- Enabled only during sprint  
- Smooth fade-out when sprint ends  

---

## End of Document
