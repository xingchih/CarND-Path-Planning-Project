# CarND-Path-Planning-Project
Self-Driving Car Engineer Nanodegree Program


## Methods
The path generation consists of two main steps:

1. Find cars around and plan behaviour (e.g., speed and lane change)
2. Genenerate path using spline

First, we find cars around and plan behaviour (e.g., speed and lane change).

We start with `previous_path_x` and `previous_path_y` which are the previously calculated path x and y values sent to simulator, yet not consumed by the simulator. If `previous_path_x` is not empty, then we use `end_path_s` as current car s position. We then process the data in `sensor_fusion` and determine the relative position of other cars around us. 

For cars in our lane, if they are in front of us and within a pre-defined distance, we mark the flag `too_close_m` to true and decrease our speed (at a step of 0.75m/s), see function `isCarTooClose()`. If there is no car in front of us or the cars are further than the safe distance away (>30m), we want to drive at a speed that is as close to our reference speed (set at 49.5 mph). If current speed is less than reference speed, we increase our speed with a step of 0.75m/s. 

We also exam the cars in the neighbouring two lanes (left and right), see function `isLaneOpen()`. If there are no cars in either lane, or cars are further away (>30m for cars are in front of us and >10m for cars behind us), we mark these two lanes as open for lane change. When there is a slow moving car in our lane, and there is at least one neighbouring lane is open for lane change, we would perform a lane change maneuver. When both left and right lanes are possible for lane change, we prefer to switch the left lane, with the assumption that the left lane is for faster traffic.

Second, we generate path using spline. 

If previous path is almost empty (size<2), we use the car as starting reference, otherwise, we use previous path end point as starting reference. The last two points from previous path are used to ensure a smooth, continous connection between previous and current paths. We add three way points that are evenly 30m spaced ahead of the starting reference, transform these points into local coordinate frame of the car, use spline to generate a smooth path, and then transform them back into global coordinate frame. These points are appended to the previous path. All points are then sent to the simulator.

## Results and Discussion
This implementation allows the car to drive smoothly in the simulator, meeting all criteria, including max acceleration, max jerk, collision free and smooth lane chaning behaviour. The intrinsic properties of the spline enabled very nice, smooth trajectory and lane changing behaviour.

Possible improvements are incorperating a finite state machine that describes and explictly tracks the state of the car. Project instruction also suggested adding a controller, such as PID or MPC, which can improve the tracking performance of the path planner's output path.


