# kaiaai_teleop ROS2 Package
Control a Kaia.ai robot (physical or simulated) using a keyboard.

See [kaiaai](https://github.com/kaiaai/kaiaai) for more instructions.

## Common Usage
Launch keyboard teleop for the default robot model. This is the most common usage scenario.
```
ros2 run kaiaai_teleop teleop_keyboard
```

## No bringup launch file
`teleop_keyboard` node cannot be launched from a `launch` file, including `.launch.py`.
If you try doing that, you will get an error (in Linux) `termios.error: (25, 'Inappropriate ioctl for device')`.

This error appears because `teleop_keyboard.py` needs direct access to TTY STDIN device in order to
capture keyboard strokes.

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
