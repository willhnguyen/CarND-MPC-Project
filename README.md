# MPC Controller Project
Self-Driving Car Engineer Nanodegree

This project aims to implement an MPC (Model Predictive Control) controller to set a car to drive itself around a race track.

## Dependencies
* cmake >= 3.5
  * All OSes: [click here for installation instructions](https://cmake.org/install/)
* make >= 4.1 (Linux, Mac), 3.81 (Windows)
  * Linux: make is installed by default on most Linux distros
  * Mac: [install Xcode command line tools to get make](https://developer.apple.com/xcode/features/)
  * Windows: [Click here for installation instructions](http://gnuwin32.sourceforge.net/packages/make.htm)
* gcc/g++ >= 5.4
  * Linux: gcc / g++ is installed by default on most Linux distros
  * Mac: same deal as make - [install Xcode command line tools](https://developer.apple.com/xcode/features/)
  * Windows: recommend using [MinGW](http://www.mingw.org/)
* **Ipopt and CppAD:** Please refer to [this document](https://github.com/udacity/CarND-MPC-Project/blob/master/install_Ipopt_CppAD.md) for installation instructions.

## Build and Run Instructions
To build the program, run the following sequence of command line instructions.

```bash
$ cd /path/to/cloned/repo
$ mkdir build
$ cd build
$ cmake ..
$ make
```

To see the program in action, make sure to download the [Term 2 simulator](https://github.com/udacity/self-driving-car-sim/releases), run it, and choose the third simulation option. Then, execute the program by executing the `mpc` program in the `build` folder.

```bash
$ ./mpc
```

## Project Information
Like the PID controller, model predictive control (MPC) aims to provide a car the means to drive itself. The difference, however, is that MPC simulates a series of actuator controls (e.g. steering and throttle) to determine which actuator control parameters provide the lowest error.

The first step of the MPC algorithm is to fit the trajectory to a line of best fit. It is usually fitted a 3rd order polynomial.

Once a trajectory has been determined, the MPC costs needs to be defined. This is done by coding all the parameters we want the MPC to consider.

For starters, this includes the CTE and EPsi (heading angle error) which identifies the state of the vehicle that directly matters. However, these alone are not enough because it can mean that a car that does not move but is on the trajectory is enough to have 0 CTE and 0 EPsi. To ensure the car is constantly moving, the difference between the current velocity and a desired velocity can be added to the cost.

The actuators can also be included in the cost terms to decrease the amount of actuator usage. Although the actuators should be used, overusing them is not desired.

As learned from the PID project, the differential term can help the car transition smoothly. In this case, the differential term will be used for the actuators to ensure the car slowly amps up or eases off its usage of the actuators rather than perform sudden changes.

The costs calculations also include the model constraints. This portion checks that the model behaves as expected over each time step.

## Project Implementation Details
The `src/mpc.cpp`, `src/mpc.h`, and `src/main.cpp` files were modified in this project.

In `src/mpc.cpp`, the `f_eval()` function defines the cost constraints that should be considered. The `Solve()` function defines how the MPC should determine the set of actuators to use and returns those actuator values.

The `src/main.cpp` file was updated to include initial state calculations and latency consideration. Ultimately, the values from the MPC controller object is passed to the simulator.

`src/mpc.h` was updated to address compilation issues due to `using namespace std` occuring before the JSON library import in `src/main.cpp`.
