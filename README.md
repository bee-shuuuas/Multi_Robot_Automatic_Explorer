# Multi_Robot_Automatic_Explorer
This repository is a multi-robot simulation which explores the environment automatically using the Frontier Exploration Algorithm. For this to understand, we must know the basic of SLAM, navigation and frontier exploration algorithm.

![Demo](Demo.gif)


# 🤖 Multi_Robot_Automatic_Explorer – Repository Explanation

This repository simulates **multiple robots exploring an environment automatically** using the **Frontier Exploration Algorithm**. The goal is to map the environment collaboratively using SLAM and merge the maps from each robot into a unified one.

---

## 🔧 Basic Concepts Involved

| Concept                | Description                                                                 |
|------------------------|-----------------------------------------------------------------------------|
| **SLAM**               | Simultaneous Localization and Mapping – lets a robot map an unknown space while localizing itself within it. |
| **Navigation Stack**   | Allows the robot to plan and move safely through the environment.           |
| **Frontier Exploration** | Algorithm that helps robots decide _where to explore next_ (frontiers = boundary between known and unknown space). |
| **Multi-Robot Mapping**| Combining maps from multiple robots into one unified map.                  |

---

## 🧠 What's Happening in the Repository?

A breakdown of the simulation logic:

### 1. **Multiple Robots Setup**
- Robots are named like `tb3_0`, `tb3_1`, ..., up to `tb3_5`.
- In some configs or logs, they might be referred to as `tb1`, `tb2`, etc.
- Each robot is a **TurtleBot3**, running its own **SLAM** and **navigation** nodes.

### 2. **URDF with Namespacing**
- Each robot has its own **URDF**.
- All links and joints in the URDF are **namespaced** (e.g., `/tb3_2/base_link`) to prevent conflicts.
- This allows running multiple robot instances simultaneously without topic or TF clashes.

### 3. **Asynchronous SLAM Toolbox**
- Each robot uses **SLAM Toolbox** in **asynchronous mode**:
  - Enables **independent SLAM** per robot.
  - Ideal for **multi-agent systems**, where robots can map simultaneously.

### 4. **Map Merging**
- Each robot generates its **own local map**.
- These maps are later **merged** to form a **single global map**.
- Tools like `map_merge` or custom TF alignment strategies are typically used for this purpose.

### 5. **Frontier Exploration Algorithm**
- The robots use the **Frontier Exploration Algorithm** to explore:
  - **Frontiers** = boundary between known and unknown space.
  - Robots detect frontiers and plan paths to them.
  - Frontiers are **assigned** to the most suitable robot.
  - As frontiers are visited and explored, **new ones are selected**.
- The process continues until **no more frontiers remain**, i.e., the map is complete.

---

## 🗺️ Summary Schematic

```plaintext
[ Each Robot (tb3_0 ... tb3_5) ]
       |
       |--> Runs its own SLAM (SLAM Toolbox async)
       |--> Explores with Navigation Stack
       |--> Frontier detection (Frontier Exploration)
       |
[ Namespaced URDFs to avoid collision ]
       |
       V
[ Individual Maps Generated ]
       |
       V
[ Map Merging Process ]
       |
       V
[ Shared Global Map ]

