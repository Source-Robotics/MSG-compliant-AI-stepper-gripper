# MSG gripper: getting started

[All models](https://github.com/Source-Robotics/robot_descriptions/blob/65907223b3aa5cb4680471d3c3639cba88ea6dc9/README.md) | [PAR6](https://github.com/Source-Robotics/robot_descriptions/blob/65907223b3aa5cb4680471d3c3639cba88ea6dc9/guides/par6.md) | [PAROL6](https://github.com/Source-Robotics/robot_descriptions/blob/65907223b3aa5cb4680471d3c3639cba88ea6dc9/guides/parol6.md) | [Change colors](https://github.com/Source-Robotics/robot_descriptions/blob/65907223b3aa5cb4680471d3c3639cba88ea6dc9/docs/COLORS.md) | [License and attribution](https://github.com/Source-Robotics/robot_descriptions/blob/65907223b3aa5cb4680471d3c3639cba88ea6dc9/ATTRIBUTION.md)

**Working with the gripper alone? Download a standalone package below.**
Both formats include **Solid jaws** (default) and **Soft TPU jaws**, with all meshes,
materials, finger joints, collision shapes and mass properties.

To customize appearance, edit the [shared color file](https://github.com/Source-Robotics/robot_descriptions/blob/65907223b3aa5cb4680471d3c3639cba88ea6dc9/source/appearance/colors.json)
and follow the [illustrated color guide](https://github.com/Source-Robotics/robot_descriptions/blob/65907223b3aa5cb4680471d3c3639cba88ea6dc9/docs/COLORS.md). Colors match standalone and arm-mounted versions.

| Solid jaws (default) | Soft TPU jaws |
| --- | --- |
| ![MSG with Solid jaws](previews/msg_solid.png) | ![MSG with Soft TPU jaws](previews/msg_soft_tpu.png) |

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
| Solid jaws (default) | [`scene_solid.xml`](https://github.com/Source-Robotics/robot_descriptions/blob/65907223b3aa5cb4680471d3c3639cba88ea6dc9/mujoco/msg/scene_solid.xml) | [`msg_solid.urdf`](https://github.com/Source-Robotics/robot_descriptions/blob/65907223b3aa5cb4680471d3c3639cba88ea6dc9/urdf/msg_description/msg_solid.urdf) |
| Soft TPU jaws | [`scene_soft_tpu.xml`](https://github.com/Source-Robotics/robot_descriptions/blob/65907223b3aa5cb4680471d3c3639cba88ea6dc9/mujoco/msg/scene_soft_tpu.xml) | [`msg_soft_tpu.urdf`](https://github.com/Source-Robotics/robot_descriptions/blob/65907223b3aa5cb4680471d3c3639cba88ea6dc9/urdf/msg_description/msg_soft_tpu.urdf) |

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

**ROS 2 / RViz:** install `msg_description` with colcon using
[the ROS 2 guide](https://github.com/Source-Robotics/robot_descriptions/blob/65907223b3aa5cb4680471d3c3639cba88ea6dc9/docs/ROS2.md), source your workspace, then run:

```bash
ros2 launch msg_description display.launch.py jaws:=solid
```

Use `jaws:=soft_tpu` for Soft TPU jaws. The launcher opens RViz and joint sliders;
the second finger follows its mimic relation. ROS 1 retains
`roslaunch msg_description display.launch jaws:=solid`.

## 3. Move or attach the gripper

`flange` is the massless physical mounting frame. The gripper is fixed there
for standalone viewing; attach that frame to your robot for a mounted assembly.
The package does not add the robot's separate 55.5 g flange hardware.

In native MuJoCo, `gripper_actuator` accepts meters: **0 opens** and
**-0.0525 closes**. Each finger has 52.5 mm of commanded travel,
with the retained joint equality coupling. The URDF uses the same named finger
joints and its retained mimic relation; the consumer must implement mimic and
adjacent-body collision filtering. See the [package README](https://github.com/Source-Robotics/robot_descriptions/blob/65907223b3aa5cb4680471d3c3639cba88ea6dc9/mujoco/msg/README.md)
for endpoint and contact details.

The complete modeled mass is **380 g**, including two **39.3 g** moving
assemblies. Local COM/inertia and actuator settings retain their current
estimates. Selecting jaws changes visible/contact geometry; soft-material
deformation is not simulated. The existing `tcp` is a control frame rather
than a newly measured jaw-specific grasp center.

## 4. View through the built-in camera

These **simulated views from `msg_camera`** show both fully open jaw options
and the orange box from the existing grasp-test scene. The modeled camera
uses a **51.5-degree vertical field of view** at **1280 x 720**. Demo lighting
is brighter for visibility; the camera pose, object and jaw geometry are unchanged.

| Solid jaws: built-in camera | Soft TPU jaws: built-in camera |
| --- | --- |
| ![MSG built-in camera with Solid jaws](previews/msg_camera_solid.png) | ![MSG built-in camera with Soft TPU jaws](previews/msg_camera_soft_tpu.png) |

The jaws frame the grasp area, and the close test object extends partly beyond
the image border. These are model renders, not calibrated real-camera photos.
The standalone gripper scene contains no test object; its camera sees the jaws
and background until you add objects to your own simulation.

[Camera settings and Python rendering example](https://github.com/Source-Robotics/robot_descriptions/blob/65907223b3aa5cb4680471d3c3639cba88ea6dc9/docs/MSG_CAMERA.md) explain
how to render the existing named camera. The same page and previews are included
under `docs/MSG_CAMERA.md` in the arm/MSG downloads. URDF provides the camera
mounting and optical frames; image rendering belongs to the consuming simulator.

## Use MSG already mounted on PAR6 or PAROL6

Download the **robot** package using the [PAR6](https://github.com/Source-Robotics/robot_descriptions/blob/65907223b3aa5cb4680471d3c3639cba88ea6dc9/guides/par6.md) or [PAROL6](https://github.com/Source-Robotics/robot_descriptions/blob/65907223b3aa5cb4680471d3c3639cba88ea6dc9/guides/parol6.md)
guide, then choose the corresponding mounted entry:

| Jaws | Robot MuJoCo scene | Robot URDF file |
| --- | --- | --- |
| Solid jaws | `scene_msg.xml` | `par6_msg.urdf` / `parol6_msg.urdf` |
| Soft TPU jaws | `scene_msg_tpu_glue.xml` | `par6_msg_tpu_glue.urdf` / `parol6_msg_tpu_glue.urdf` |

The [MoveIt 2 demos](https://github.com/Source-Robotics/robot_descriptions/blob/65907223b3aa5cb4680471d3c3639cba88ea6dc9/docs/MOVEIT2.md) support both MSG jaw options on either
arm, including TCP planning and coupled `open`/`closed` presets. Install the
arm's matching URDF and MoveIt packages for that workflow.

## Source checkout and validation

In the MSG hardware repository, **`msg_description/`** contains this
product's guide and local previews. Download and extract the model ZIP into a
separate working directory or ROS workspace. The hardware guide folder is
documentation; the extracted URDF folder is the actual ROS/model package.

All maintained models live in the central `robot_descriptions` repository,
under `mujoco/msg` and `urdf/msg_description`. A software project
can load those paths from a pinned checkout if it needs local model sources.

[Standalone validation](https://github.com/Source-Robotics/robot_descriptions/blob/65907223b3aa5cb4680471d3c3639cba88ea6dc9/reports/standalone_grippers_validation.json) compares
tool-relative FK, local inertia, masses and controls with the mounted models,
then checks physical close/open behavior and relocated package assets.
[Migration instructions](https://github.com/Source-Robotics/robot_descriptions/blob/65907223b3aa5cb4680471d3c3639cba88ea6dc9/guides/legacy-models.md) cover the older standalone exports.

## Maintained model source

This folder contains the product guide and preview images. Download the runtime
model packages using the links above and extract them into your working directory.
All model files are maintained in [robot_descriptions](https://github.com/Source-Robotics/robot_descriptions);
this hardware repository has no model submodule to initialize.

Current tested model release: **[v2.18.0](https://github.com/Source-Robotics/robot_descriptions/releases/tag/v2.18.0)**.
See the [shared product guide](https://github.com/Source-Robotics/robot_descriptions/blob/65907223b3aa5cb4680471d3c3639cba88ea6dc9/guides/msg.md)
for the corresponding central documentation.
