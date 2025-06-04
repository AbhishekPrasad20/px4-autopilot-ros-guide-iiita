

```markdown
# PX4 Autopilot + ROS 2 Guide (IIITA) 🚀  
A step-by-step guide to setting up **PX4 Autopilot**, **ROS 2**, and **Micro XRCE-DDS** for seamless drone simulation and control.

---

## 📌 PX4 Development Environment Setup  
### **Install PX4**
```bash
cd ~
git clone https://github.com/PX4/PX4-Autopilot.git --recursive
bash ./PX4-Autopilot/Tools/setup/ubuntu.sh
cd PX4-Autopilot/
make px4_sitl
```
💡 **Tip:** To **skip simulator installation**, run:
```bash
bash ./PX4-Autopilot/Tools/setup/ubuntu.sh --no-sim-tools
```
📖 **More Info:** [PX4 Ubuntu Dev Guide](https://docs.px4.io/main/en/dev_setup/dev_env_linux.html)

---

## 📌 ROS 2 Installation  
### **Install ROS 2 Humble on Ubuntu 22.04**
```bash
sudo apt update && sudo apt install locales
sudo locale-gen en_US.UTF-8
sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
export LANG=en_US.UTF-8

sudo apt install software-properties-common curl -y
sudo add-apt-repository universe
sudo apt update

sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null

sudo apt update && sudo apt upgrade -y
sudo apt install ros-humble-desktop ros-dev-tools
source /opt/ros/humble/setup.bash && echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc
```
🚀 **Alternative:** Install **Foxy** using [ROS 2 Foxy Install Guide](https://docs.ros.org/en/foxy/Installation/Ubuntu-Install-Debians.html).  

### **Install Python Dependencies**
```bash
pip install --user -U empy==3.3.4 pyros-genmsg setuptools
```

---

# 📌 **Micro XRCE-DDS Agent & Client Setup**
PX4 uses **uXRCE-DDS** for communication via DDS. This setup ensures proper interaction between PX4 and ROS 2.

---

## **🔹 Prerequisites**
| Prerequisite | Required for | Installation Command |
|-------------|-------------|----------------------|
| **CMake ≥ 3.16** | Build system | `sudo apt install cmake` |
| **C++11 Compiler** | Build system | `sudo apt install build-essential` |
| **make/ninja** | Build system | `sudo apt install make ninja-build` |
| **Git** | Superbuild | `sudo apt install git` |
| **Fast-CDR** | Dependency | Superbuild or system install |
| **Fast DDS (fastrtps)** | Dependency | Superbuild or system install |
| **spdlog** | Logger profile | Superbuild or system install |
| **microxrcedds_client** | P2P profile | Superbuild or system install |
| **microcdr** | P2P profile | Superbuild or system install |
| **GTest/GMock** | Unit Testing | `sudo apt install libgtest-dev` |
| **Python** | Scripts/Tests | `sudo apt install python3` |
| **NSIS** | Windows installer | Download from [NSIS](https://nsis.sourceforge.io/) |

---

## **🔹 Install Micro XRCE-DDS Agent**
Before proceeding, ensure **Fast-CDR** is installed.

### **Step 1: Clone Repository**
```bash
git clone -b v2.4.2 https://github.com/eProsima/Micro-XRCE-DDS-Agent.git
cd Micro-XRCE-DDS-Agent
mkdir build && cd build
```

### **Step 2: Replace CMakeLists.txt**
Ensure you replace the default `CMakeLists.txt` with the one available in this repo before building.

### **Step 3: Build & Install**
```bash
cmake ..
make -j$(nproc)
sudo make install
sudo ldconfig /usr/local/lib/
```
📌 **Note:** Ensure `fastcdr` is installed before building.

---

## **🔹 Start the Micro XRCE-DDS Agent**
Once installed, run the agent to connect to PX4:
```bash
MicroXRCEAgent udp4 -p 8888
```
📌 **Important:** Keep this terminal open while running the PX4 client.
---

## 📌 Start PX4 Gazebo Simulation  
Launch PX4 SITL with Gazebo:
```bash
cd ~/PX4-Autopilot/
make px4_sitl gz_x500
```
Once started, PX4 will automatically connect to the Micro XRCE-DDS agent.

### **Expected Output**
PX4 system console should display:
```
INFO  [uxrce_dds_client] successfully created rt/fmu/out/failsafe_flags data writer
INFO  [uxrce_dds_client] successfully created rt/fmu/out/sensor_combined data writer
```
The Micro XRCE-DDS agent terminal should show:
```
[info] ProxyClient.cpp | create_datawriter | datawriter created
[info] ProxyClient.cpp | create_publisher | publisher created
```

---

## 🔗 Useful Resources  
- [PX4 SITL Debugging Guide](https://docs.px4.io/main/en/simulation/#debugging)  
- [PX4-ROS 2 Bridge](https://docs.px4.io/main/en/middleware/ros2.html)  
- [PX4 Gazebo Tuning](https://docs.px4.io/main/en/simulation/gazebo.html)  

---

🚀 **Contributions & Feedback**  
Feel free to open **issues** or **pull requests** to improve this guide!

```

