# Udacity CARND Term 2 Project 5 - Model Predictive Control

[//]: # (Image References)
[image1]: https://raw.githubusercontent.com/ruanvdm11/Ruan_CARND_Term2_PROJ5/master/Reference_Images/Video_screenshot.JPG "Video1"

In this project is was necessary to use the supplied Udacity simulator in order to steer a virtual vehicle around a track using model predictive control. This is an advanced method of control relying on the fact that there is a level of certainty concerning the mathematical model of the system, which in this case would be the kinematic equations of a vehicle.

## The Model
### State
The state is updated at each timestep depending on the cost calculated. MPC (Model Predicted Control) attempts to minimise the cost in order to control a system. If a minimum cost is obtained the actuators are triggered and the state updated.

The state variable is an array defining what the vehicle is doing and where it is in the world. This array contains: position x, postion y, orientation, and velocity. These parameters allow for the calculation and prediction of where the vehicle will be after a certain time when one knows what is happening currently.

### Actuators and Update Equations
The actuator array is used to control the vehicle. In this case it is the throttle and steering actuators. The throttle actuator is used also to brake the vehicle which can be done because the value passed to the actuator is between [-1, 1]. Where values larger than 0 is throttle and values smaller than 0 is brake.

The value passed to the steering actuator is also between [-1, 1] and relates to an angle of postive or negative 25 degrees.

The workflow followed to get a value to the actuators is as follows. At the start, data is obtained from the simulator representing x an y points. The points are translated relative to the vehicle's coordinate system so that the vehicle position relative to the points can be deterimined. These points are used to obtain a third order polynomial function. The cte (cross track error) and error in orientation (epsi) is calculated from the polynomial function in order to know how far off the vehicle is from the correct route. A state variable is set that is passed to the MPC funtion along with the coefficients of the polynomial function. Variable and actuator constraints are then set whereafter the bounding arrays are given to the ipopt optimizer.

The values returned from the optimizer function can then be used directly as the values for the steering angle (has to be taken to radians) and throttle. The process is then repeated for the next time step.

## Timestep Length and Elapsed Duration
The timestep length in this instance is 8 and the elapsed duration 0.1. The reason for using the value 8 is that it keeps the vehicle quite agile in places where there are sharp corners. The drawback is that the ride might be too fluctuating. However, in the case where this values is larger, such as 25, the vehicle starting correcting for corners to early which lead to large errors. Thus, for the selected speed and time constants the vehicle performs well.

## Latency
One of the methods to deal with latency is to set the parameters such that the vehicle does not react too abruptly to change but well enough to navigate the course. The model presented in thsi hand-in has a good trade-off between these two extremities. If after the latency period had passed the model recovers elegantly even though is some cases the difference between the state before and after latency is quite large.

## Final Video
Here is a video of the vehicle successfully completing a lap.

[![alt text][image1]](https://youtu.be/q8RFOOAbhjQ)
