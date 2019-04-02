# **Model Predictive Control (MPC)** 

### Writeup by Ernesto Cañibano

### This is the writeup associated with the fifth project of the Term 2, of the Self-driving Car Nanodegree


---

# Implementation

## The model
**Description of the model in detail, including the state, actuators and update equations.**
  
I used the simple kinematic model studied in the classroom. The model defines a 
state vector \[ x, y, &psi;, v, cte, e&psi; \] and the vehicle has two actuators \[ &delta;, *a* \] to control
its movement. The the description of all varibles are the following:
  * (*x*) vehicle coordinate x.
  * (*y*) vehicle coordinate y.
  * (&psi;) orientation of the vehicle.
  * (v) velocity of the vehicle.
  * (*cte*) cross track error.
  * (*e*&psi;) orientation error.
  * (&delta;) steering angle in the range *[-1, 1]*.
  * (*a*) acceleration in the range *[-1, 1]*.
  
Once known the state vector and the values of the actuators, it is possible predict the state of the vehicle
with the following equations:
  * *x<sub>t+1</sub> = x<sub>t</sub> + v<sub>t</sub> \* cos(&psi;<sub>t</sub>)  \* dt<br/>*
  * *y<sub>t+1</sub> = y<sub>t</sub> + v<sub>t</sub> \* sin(&psi;<sub>t</sub>)  \* dt<br/>*
  * *&psi;<sub>t+1</sub> = &psi;<sub>t</sub> + v<sub>t</sub> / Lf \*   &delta;<sub>t</sub> \* dt<br/>*
  * *v<sub>t+1</sub> = v<sub>t</sub> + a<sub>t</sub> \* dt<br/>*
  * *cte<sub>t+1</sub> = f(x<sub>t</sub>) - y<sub>t</sub> + v<sub>t</sub> \* sin(e&psi;<sub>t</sub>) \* dt<br/>*
  * *e&psi;<sub>t+t</sub> = &psi;<sub>t</sub> - &psi;des<sub>t</sub> + v<sub>t</sub> / Lf  \* &delta;<sub>t</sub> \* dt<br/>*

The functions *f(x)* and *&psi;des* are polynomial functions calculated from the waypoints.

Finally explain the **cost function** which is used for the MPC controller to select the best trajectory.
My cost confuction includes the following components:
  * Cross track error with a weight of 1.
  * Orientation error with a weight of 100.
  * Velocity error with a weight of 10.
  * Steering angle with a weight of 5.
  * Aceleration with a weight of 5.
  * Steering angle variations with a weight of 1000.
  * Aceleration variations with a weight of 1.


## Timestep Length and Elapsed Duration (N & dt)
  **Discussion the reasoning behind the chosen N (timestep lenth) and dt (elapsed time between timesteps) values.** 

  I set the the timestep lengt *N = 10* and elapsed duration *dt = 0.1*, it means that the trajectory is calculated
  each second. I tried with several values, and I found several values with which the predicted trajectory converged
  quickly with the polynomial fitted trajectory and driving was smooth. Among these values I chose the ones I think 
  generate a lower computational load.
  
## Polinomial Fitting and MPC Preprocessing
  **Description of how the polynomial is fited to waypoints and the preprocess of the waypoints, vehicle state and 
  actuators.**
  
  I transformed the received waypoints from global coordinate system to car coordiante system. I used the 
  transformation explained in the following [link](http://farside.ph.utexas.edu/teaching/336k/Newtonhtml/node153.html).
      
## Model Predicitive Control with Latency
  **Implementation of the MPC that handles a 100ms latency.** 
  
  I assumed a latency of 100ms in the vehicle actuators as suggested in project specifications. To compensate 
  the effect of this latency at first I calculate the initial state of the vehicle, after that using this 
  initial state and the model I predict the state of the vehicle after 100ms. I use this new state vector as
  input in the MPC controller.
  
# Simulation
  The vehicle must drive a lap around the track in the following terms:
  *  No tire may leave the drivable portion of the track surface. The car may not pop up onto ledges or roll over 
  The car may not pop up onto ledges or roll over any surfaces that would otherwise be considered unsafe (if humans 
  were in the vehicle).any surfaces that would otherwise be considered unsafe (if humans were in the vehicle).
  *  The car can't go over the curb, but, driving on the lines before the curb is ok. 
  
  <img src="videos/result.gif" width="480" alt="Output" />
  
  The finsl result can be seen in this [video](videos/result.mp4).
  



