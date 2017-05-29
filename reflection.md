# Refelction
---

Much of my initial code was based on mpc_to_line/solution/MPC.cpp from https://github.com/udacity/CarND-MPC-Quizzes.
I'll refer to this as "the MPC-Quiz".

## Model
The model was the same as the one descibed in the lessons.

State:
* Included vehicle position (x & y), vehicle angle (psi), vehicle velocity (v), cross track error (cte) and the angle error (epsi)

Actuators:
* Includes steering angle and acclerator/throttle value.

Update Equations:
* These are the same from Lesson 18 (kinematic model and error equations).


## Timestep Length and Elapsed Duration (N & dt)

First Variables
* My first versions had dt=0.05 and N=25. I started with these numbers because that's what the MPC-Quiz.

Second Variable
* I later shortened N to 10, in order to deal with latency. This is explained more in the section on "Model Predictive Control with Latency".

## Polynomial Fitting and MPC Preprocessing

Preprocessing - Conversion to Vehicle Coordinates
* We convert the road points from map coordinate system to vehicle coordinate system. 
* This was necessary to plot the yellow and blue lines. It also makes numbers smaller, which should make optimization more accurate.

Polynomial Fitting
* I then fit the road coordinates to a 3rd order polynomial. I picked 3rd order, because that's what we used in the lessons.

## Model Predictive Control with Latency

Without Latency
* I first tuned the model without latency. I commented out the line ```this_thread::sleep_for(chrono::milliseconds(100));```. * This allowed me to focus on making sure the general code and model worked, that I was transforming data points correctly, and that I could output debug data.

With Latency
* I then added back in the latency.
* The car would oscilated slowly and then uncontrollablly as it moved down the track. The yellow projected line of the path was also displaced. I assumed this was from the model trying to work with data that was late.
* First, I changed the x-position of the input state of the model from 0 to v*0.1. This was to estimate the true position of the vehicle when the data was recieved. Without latency, the vehicle position - in the vehicle frame - is 0. But we then extrapalate the vehicle linearly 100ms (0.1 sec) at the current velocity, to get a sense of where the vehicle really is.
  * This got rid of some of the oscillation, but not all.
* Next, I shortened the N (number of timestemps to compute) from 25 to 10. This got rid of the rest of the oscillations. I suspect this worked, because the optimizer had more time to fully optimize the path. The ipopt solver's max cpu time was set to 0.5; If it takes longer than that to do a full optimization, the solver would stop. By lowering the number of optimizations needed, we make sure the optimizer has a chance to do a good job.
