# Discrete Automation PLC Programming for an Industrial Scenario

## Description of the Task

The system shown in the figure below is used to fill up containers.

<a href="https://drive.google.com/uc?export=view&id=1aarhBj7RD-nfBF1z0rUAgmfLjwDOIYPn"><img src="https://drive.google.com/uc?export=view&id=1aarhBj7RD-nfBF1z0rUAgmfLjwDOIYPn" style="width: 500px; max-width: 100%; height: auto" title="Click for the larger version." /></a>

The system is composed of a conveyor and a tank. The conveyor is used to deliver empty containers under the tank. The container should stop under the tank to get filled. Once filled, the container is transported out of the system. The tank has capacity to fill in 10 containers, after which system should stop until the tank is refiled to serve new containers.

The system should have at least following outputs:
* Motor - the motor to control the conveyor.
* Valve - to open for filling in the container.
* Tank_empty_out - the led to signal that the tank got empty.


The system should have at least the following inputs:
* Container_in_place - the container is under the tank in the filling position (probably a good moment to stop the conveyor for filling up the container).
* Container_full - sensor telling when the container is full (probably the good way to decide when the valve of the tank should be opened/closed).
* Tank_empty_in - sensor telling when the tank is empty (probably a good moment to signal with the led (Tank_empty_out) the operator about * the situation).
* Loading - just a button to signal the input of the container.
* Unloading - just a button to signal the removal of the container from the system.
* START - a button to tell system to run.
* STOP - a button to stop the system.
* EMERGENCY - a button to give an emergency signal (system should get into the safety mode: valve closed, motor turned off.)

## Programming Environment
* Language: Structured Text (IEC-61131-C)
* IDE: TwinCat (Beckhoff)

----------------------------------------------------------------------------------------------------------------------------------------


# Solution

The Visualization of the final implementation is shown below.

<a href="https://drive.google.com/uc?export=view&id=1KsdO_-8cuT0hCNpDX6GT61C0e_1oARV4"><img src="https://drive.google.com/uc?export=view&id=1KsdO_-8cuT0hCNpDX6GT61C0e_1oARV4" style="width: 500px; max-width: 100%; height: auto" title="Click for the larger version." /></a>

We can differentiate 5 clear states in our system:
1. An initial State 0, in which the system does nothing at all. Takes place before pressing the START button.
2. A first state, in which the system has already been powered and is waiting for a bucket to hit the conveyor belt.
3. A second state, in which the bucket advances towards the filling valve.
4. A third state, in which the bucket gets filled.
5. A fourth state, in which the already filled bucket advances towards the end of the
conveyor.

### State Chart Diagram

<a href="https://drive.google.com/uc?export=view&id=1rqOvMDzZP5T6W7KTR_hKiBV6NoN9lkE_"><img src="https://drive.google.com/uc?export=view&id=1rqOvMDzZP5T6W7KTR_hKiBV6NoN9lkE_" style="width: 500px; max-width: 100%; height: auto" title="Click for the larger version." /></a>

Apart from this straightforward process, at any point we are capable of pressing the STOP button, which makes the system stop after the current bucket is filled and until we press the start button again, and the EMERGENCY button, which turns momentarily all the outputs of the system (Valve and Motor).

It is necessary to highlight that the Tank_empty_in is a normally closed contact, in such a way that it outputs the value FALSE when it senses fluid in the tank and TRUE when we have run out of it. The possibility in which the tank was not filled to its maximum capacity at the beginning of the process should be also considered. This implies that it can run out of liquid before 10 buckets. Due to this, I have included a Refill Tank button, that would fill the tank and therefore set the bucket counter down to zero again.
