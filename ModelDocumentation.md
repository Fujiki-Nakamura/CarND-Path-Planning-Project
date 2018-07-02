## Reflection
### Prediction
From the `line 257` in `src/main.cpp`, the program makes predictions about the other cars around the ego vehicle. Using the sensor fusion data, it decides which car is on which lane. Also using the same sensor fusion data, it predicts the other cars' positions in the near future (`line 279`). Using the lane decisions and the position predictions of the cars, it makes three decisions about its environment (from `line 281`):
1. if a car is ahead of the vehicle and the distance between the car and the vehicle is less than 30m, the car is too close.
2. if a car is on the left lane of the vehicle and the car is within 30m ahead or 30m behind of it, it has a car on the left.
3. if a car is on the right lane of the vehicle and the car is within 30m ahead or 30m behind of it, it has a car on the right.

### Behavior
Based on the three decisions about the environment, the ego vehicle decides its behavior (from `line 296`). If it is not too close to the car ahead of it, it can speed up to the maximum velocity (49.5 MPH) by the maximum acceleration (.224). If it is too close to the car ahead, it can choose to turn left or right to pass the car. First it tries to turn left but if it has a car on the left, then it tries to turn right. However, if it also has traffic on the right, it decides to speed down instead of making turns.

### Trajectory Generation
The part of the trajectory generation begins from `line 308` based on the information of the velocity and the lane calculated in the previous part, and the previous path. First, the last two points from the previous path and three other points at far distances (30, 60 and 90m ahead) are used to initiaze the calculation of the spline (`from line 315`). The spline is used to interpolate waypoints to get a smooth path. To have more continuity between the previous and the next path, the previous path points are copied to the next path points. After copying, the next path is filled with the points calculated by the spline. In the spline calculation of the points, the number of the points added to the path is calculated in order to keep the velocity.
