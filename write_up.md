## Writeup
---
**PID Controller**

---
References:
Udacity starter code provided in the tutorials was used throughout and the Q and A https://www.youtube.com/watch?v=YamBuzDjrs8&feature=youtu.be

### PID controller

A PID controllor refers to a control system that contains a proportional, derivative and integral component. 

In this project a PID controller is used to control the car's steering angle whilst driving round a simulated track. The cross track error is used as the imput error to the controller. The aim is to drive the car around the track with controller adjusting the steering angle based on the cross track error. 

Proportional: The proportional part of the controller adjusts the steering angle in proportion to the cross track error. This means that the larger the cross track error the sharper the turning angle. A proportional controller in isolation is prone to overshooting and therefore oscillating around the target location.This is because until the target is met there will always be some steering and by the time this goes to zero, because the car is moving it will have overshot. 

Derviative: This adds a component that accounts for the rate of change of the cross track error. This allows the controller to remove the oscillations from the pure proportional controller. 

Integral: The integral part looks at the cumulative error to identify any bias in the system that causes the proportional and derivative controllers to settle at a control point that is not optimal. For this project this would only be true if there was a bias in the steering angle. This would become apparent as the car would drive constantly off center from the middle of the track. This doesn't seem to be an issue so this aspect doesn't play a role in this solution. 


### Implementation 

The implementation is simply the implementation of the combination of the proportional, integral and derivative components. 

As the timestep was 1, the derviate error could be calculated as simply the difference between the current and last cross track error.
```python
  d_error = cte - p_error;
  p_error = cte;
  i_error += cte;
  double error = Kp*p_error + Kd*d_error + Ki*i_error;
```
### Hyperparameter Tuning
The proportional, derivative and integral components of the controller are all multiplied by a coefficient in the implementation. These need tuning to fine their optimal. 
Although different options are available a adequate solution could be found by manually testing different options. Initially only the proportional element was added, causing the car to oscillate. Adding the derivative term helped to reduce this. The proportional term was then decreased until the car could drive around the track.

Some integral control was added but this made the performance worse and so was removed. The final coefficients were

```python
double init_Kp = -0.1 ;
double init_Ki = -0 ;
double init_Kd = -0.8 ;
```