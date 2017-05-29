# Refelction
---

Much of my initial code was based on mpc_to_line/solution/MPC.cpp from https://github.com/udacity/CarND-MPC-Quizzes.
I'll refer to this as "the MPC-Quiz".

## Model
Student describes their model in detail. This includes the state, actuators and update equations.
Xx


## Timestep Length and Elapsed Duration (N & dt)

First Variables
* My first versions had dt=0.05 and N=25. I started with these numbers because that's what the MPC-Quiz.

Second Variable
* I later shortened N to 10, in order to deal with latency.
* Xx

## Polynomial Fitting and MPC Preprocessing


## Model Predictive Control with Latency

Without Latency
* I first tuned the model without latency. I commented out the line ```this_thread::sleep_for(chrono::milliseconds(100));```. * This allowed me to focus on making sure the general code and model worked, that I was transforming data points correctly, and that I could output debug data.

With Latency
* I then added back in the latency.
* The car would oscilated slowly and then uncontrollablly as it moved down the track. The yellow projected line of the path was also displaced. I assumed this was from the model trying to work with data that was late.
* I first changed the x-position of the input state of the model from 0 to v*0.1. Xx
* Xx
