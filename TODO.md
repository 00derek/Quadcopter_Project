The performance of the agent is very sensitive to the reward function.
I would like to request you to try the following suggestions for the reward function for the takeoff task.

We want minimum change in direction of x and y and making z close to the target z, so we punish for moving in directions x and y and give rewards to direction in z.

# punish movement in x direction
punish_x = abs(self.sim.pose[0] - self.target_pos[0])

# punish movement in y direction
punish_y = abs(self.sim.pose[1] - self.target_pos[1])

# give reward in z direction
Zdist = abs(self.target_pos[2]-init_pose[2])
reward_z = 1.0 - (self.target_pos[2] - self.sim.pose[2])/self.Zdist
The task takeoff also requires that the agent stays stable and does not rotate. To make this possible, we punish the rotation of the agent along all the axis.
punish_rot1 = abs(self.sim.pose[3])
punish_rot2 = abs(self.sim.pose[4])
punish_rot3 = abs(self.sim.pose[5])
Finally the above two points can be combined into a single reward and returned.
reward = reward_z - 0.1 * (punish_x + punish_y) - 0.1 * (punish_rot1 + punish_rot2 + punish_rot3)
It is requested to experiment with the weights assigned to the punishments in x and y directions and the rotations. Both of them being 0.1 in this case.
The above reward function should help the agent learn better. And I hope the above reward function helps you in coming up with more reward functions and experiment.

Check out the link:
https://bons.ai/blog/deep-reinforcement-learning-models-tips-tricks-for-writing-reward-functions


=========
If you try the hover task, the following suggestion should be very useful.
To make the agent hover it could mean to make the quadcopter fly close to the target height. Positive reward could be given when the agent flies within a range of, let's say, 2 meters above and below the target height. And negative reward if the agent crosses that limit. This should help the agent to fly near the target height.


