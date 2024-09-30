# kaiaai_teleop ROS2 Package
Control a Kaia.ai robot (physical or simulated) using a keyboard.

## Common Usage
Launch keyboard teleop for the default robot model. This is the most common usage scenario.
```
ros2 run kaiaai_teleop teleop_keyboard
```

Now you should be able to control your robot using your PC keyboard.
Make sure your physical or simulated robot is up and running.
See [kaiaai](https://github.com/kaiaai/kaiaai) for instructions on bringing your robot up physically and in a simulation.
```
root@e3043f4ccd4c:/ros_ws# ros2 run kaiaai_teleop teleop_keyboard
YAML file name : /ros_ws/install/makerspet_mini/share/makerspet_mini/config/teleop_keyboard.yaml
Max linear velocity 0.200        Max angular velocity 3.860
Control Kaia.ai-compatible Robot
--------------------------------
Moving around:
      w
 a    s    d
      x
w/x   : increase/decrease linear  velocity
a/d   : increase/decrease angular velocity
s     : keep straight
CAPS  : large step
Space : force stop
CTRL-C to quit
```

## Advanced Usage
Launch keyboard teleop for `makerspet_loki` robot model.
```
ros2 run kaiaai_teleop teleop_keyboard robot_model:=makerspet_loki
```

Launch keyboard teleop and use `/path/to/teleop_keyboard.yaml` configuration file.
```
ros2 run kaiaai_teleop teleop_keyboard --ros-args --params-file /path/to/teleop_keyboard.yaml
```

Launch keyboard teleop and use `makerspet_loki` robot's configuration file.
```
ros2 run kaiaai_teleop teleop_keyboard --ros-args --params-file `ros2 pkg prefix --share makerspet_loki`/config/teleop_keyboard.yaml
```

## Configuration file
`teleop_keyboard.yaml` is a configuration file containing robot speed parameters:
- minimum and maximum linear and angular speeds
- large and small steps for the linear and angular speeds
```
teleop_keyboard_node:
  ros__parameters:
    max_lin_vel: 0.2
    max_ang_vel: 3.86
    lin_vel_step: 0.05
    ang_vel_step: 0.2
    lin_vel_step_large: 0.2
    ang_vel_step_large: 2.5
```

## Modding robot configuration
Edit `config/teleop_keyboard.yaml` in a robot package to mod the robot speeds to your liking.

## PS - no bringup launch file
`teleop_keyboard` node cannot be launched from a `launch` file, including `.launch.py`.
If you try doing that, you will get an error (in Linux) `termios.error: (25, 'Inappropriate ioctl for device')`.
```
root@e3043f4ccd4c:/ros_ws# ros2 launch kaiaai_bringup teleop_keyboard.launch.py
[INFO] [launch]: All log files can be found below /root/.ros/log/2024-09-30-00-54-45-663064-e3043f4ccd4c-397
[INFO] [launch]: Default logging verbosity is set to INFO
Teleop params: /ros_ws/install/makerspet_mini/share/makerspet_mini/config/teleop_keyboard.yaml
[INFO] [teleop_keyboard-1]: process started with pid [398]
[teleop_keyboard-1] Traceback (most recent call last):
[teleop_keyboard-1]   File "/ros_ws/install/kaiaai_teleop/lib/kaiaai_teleop/teleop_keyboard", line 33, in <module>
[teleop_keyboard-1]     sys.exit(load_entry_point('kaiaai-teleop', 'console_scripts', 'teleop_keyboard')())
[teleop_keyboard-1]   File "/ros_ws/build/kaiaai_teleop/kaiaai_teleop/teleop_keyboard.py", line 194, in main
[teleop_keyboard-1]     node = TeleopKeyboardNode(start_parameter_services=False)
[teleop_keyboard-1]   File "/ros_ws/build/kaiaai_teleop/kaiaai_teleop/teleop_keyboard.py", line 62, in __init__
[teleop_keyboard-1]     self.tty_attr = None if os.name == 'nt' else termios.tcgetattr(sys.stdin)
[teleop_keyboard-1] termios.error: (25, 'Inappropriate ioctl for device')
[ERROR] [teleop_keyboard-1]: process has died [pid 398, exit code 1, cmd '/ros_ws/install/kaiaai_teleop/lib/kaiaai_teleop/teleop_keyboard --ros-args --params-file /ros_ws/install/makerspet_mini/share/makerspet_mini/config/teleop_keyboard.yaml'].
root@e3043f4ccd4c:/ros_ws#
```
The error above because `teleop_keyboard.py` needs direct access to TTY STDIN device in order to
capture keyboard strokes.
