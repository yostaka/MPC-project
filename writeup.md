**The model in use**

State of the vehicle consists of:
* x: horizontal position of the vehicle
* y: vertical position of the vehicle
* psi: vehicle orientation angle
* v: velocity of the vehicle
* cte: cross track error indicating how far the vehicle position from a reference trajectory
* epsi: error in vehicle orientation ange to the angle on a reference trajectory

Actuators in the model consist of:
* steer_value(delta): steering angle of the vehicle
* throttle_value(a): throttle to manipulate vehicle acceleration

Followings are the update equations for each state:
* x1 = x0 + v0 * cos(psi) * dt
* y1 = y0 + v0 * sin(psi) * dt
* psi1 = psi0 - v0 * delta0 / Lf * dt
* v1 = v0 + a0 * dt
* cte1 = (f0 - y0) + (v0 * sin(epsi0) * dt)
* epsi1 = (psi0 - psides0) - v0 * delta0 / Lf * dt

Note f0 and psides0 used in the equations aboave are calculated as follows:
* f0 = coeffs[0] + coeffs[1] * x0 + coeffs[2] * x0 * x0 + coeffs[3] * x0 * x0 * x0
* psides0 = alan(3*coeffs[3]*x0*x0 + 2*coeffs[2]*x0 + coeffs[1])

where coeffs are coefficients derived with polyfit


**Timestep Length and Elapsed Duration(N & dt)**

I finally picked N = 10 and dt = 0.1.

I started to use N = 25 and dt = 0.05 which are the same values used in the quiz. The vehicle is driving widingly, and I think it is because small dt enforces steep steering and it makes overshoot.

Then, I picked N = 25 and dt = 1.0 and the vehicle drives smoothly, and it looks almost fine. But sometimes the vehicle go off from the road, and it seemed that the computation was not done in time and caused the behavior.

Finally, I picked N = 10 and dt = 1.0 and it worked fine.


**Preprocess of waypoints**

I implemented preprocess of waypoints so remaining calculation is to be more readable and intuitive.

* First, waypoints are shifted as (0, 0) is to be vehicle position
* Next, waypoints are rotated by psi (vehicle orientation angle) so horizontal axis(x-axis) is to be car orientation angle
