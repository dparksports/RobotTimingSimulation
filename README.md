# Robot Schedule Simulation

# What it does
1) This measures how long a robot will take to complete a circuit of 1000 or more assigned nodes.
2) It also estimates the minimum amount of time it takes to complete a circuit.
3) Uses 3 input files:
  - nodes_input.csv: describes 1000 or more node types.
  - paths_input.csv: describes 1000 or more assigned nodes to be visited by each robot.
  - robots_input.csv: described 4 or more robot types and its speed

# Capabilities
1) It schedules more than 1000 assigned nodes to be visited by each robot. For example, it can handle 2000 assigned nodes per circuit, instead of 1000 default count.  
2) It schedules more than 4 robots.  For example, it can handle 10 robots, instead of 4 default count. 
3) It does a real time simulation.  For example, the default setup of 1000 nodes per robot with 4 robots takes about 50 hours.
4) It estimates the minimum run time, without running a real time simulation.
5) It reduces a 50-hour simulation to a 1 minute run time, and still able to complete the entire 50 hour simulation, in hyper, real time.  This default real time simulation is with 1000 nodes per robot with 4 robots.  
7) It handles a different count of assigned nodes per circuit, of multiple circuits.
8) And more!

# Collision avoidance
- Multiple robots may visit the same node at the same time.
- To avoid an incident of collision, each robot uses an assigned billboard which shows the currently visiting node.
- For example, when a robot visits a node, it marks the node number in its billboard.
- When another robot visits the same node, it is guarded by a "lock_guard" by a "mutex".
- When the first robot exits the current node, after completing an assigned task on the current node, the lock guard automatically releases the mutex in a function scope.

# Collision avoidance - Technical
- This is done by having the first visiting robot to exclusively lock the assigned node at the functional scope of 'reserveBillboard' function, and releasing the exclusive lock, when it leaves the function scope.
- When the mutex is released, any waiting robots on the node will resume, in order of arrival.
- When multiple robots arrive at the same node at the same time, the first robot to arrive will catch the flag, or the mutex, and will perform its assigned task.

# Steps to build the app.
```markdown
1. git clone git@github.com:dparksports/RobotTimingSimulation.git
2. cd RobotTimingSimulation
3. mkdir build;cd build
4. cmake..
5. make
6. ./robot 
```

# Output files
```markdown
- visited.csv: a list of nodes tagged with visited robot IDs, in order.
- measuredtimes.csv: a list of measured time of each circuit by each robot ID, in seconds.
```

# Output Log
```markdown
- The output logs each node visited by each robot, its task and its run time.
- This calculates a minium estimated travel time for each robot.
- This verifies all assigned nodes in order of each circuit to be visited by a robot.
```

# Timing scale
- To simulate the actual time in real time, change the current millisecond timing resolution to 'second timing resolution' like this in code.
        std::this_thread::sleep_for(std::chrono::second(travelTime));
- For example, simulating in this robot simulation in real time, in seconds, will take about 50 hours for 1000 assigned nodes per each robot, of all 4 robots.
- The current setup optimizes the real time simulation, by reducing the entire run time of about 50 hours to about 1 minute.

# Robot run per thread
- Uses a detached thread per a robot run.
- Gated by sleep_for with 60 seconds to catch all thread completion.
- This should be increased to a large wait time, if used in non-optimized, second timing resolution.
 
# Verification
- When I get a chance, need to add some form of verification and unit tests.
- Currently, although none of formal verification is added, one may use the generating output log to validate all assigned nodes are read in correctly and visited correctly in order and to validate the total time by verifying the currently accrued run time per robot.

# Default Setup
- Robots: 2 Types: Mover, Organizer
- Mover Robot Tasks: Push, Pull
- Organizer Robot Tasks: Pick, Place
- Robot default count: 4
- Node default count per circuit: 1000
- Actual measured time for 1000 nodes/circuit with 4 robots: ~50 hours
- Optimized measured time for 1000 nodes/circuit with 4 robots: ~1 minute

# Wish List
- When I get a chance, I will add some comment.  ;)
- Refactor one file into multiple classes to keep it simple.
- Refactor the gating code of sleep_for with something else nice.

# License
- See the license file for details.
- Copyright 2020, Dan Park, all rights reserved.

