# Racing-LMPC-ROS2

[![ROS2 Humble](https://github.com/HaoruXue/Racing-LMPC-ROS2/actions/workflows/ros2-humble-ci.yaml/badge.svg)](https://github.com/HaoruXue/Racing-LMPC-ROS2/actions/workflows/ros2-humble-ci.yaml)

C++ ROS2 packages that implement learning model predictive control for real-world autonomous race cars.

Paper: [Learning Model Predictive Control with Error Dynamics Regression for Autonomous Racing](https://arxiv.org/abs/2309.10716)

![lmpc-foxglove](vis.gif)

https://github.com/HaoruXue/Racing-LMPC-ROS2/assets/32390737/745e5f15-f159-42ad-be74-ac5673867a03

## Install

### Dependencies

- Ubuntu 22.04 ros2 humble
- osqp——用源码安装：
  ```bash
  git clone --recursive https://github.com/osqp/osqp
  cd osqp
  mkdir build && cd build
  cmake .. && make -j4
  sudo make install
  sudo ldconfig
  ```
  
- ipopt——apt安装：
  ```bash
  sudo apt install coinor-libipopt-dev
   ```
  
- casadi——源码安装：
  先把依赖装了：
  [casadi](https://github.com/casadi/casadi/wiki/InstallationLinux)
  ```bash
  git clone https://github.com/casadi/casadi.git
  cd casadi
  mkdir build && cd build
  cmake .. -DWITH_PYTHON=ON -DWITH_OSQP=ON -DWITH_IPOPT=ON -DCMAKE_BUILD_TYPE=Release -DPYTHON_PREFIX=/usr/local/lib/python3.10/dist-packages
  make -j4
  sudo make install
  sudo ldconfig
  ```

### Build

```bash
# Clone the repo
cd Racing-LMPC-ROS2
git checkout humble-release
# Install rosdep dependencies
rosdep update
rosdep install --from-paths src --ignore-src -r -y
# Build
colcon build --packages-up-to racing_lmpc_launch --cmake-args -DCMAKE_BUILD_TYPE=Release
```

### Run LMPC

Open Foxglove Studio. Load `lmpc.foxglove.json` layout file from the root of this repo. Open a Foxglove Bridge connection with the default port and IP setting.

In terminal 1, run the following command to launch the simulator:

```bash
source install/setup.bash
ros2 launch racing_lmpc_launch sim_barc_lmpc.launch.py
```

In terminal 2, run the following command to launch the foxglove bridge:

```bash
source install/setup.bash
ros2 launch foxglove_bridge foxglove_bridge_launch.xml
```

You can now use Foxglove Studio to visualize the simulation.

### Run Tracking MPC

Same as above, except run the following command in terminal 1:

```bash
source install/setup.bash
ros2 launch racing_lmpc_launch sim_barc_tracking_mpc.launch.py
```

For IAC Putnam full course, run the following command in terminal 1:

```bash
source install/setup.bash
ros2 launch racing_lmpc_launch sim_putnam_config_a_tracking_mpc.launch.py
```

Remember to change the line scales of the 3D pannel accordingly to view them easily. Change display frame to `base_link` because the track is huge.

## Cite As

```bibtex
@misc{xue2023lmpc,
      title={Learning Model Predictive Control with Error Dynamics Regression for Autonomous Racing}, 
      author={Haoru Xue and Edward L. Zhu and Francesco Borrelli},
      year={2023},
      eprint={2309.10716},
      archivePrefix={arXiv},
      primaryClass={cs.RO}
}
```
