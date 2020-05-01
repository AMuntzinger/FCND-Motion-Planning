## Project: 3D Motion Planning
![Quad Image](./misc/enroute.png)

---


# Required Steps for a Passing Submission:
1. Load the 2.5D map in the colliders.csv file describing the environment.
2. Discretize the environment into a grid or graph representation.
3. Define the start and goal locations.
4. Perform a search using A* or other search algorithm.
5. Use a collinearity test or ray tracing method (like Bresenham) to remove unnecessary waypoints.
6. Return waypoints in local ECEF coordinates (format for `self.all_waypoints` is [N, E, altitude, heading], where the drone’s start location corresponds to [0, 0, 0, 0].
7. Write it up.
8. Congratulations!  Your Done!

## [Rubric](https://review.udacity.com/#!/rubrics/1534/view) Points
### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  

You're reading it! Below I describe how I addressed each rubric point and where in my code each point is handled.

### Explain the Starter Code

#### 1. Explain the functionality of what's provided in `motion_planning.py` and `planning_utils.py`
These scripts contain a basic planning implementation that uses event driven programming. 
The main script `motion_planning.py` describes the different flight phases such as manual, arming, takeoff, waypoint etc. Also, it defines the callbacks to different events and the transitions between phases. 
`planning_utils.py` includes several functions, e.g. a function to create a grid from given 2D configuration space including obstacles, drone altitude and safety distance.
The basic actions and their costs are defined here as well as a check which actions are valid in a certain configuration. Finally, the script includes the A\* search algorithm implementation to find a valid path from start to goal. 

### Implementing Your Path Planning Algorithm

#### 1. Set your global home position
Here I read the first line of the csv file using `readline()`, extracted lat0 and lon0 as floating point values by splitting the first line accordingly and used the self.set_home_position() method to set global home. 


#### 2. Set your current local position
I converted the global position to the current local position using global_to_local().

#### 3. Set grid start position from local position
This is another step in adding flexibility to the start location. 
The north and east offsets are subtracted from the current local position instead of the local home position.

#### 4. Set grid goal position from geodetic coords
This step is to add flexibility to the desired goal location. 
The function is able to choose any (lat, lon) within the map and have it rendered to a goal location on the grid. 
I tried out different random target positions.

#### 5. Modify A* to include diagonal motion (or replace A* altogether)
I adapted the code in planning_utils() to update the A\* implementation. 
Now it includes diagonal motions on the grid that have a cost of sqrt(2).
Also, it is checked whether the diagonal moves are possible or not, if the corresponding locations lie outside the grid, the action is removed from the current list of valid actions.

#### 6. Cull waypoints 
For this step I used a collinearity test in order to prune your path of unnecessary waypoints. 
The function simply checks whether three subsequent points lie in a common line, to this end the determinant of the triangle is checked. 
If the determinant is zero, the points are collinear and the middle point can be removed.



### Execute the flight
#### 1. Does it work?
It works!

### Double check that you've met specifications for each of the [rubric](https://review.udacity.com/#!/rubrics/1534/view) points.


