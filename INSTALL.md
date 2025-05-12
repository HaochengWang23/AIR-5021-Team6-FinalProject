# Installation
## **Dependencies**

Python versions:

* Python 3.10

Operating systems:

* Linux: Ubuntu 18.04+, Centos 7+


Hardware:

* Rendering: NVIDIA or AMD GPU

* Ray tracing: NVIDIA RTX GPU or AMD equivalent

* Ray-tracing Denoising: NVIDIA GPU

* GPU Simulation: NVIDIA GPU

Software:

* Ray tracing: NVIDIA Driver >= 470
* Denoising (OIDN): NVIDIA Driver >= 520

## 0. Install Vulkan
```
sudo apt install libvulkan1 mesa-vulkan-drivers vulkan-tools
```

## 1. Basic Env
First, prepare a conda environment.
```bash
conda create -n RoboTwin_Challenge python=3.10
conda activate RoboTwin_Challenge
```

```
pip install torch==2.4.1 torchvision sapien==3.0.0b1 scipy==1.10.1 mplib==0.2.1 gymnasium==0.29.1 trimesh==4.4.3 open3d==0.18.0 imageio==2.34.2 pydantic zarr huggingface_hub==0.25.0 hydra-core==1.2.0
```

Then, install pytorch3d:
```
cd third_party/pytorch3d_simplified && pip install -e . && cd ../..
```

## 2. Download Assert
```
bash script/_download_assets.sh
```

## 3. Modify `mplib` Library Code: Remove `or collide`
```
# use `pip show mplib` to find where mplib is installed
# mplib.planner (mplib/planner.py) line 807
# remove `or collide`

if np.linalg.norm(delta_twist) < 1e-4 or collide or not within_joint_limit:
                return {"status": "screw plan failed"}
=>
if np.linalg.norm(delta_twist) < 1e-4 or not within_joint_limit:
                return {"status": "screw plan failed"}
```


