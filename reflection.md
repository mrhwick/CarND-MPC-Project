## The Model

I used the model that was illustrated in both the lesson and the walkthrough video provided by the instructors. This model is a simple kinematic model of position, heading, and velocity. The model also includes the cross-track error and the error in heading.

The model's state equation is as follows:

![image](https://user-images.githubusercontent.com/865759/32816381-66df9a90-c986-11e7-8c19-3cf9a2aaa689.png)

## Timestep Length & Elapsed Duration

I chose the timestep length 10 as a good number of steps to predict over for accuracy reasons. When I increased the number of steps, the model began making awkward preditions around the tighter curves of the track. Running on my simulator, N = 10 allowed the MPC to make good predictions about the upcoming track changes without overcompensating to portions of the track that weren't relevant. When I ran the MPC with a higher value, N = 20 for example the MPC gave the vehicle's actuators extreme instructions. This caused the car to swerve back and forth across the track on one of the tighter turns. Smaller values for N allowed the MPC to make accurate predictions over a reasonable number of points to keep the car on the road.

I chose 0.1 as the elapsed duration because that matches the amount of latency that the MPC was experiencing in this assignment. I essentially wanted to predict states at the time steps in which the MPC would receive control of the vehicle again. That means that basing the "dt" value on the latency would generate N number of steps spread across N number of future checkpoints with latency.

## Polynomial Fitting & MPC Processing

I used a 3rd order polynomial to fit the future waypoints as presented in the classroom exercises. Higher order polynomials allow the model to represent curves that don't simply require a single steering angle. For example, a curve in the shape of an S would require a 3rd order or higher polynomial in order to represent the left & right turns necessary to navigate it.

## Latency Handling

I adjusted the first point to be used in the MPC solving to account for latency. This was achieve by assuming 100ms latency, and using the kinematic model to predict where the car would be when the values arrive for processing. Extrapolating the values provided 100ms into the future creates a good enough approximation of where the car is at the time processing begins.

I attempt to adjust all state variables for latency.
