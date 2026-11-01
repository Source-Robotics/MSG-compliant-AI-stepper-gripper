# MSG gripper: getting started

[All models](https://github.com/Source-Robotics/robot_descriptions/blob/ff6c2ec64626c988abc030f56885e8a9babcf4ef/README.md) | [PAR6](https://github.com/Source-Robotics/robot_descriptions/blob/ff6c2ec64626c988abc030f56885e8a9babcf4ef/guides/par6.md) | [PAROL6](https://github.com/Source-Robotics/robot_descriptions/blob/ff6c2ec64626c988abc030f56885e8a9babcf4ef/guides/parol6.md)

**Working with the gripper alone? Download a standalone package below.**
Both formats include **Solid jaws** (default) and **Soft TPU jaws**, with all meshes,
materials, finger joints, collision shapes and mass properties.

| Solid jaws (default) | Soft TPU jaws |
| --- | --- |
| ![MSG with Solid jaws](https://raw.githubusercontent.com/Source-Robotics/robot_descriptions/ff6c2ec64626c988abc030f56885e8a9babcf4ef/source/grippers/previews/msg_solid.png) | ![MSG with Soft TPU jaws](https://raw.githubusercontent.com/Source-Robotics/robot_descriptions/ff6c2ec64626c988abc030f56885e8a9babcf4ef/source/grippers/previews/msg_soft_tpu.png) |

## 1. Download the gripper-only package

| Format | Download | Extracted folder |
| --- | --- | --- |
| MuJoCo | [Download msg_mujoco.zip](https://github.com/Source-Robotics/robot_descriptions/releases/latest/download/msg_mujoco.zip) | `msg/` |
| URDF | [Download msg_urdf.zip](https://github.com/Source-Robotics/robot_descriptions/releases/latest/download/msg_urdf.zip) | `msg_description/` |

These packages contain **only MSG**, with a fixed mounting frame and two
moving fingers. No arm or grasp-test fixture is included. Keep the extracted
folder together so the model can find its meshes.

## 2. Choose jaws and open the model

| Jaws | MuJoCo viewer scene | URDF entry file |
| --- | --- | --- |
| Solid jaws (default) | [`scene_solid.xml`](https://github.com/Source-Robotics/robot_descriptions/blob/ff6c2ec64626c988abc030f56885e8a9babcf4ef/mujoco/msg/scene_solid.xml) | [`msg_solid.urdf`](https://github.com/Source-Robotics/robot_descriptions/blob/ff6c2ec64626c988abc030f56885e8a9babcf4ef/urdf/msg_description/msg_solid.urdf) |
| Soft TPU jaws | [`scene_soft_tpu.xml`](https://github.com/Source-Robotics/robot_descriptions/blob/ff6c2ec64626c988abc030f56885e8a9babcf4ef/mujoco/msg/scene_soft_tpu.xml) | [`msg_soft_tpu.urdf`](https://github.com/Source-Robotics/robot_descriptions/blob/ff6c2ec64626c988abc030f56885e8a9babcf4ef/urdf/msg_description/msg_soft_tpu.urdf) |

**MuJoCo:** from the directory where you extracted the download, run:

```bash
python -m mujoco.viewer --mjcf=msg/scene_solid.xml
```

Choose the other `scene_*.xml` to change jaws. `scene.xml` also opens Solid jaws.
The native gripper-only model files are `msg_solid.xml` and
`msg_soft_tpu.xml`; the viewer scenes add lighting and a floor.

**URDF viewer:** load the selected `.urdf` from `msg_description/`, keeping
its `meshes/` folder alongside it. Upload the whole extracted package if a web
viewer requests a folder or ZIP.

**ROS 1 / RViz:** put `msg_description` in `catkin_ws/src`, build and source
your workspace, then run:

```bash
roslaunch msg_description display.launch jaws:=solid
```

Use `jaws:=soft_tpu` for Soft TPU jaws. The supplied launch is ROS 1; ROS 2 can
consume the URDF using its own launch setup.

## 3. Move or attach the gripper

`flange` is the massless physical mounting frame. The gripper is fixed there
for standalone viewing; attach that frame to your robot for a mounted assembly.
The package does not add the robot's separate 55.5 g flange hardware.

In native MuJoCo, `gripper_actuator` accepts meters: **0 opens** and
**-0.0525 closes**. Each finger has 52.5 mm of commanded travel,
with the retained joint equality coupling. The URDF uses the same named finger
joints and its retained mimic relation; the consumer must implement mimic and
adjacent-body collision filtering. See the [package README](https://github.com/Source-Robotics/robot_descriptions/blob/ff6c2ec64626c988abc030f56885e8a9babcf4ef/mujoco/msg/README.md)
for endpoint and contact details.

The complete modeled mass is **380 g**, including two **39.3 g** moving
assemblies. Local COM/inertia and actuator settings retain their current
estimates. Selecting jaws changes visible/contact geometry; soft-material
deformation is not simulated. The existing `tcp` is a control frame rather
than a newly measured jaw-specific grasp center.

## Use MSG already mounted on PAR6 or PAROL6

Download the **robot** package using the [PAR6](https://github.com/Source-Robotics/robot_descriptions/blob/ff6c2ec64626c988abc030f56885e8a9babcf4ef/guides/par6.md) or [PAROL6](https://github.com/Source-Robotics/robot_descriptions/blob/ff6c2ec64626c988abc030f56885e8a9babcf4ef/guides/parol6.md)
guide, then choose the corresponding mounted entry:

| Jaws | Robot MuJoCo scene | Robot URDF file |
| --- | --- | --- |
| Solid jaws | `scene_msg.xml` | `par6_msg.urdf` / `parol6_msg.urdf` |
| Soft TPU jaws | `scene_msg_tpu_glue.xml` | `par6_msg_tpu_glue.urdf` / `parol6_msg_tpu_glue.urdf` |

## Source checkout and validation

In the MSG hardware repository, **`description/`** opens this product's
instructions. Its shared source is at `description/source/`. Standalone models
are under `description/source/mujoco/msg` and
`description/source/urdf/msg_description`.

[Standalone validation](https://github.com/Source-Robotics/robot_descriptions/blob/ff6c2ec64626c988abc030f56885e8a9babcf4ef/reports/standalone_grippers_validation.json) compares
tool-relative FK, local inertia, masses and controls with the mounted models,
then checks physical close/open behavior and relocated package assets.
[Migration instructions](https://github.com/Source-Robotics/robot_descriptions/blob/ff6c2ec64626c988abc030f56885e8a9babcf4ef/guides/legacy-models.md) cover the older standalone exports.

## Shared source

These instructions use model version **2.13.0**. The [`source` submodule](https://github.com/Source-Robotics/robot_descriptions/tree/ff6c2ec64626c988abc030f56885e8a9babcf4ef)
pins that exact release. To initialize it in an existing checkout:

```bash
git submodule sync --recursive
git submodule update --init --recursive
```

GitHub's repository ZIP excludes submodules. Use the model ZIP downloads above
for a ready-to-use package, or clone this hardware repository with
`git clone --recurse-submodules`.
