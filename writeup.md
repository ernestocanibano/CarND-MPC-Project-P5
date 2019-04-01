# **Model Predictive Control (MPC)** 

### Writeup by Ernesto Cañibano

### This is the writeup associated with the fifth project of the Term 2, of the Self-driving Car Nanodegree


---

## Implementation

### The model
  Description of the model in detail, including the state, actuators and update equations.


* **Proportional component Kp.**

  Proportional controller only receives as input the current cross-track error (CTE). 
  The ouput of the controller is the steering angle of the car to try to correcto the CTE. 
  In this way the controller tries to keep the car in a reference trajectory.
  
  In the following videos, it can be observed the effect of the proportional term in the trajectory of the car.
  In both of them the components derivative and integral has been canceled (**Kd = 0**, **Ki = 0**).
  *  [**Kp = 1.0**](videos/kp=1.0_ki=0.0_kd=0.0.mp4) With a big value of the proportional the car responses very fast
  to try to correct the CTE, but the overshooting produces a big oscillation effect even before to reach the first turn. 
  *  [**kp = 0.01**](videos/kp=0.01_ki=0.0_kd=0.0.mp4) If proportional component is too small, the car doesn't oscillate
  too much on the straight part of the track, but it is unable to keep the car on the track as soon the first turn arrives.
 
  The main problem of the proportional controller is the overshooting which generates an oscillation in the trajectory
  of the vehicle. It is not possible to keep the car on track only with the proportional component.



### Timestep Length and Elapsed Duration (N & dt)
  Discussion the reasoning behind the chosen N (timestep lenth) and dt (elapsed time between timesteps) values. 

 
  
  1. Set **Kd = 0** and **Ki = 0**.
  2. Try different values for **Kp** until achieve the car response to CTE with a steady oscillation. 
  I started with **Kp = 0.01** and increasing it.
  3. Next step is try values for **Kd**. I started with **Kp = 0.5** and increased it until the oscillation disappeared.
  4. Test the controller driving all the track. If the car left the ciruit go to step 2 again and increment **Kp**. 
  If the car finish all the circuit go to step 2 deacreasing **Kp**. Repeat the proccess until the behaviour is as smooth
  as possible.
  5. Try with different values of **Ki** util get complete the track with the minimum number of oscillations. I started
  with **ki = 0.05** and then i decrase it. 
  
### Polinomial Fitting and MPC Preprocessing
  Description of how the polynomial is fited to waypoints and the preprocess of the waypoints, vehicle state and 
  actuators.
  
### Model Predicitive Control with Latency
  Implementation of the MPC that handles a 100ms latency. 
  
## Simulation
  The vehicle must drive a lap around the track in the following terms:
  *  No tire may leave the drivable portion of the track surface. The car may not pop up onto ledges or roll over 
  The car may not pop up onto ledges or roll over any surfaces that would otherwise be considered unsafe (if humans 
  were in the vehicle).any surfaces that would otherwise be considered unsafe (if humans were in the vehicle).
  *  The car can't go over the curb, but, driving on the lines before the curb is ok. 
  
  <img src="videos/result.gif" width="480" alt="Output" />

  
  



