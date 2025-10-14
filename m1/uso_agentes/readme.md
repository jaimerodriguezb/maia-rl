Analisis pacman

Analisis 1
################################################################################################
> Understand the code and find where are the statuses, actions and rewards. 

● I'll help you understand the code and locate where the states, actions, and rewards are defined in this Pacman reinforcement
  learning project.

  Let me start by exploring the codebase structure and then examining the key files.

● Search(pattern: "**/*.py")
  ⎿  Found 85 files (ctrl+o to expand)

● Read(Reinforcement\qlearningAgents.py)
  ⎿  Read 235 lines

● Read(Reinforcement\learningAgents.py)
  ⎿  Read 259 lines

● Now let me read more key files to understand the complete picture:

● Read(Reinforcement\game.py)
  ⎿  Read 150 lines

● Read(Reinforcement\environment.py)
  ⎿  Read 57 lines

● Read(Reinforcement\featureExtractors.py)
  ⎿  Read 104 lines

● Let me also check the pacman.py file to see how the game is executed:

● Read(Reinforcement\pacman.py)
  ⎿  Read 200 lines

● Search(pattern: "getScore|reward", path: "Reinforcement", output_mode: "content", head_limit: 30)
  ⎿  Found 30 lines (ctrl+o to expand)

● Based on my analysis of the code, I can now provide you with a comprehensive explanation of where states, actions, and rewards
  are defined in this Pacman reinforcement learning project.

  States, Actions, and Rewards in the Pacman RL Project

  States

  States are represented by the GameState class in Reinforcement\pacman.py. A state contains:

  Location: Reinforcement\pacman.py:61-177

  Key components:
  - Pacman position: getPacmanPosition() - returns (x,y) coordinates
  - Ghost positions: getGhostPositions() - list of ghost (x,y) positions
  - Food grid: getFood() - boolean grid indicating food locations
  - Walls: getWalls() - boolean grid indicating wall locations
  - Capsules: getCapsules() - list of power pellet positions
  - Score: getScore() - current game score
  - Game status: isWin(), isLose() - terminal state indicators

  The state is accessible in Q-learning through the state parameter passed to various methods. In the learning process, states are
   represented as the full GameState object which can be hashed and used as dictionary keys.

  ---
  Actions

  Actions are defined in Reinforcement\game.py:53-73 as directional movements:

  Location: Reinforcement\game.py:53-58

  class Directions:
      NORTH = 'North'
      SOUTH = 'South'
      EAST = 'East'
      WEST = 'West'
      STOP = 'Stop'

  How actions are obtained:
  - getLegalActions(state) - returns list of legal actions for current state
    - In learningAgents.py:116-122
    - In qlearningAgents.py:38-39
  - Actions are selected using getAction(state) which implements epsilon-greedy exploration

  Action selection in qlearningAgents.py:102-127:
  - With probability epsilon: random action (exploration)
  - With probability 1-epsilon: best action based on Q-values (exploitation)

  ---
  Rewards

  Rewards are computed as score differences between states:

  Primary reward calculation in learningAgents.py:212:
  reward = state.getScore() - self.lastState.getScore()

  Reward sources defined in Reinforcement\pacman.py:

  1. Food consumption (pacman.py:381):
    - +10 points per food pellet eaten
  2. Winning (pacman.py:388):
    - +500 points for eating all food
  3. Eating scared ghost (pacman.py:462):
    - +200 points
  4. Death by ghost (pacman.py:469):
    - -500 points
  5. Time penalty (pacman.py:122, pacman.py:274):
    - -1 point per move (encourages efficiency)

  Reward flow:
  1. Agent takes action in state
  2. Environment updates via observeTransition() (learningAgents.py:124-133)
  3. Reward computed as score delta
  4. update(state, action, nextState, reward) called to update Q-values

  ---
  Q-Learning Update

  The Q-value update happens in qlearningAgents.py:129-142:

  def update(self, state, action, nextState, reward):
      self.qValues[(state, action)] = (1-self.alpha) * self.getQValue(state, action) + \
                                       self.alpha * (reward + self.discount * self.computeValueFromQValues(nextState))

  This implements the standard Q-learning formula:
  Q(s,a) ← (1-α)Q(s,a) + α[r + γ·max_a' Q(s',a')]

  Where:
  - alpha = learning rate
  - gamma (stored as discount) = discount factor
  - reward = immediate reward from score difference
  - nextState = resulting state after action

  ---
  Feature-Based Approximation

  For approximate Q-learning (ApproximateQAgent), features are extracted in featureExtractors.py:66-103:

  SimpleExtractor features:
  - bias: constant 1.0
  - #-of-ghosts-1-step-away: number of nearby ghosts
  - eats-food: 1.0 if action eats food
  - closest-food: normalized distance to nearest food

  These features allow learning with function approximation instead of storing explicit Q-values for every (state, action) pair.
################################################################################################






AGRESIVO
================================================================================================
python pacman.py -p PacmanQAgent -x 2000 -n 2010 -l smallGrid -a epsilon=0.8,alpha=0.8,gamma=0.2
Beginning 2000 episodes of Training
Reinforcement Learning Status:
        Completed 100 out of 2000 training episodes
        Average Rewards over all training: -509.35
        Average Rewards for last 100 episodes: -509.35
        Episode took 0.70 seconds
Reinforcement Learning Status:
        Completed 200 out of 2000 training episodes
        Average Rewards over all training: -509.60
        Average Rewards for last 100 episodes: -509.84
        Episode took 0.73 seconds
Reinforcement Learning Status:
        Completed 300 out of 2000 training episodes
        Average Rewards over all training: -509.46
        Average Rewards for last 100 episodes: -509.20
        Episode took 0.73 seconds
Reinforcement Learning Status:
        Completed 400 out of 2000 training episodes
        Average Rewards over all training: -509.45
        Average Rewards for last 100 episodes: -509.41
        Episode took 0.66 seconds
Reinforcement Learning Status:
        Completed 500 out of 2000 training episodes
        Average Rewards over all training: -509.58
        Average Rewards for last 100 episodes: -510.11
        Episode took 0.82 seconds
Reinforcement Learning Status:
        Completed 600 out of 2000 training episodes
        Average Rewards over all training: -509.58
        Average Rewards for last 100 episodes: -509.57
        Episode took 0.82 seconds
Reinforcement Learning Status:
        Completed 700 out of 2000 training episodes
        Average Rewards over all training: -509.60
        Average Rewards for last 100 episodes: -509.71
        Episode took 0.80 seconds
Reinforcement Learning Status:
        Completed 800 out of 2000 training episodes
        Average Rewards over all training: -509.62
        Average Rewards for last 100 episodes: -509.77
        Episode took 0.74 seconds
Reinforcement Learning Status:
        Completed 900 out of 2000 training episodes
        Average Rewards over all training: -509.63
        Average Rewards for last 100 episodes: -509.69
        Episode took 0.79 seconds
Reinforcement Learning Status:
        Completed 1000 out of 2000 training episodes
        Average Rewards over all training: -509.51
        Average Rewards for last 100 episodes: -508.42
        Episode took 0.77 seconds
Reinforcement Learning Status:
        Completed 1100 out of 2000 training episodes
        Average Rewards over all training: -508.67
        Average Rewards for last 100 episodes: -500.29
        Episode took 0.85 seconds
Reinforcement Learning Status:
        Completed 1200 out of 2000 training episodes
        Average Rewards over all training: -508.72
        Average Rewards for last 100 episodes: -509.27
        Episode took 0.81 seconds
Reinforcement Learning Status:
        Completed 1300 out of 2000 training episodes
        Average Rewards over all training: -508.80
        Average Rewards for last 100 episodes: -509.79
        Episode took 0.81 seconds
Reinforcement Learning Status:
        Completed 1400 out of 2000 training episodes
        Average Rewards over all training: -508.84
        Average Rewards for last 100 episodes: -509.33
        Episode took 0.78 seconds
Reinforcement Learning Status:
        Completed 1500 out of 2000 training episodes
        Average Rewards over all training: -508.85
        Average Rewards for last 100 episodes: -509.05
        Episode took 0.77 seconds
Reinforcement Learning Status:
        Completed 1600 out of 2000 training episodes
        Average Rewards over all training: -508.88
        Average Rewards for last 100 episodes: -509.31
        Episode took 0.77 seconds
Reinforcement Learning Status:
        Completed 1700 out of 2000 training episodes
        Average Rewards over all training: -508.90
        Average Rewards for last 100 episodes: -509.20
        Episode took 0.77 seconds
Reinforcement Learning Status:
        Completed 1800 out of 2000 training episodes
        Average Rewards over all training: -508.94
        Average Rewards for last 100 episodes: -509.70
        Episode took 0.78 seconds
Reinforcement Learning Status:
        Completed 1900 out of 2000 training episodes
        Average Rewards over all training: -508.45
        Average Rewards for last 100 episodes: -499.45
        Episode took 0.83 seconds
Reinforcement Learning Status:
        Completed 2000 out of 2000 training episodes
        Average Rewards over all training: -508.49
        Average Rewards for last 100 episodes: -509.29
        Episode took 0.87 seconds
Training Done (turning off epsilon and alpha)
---------------------------------------------
Pacman died! Score: -520
Pacman died! Score: -505
Pacman died! Score: -509
Pacman died! Score: -504
Pacman died! Score: -508
Pacman died! Score: -511
Pacman died! Score: -505
Pacman died! Score: -504
Pacman died! Score: -504
Pacman died! Score: -511
Average Score: -508.1
Scores:        -520.0, -505.0, -509.0, -504.0, -508.0, -511.0, -505.0, -504.0, -504.0, -511.0
Win Rate:      0/10 (0.00)
Record:        Loss, Loss, Loss, Loss, Loss, Loss, Loss, Loss, Loss, Loss

CONSERVADOR
=========================================================================
> python pacman.py -p PacmanQAgent -x 2000 -n 2010 -l smallGrid -a epsilon=0.2,alpha=0.2,gamma=0.8
Beginning 2000 episodes of Training
Reinforcement Learning Status:
        Completed 100 out of 2000 training episodes
        Average Rewards over all training: -510.17
        Average Rewards for last 100 episodes: -510.17
        Episode took 0.88 seconds
Reinforcement Learning Status:
        Completed 200 out of 2000 training episodes
        Average Rewards over all training: -510.54
        Average Rewards for last 100 episodes: -510.90
        Episode took 1.11 seconds
Reinforcement Learning Status:
        Completed 300 out of 2000 training episodes
        Average Rewards over all training: -510.98
        Average Rewards for last 100 episodes: -511.86
        Episode took 1.21 seconds
Reinforcement Learning Status:
        Completed 400 out of 2000 training episodes
        Average Rewards over all training: -511.00
        Average Rewards for last 100 episodes: -511.07
        Episode took 1.15 seconds
Reinforcement Learning Status:
        Completed 500 out of 2000 training episodes
        Average Rewards over all training: -511.22
        Average Rewards for last 100 episodes: -512.12
        Episode took 1.31 seconds
Reinforcement Learning Status:
        Completed 600 out of 2000 training episodes
        Average Rewards over all training: -499.19
        Average Rewards for last 100 episodes: -439.04
        Episode took 1.16 seconds
Reinforcement Learning Status:
        Completed 700 out of 2000 training episodes
        Average Rewards over all training: -499.14
        Average Rewards for last 100 episodes: -498.85
        Episode took 1.31 seconds
Reinforcement Learning Status:
        Completed 800 out of 2000 training episodes
        Average Rewards over all training: -495.68
        Average Rewards for last 100 episodes: -471.43
        Episode took 1.39 seconds
Reinforcement Learning Status:
        Completed 900 out of 2000 training episodes
        Average Rewards over all training: -488.38
        Average Rewards for last 100 episodes: -429.98
        Episode took 1.41 seconds
Reinforcement Learning Status:
        Completed 1000 out of 2000 training episodes
        Average Rewards over all training: -486.49
        Average Rewards for last 100 episodes: -469.43
        Episode took 1.28 seconds
Reinforcement Learning Status:
        Completed 1100 out of 2000 training episodes
        Average Rewards over all training: -478.51
        Average Rewards for last 100 episodes: -398.76
        Episode took 1.44 seconds
Reinforcement Learning Status:
        Completed 1200 out of 2000 training episodes
        Average Rewards over all training: -467.71
        Average Rewards for last 100 episodes: -348.96
        Episode took 1.54 seconds
Reinforcement Learning Status:
        Completed 1300 out of 2000 training episodes
        Average Rewards over all training: -457.10
        Average Rewards for last 100 episodes: -329.74
        Episode took 1.66 seconds
Reinforcement Learning Status:
        Completed 1400 out of 2000 training episodes
        Average Rewards over all training: -450.96
        Average Rewards for last 100 episodes: -371.09
        Episode took 1.60 seconds
Reinforcement Learning Status:
        Completed 1500 out of 2000 training episodes
        Average Rewards over all training: -441.56
        Average Rewards for last 100 episodes: -309.96
        Episode took 1.63 seconds
Reinforcement Learning Status:
        Completed 1600 out of 2000 training episodes
        Average Rewards over all training: -433.93
        Average Rewards for last 100 episodes: -319.57
        Episode took 1.85 seconds
Reinforcement Learning Status:
        Completed 1700 out of 2000 training episodes
        Average Rewards over all training: -424.75
        Average Rewards for last 100 episodes: -277.87
        Episode took 1.55 seconds
Reinforcement Learning Status:
        Completed 1800 out of 2000 training episodes
        Average Rewards over all training: -416.04
        Average Rewards for last 100 episodes: -267.91
        Episode took 1.54 seconds
Reinforcement Learning Status:
        Completed 1900 out of 2000 training episodes
        Average Rewards over all training: -406.17
        Average Rewards for last 100 episodes: -228.48
        Episode took 1.65 seconds
Reinforcement Learning Status:
        Completed 2000 out of 2000 training episodes
        Average Rewards over all training: -398.80
        Average Rewards for last 100 episodes: -258.81
        Episode took 1.64 seconds
Training Done (turning off epsilon and alpha)
---------------------------------------------
Pacman emerges victorious! Score: 503
Pacman emerges victorious! Score: 495
Pacman emerges victorious! Score: 495
Pacman emerges victorious! Score: 496
Pacman emerges victorious! Score: 503
Pacman emerges victorious! Score: 503
Pacman emerges victorious! Score: 503
Pacman emerges victorious! Score: 495
Pacman died! Score: -518
Pacman emerges victorious! Score: 495
Average Score: 397.0
Scores:        503.0, 495.0, 495.0, 496.0, 503.0, 503.0, 503.0, 495.0, -518.0, 495.0
Win Rate:      9/10 (0.90)
Record:        Win, Win, Win, Win, Win, Win, Win, Win, Loss, Win


BALANCEADO
=====================================================================================
python pacman.py -p PacmanQAgent -x 2000 -n 2010 -l smallGrid -a epsilon=0.4,alpha=0.3,gamma=0.7
Beginning 2000 episodes of Training
Reinforcement Learning Status:
        Completed 100 out of 2000 training episodes
        Average Rewards over all training: -509.41
        Average Rewards for last 100 episodes: -509.41
        Episode took 0.86 seconds
Reinforcement Learning Status:
        Completed 200 out of 2000 training episodes
        Average Rewards over all training: -510.46
        Average Rewards for last 100 episodes: -511.51
        Episode took 1.08 seconds
Reinforcement Learning Status:
        Completed 300 out of 2000 training episodes
        Average Rewards over all training: -510.24
        Average Rewards for last 100 episodes: -509.81
        Episode took 1.01 seconds
Reinforcement Learning Status:
        Completed 400 out of 2000 training episodes
        Average Rewards over all training: -507.62
        Average Rewards for last 100 episodes: -499.76
        Episode took 0.96 seconds
Reinforcement Learning Status:
        Completed 500 out of 2000 training episodes
        Average Rewards over all training: -505.92
        Average Rewards for last 100 episodes: -499.10
        Episode took 0.91 seconds
Reinforcement Learning Status:
        Completed 600 out of 2000 training episodes
        Average Rewards over all training: -506.43
        Average Rewards for last 100 episodes: -508.97
        Episode took 0.93 seconds
Reinforcement Learning Status:
        Completed 700 out of 2000 training episodes
        Average Rewards over all training: -502.65
        Average Rewards for last 100 episodes: -479.99
        Episode took 1.17 seconds
Reinforcement Learning Status:
        Completed 800 out of 2000 training episodes
        Average Rewards over all training: -502.30
        Average Rewards for last 100 episodes: -499.81
        Episode took 1.10 seconds
Reinforcement Learning Status:
        Completed 900 out of 2000 training episodes
        Average Rewards over all training: -498.66
        Average Rewards for last 100 episodes: -469.55
        Episode took 1.24 seconds
Reinforcement Learning Status:
        Completed 1000 out of 2000 training episodes
        Average Rewards over all training: -497.87
        Average Rewards for last 100 episodes: -490.81
        Episode took 1.13 seconds
Reinforcement Learning Status:
        Completed 1100 out of 2000 training episodes
        Average Rewards over all training: -494.38
        Average Rewards for last 100 episodes: -459.44
        Episode took 1.31 seconds
Reinforcement Learning Status:
        Completed 1200 out of 2000 training episodes
        Average Rewards over all training: -492.36
        Average Rewards for last 100 episodes: -470.10
        Episode took 1.33 seconds
Reinforcement Learning Status:
        Completed 1300 out of 2000 training episodes
        Average Rewards over all training: -492.19
        Average Rewards for last 100 episodes: -490.19
        Episode took 1.28 seconds
Reinforcement Learning Status:
        Completed 1400 out of 2000 training episodes
        Average Rewards over all training: -491.36
        Average Rewards for last 100 episodes: -480.58
        Episode took 1.22 seconds
Reinforcement Learning Status:
        Completed 1500 out of 2000 training episodes
        Average Rewards over all training: -489.23
        Average Rewards for last 100 episodes: -459.46
        Episode took 1.30 seconds
Reinforcement Learning Status:
        Completed 1600 out of 2000 training episodes
        Average Rewards over all training: -488.05
        Average Rewards for last 100 episodes: -470.38
        Episode took 1.33 seconds
Reinforcement Learning Status:
        Completed 1700 out of 2000 training episodes
        Average Rewards over all training: -488.17
        Average Rewards for last 100 episodes: -490.04
        Episode took 1.23 seconds
Reinforcement Learning Status:
        Completed 1800 out of 2000 training episodes
        Average Rewards over all training: -487.70
        Average Rewards for last 100 episodes: -479.64
        Episode took 1.28 seconds
Reinforcement Learning Status:
        Completed 1900 out of 2000 training episodes
        Average Rewards over all training: -486.24
        Average Rewards for last 100 episodes: -460.02
        Episode took 1.28 seconds
Reinforcement Learning Status:
        Completed 2000 out of 2000 training episodes
        Average Rewards over all training: -485.42
        Average Rewards for last 100 episodes: -469.81
        Episode took 1.27 seconds
Training Done (turning off epsilon and alpha)
---------------------------------------------
Pacman emerges victorious! Score: 499
Pacman emerges victorious! Score: 503
Pacman died! Score: -508
Pacman emerges victorious! Score: 499
Pacman died! Score: -517
Pacman died! Score: -502
Pacman emerges victorious! Score: 503
Pacman died! Score: -506
Pacman died! Score: -502
Pacman emerges victorious! Score: 499
Average Score: -3.2
Scores:        499.0, 503.0, -508.0, 499.0, -517.0, -502.0, 503.0, -506.0, -502.0, 499.0
Win Rate:      5/10 (0.50)
Record:        Win, Win, Loss, Win, Loss, Loss, Win, Loss, Loss, Win





SMALL CLASSIC
BALENCEADO
========================================================================
> python pacman.py -p PacmanQAgent -x 2000 -n 2010 -l smallClassic -a epsilon=0.4,alpha=0.3,gamma=0.7
Beginning 2000 episodes of Training
Reinforcement Learning Status:
        Completed 100 out of 2000 training episodes
        Average Rewards over all training: -450.42
        Average Rewards for last 100 episodes: -450.42
        Episode took 6.40 seconds
Reinforcement Learning Status:
        Completed 200 out of 2000 training episodes
        Average Rewards over all training: -448.51
        Average Rewards for last 100 episodes: -446.61
        Episode took 6.94 seconds
Reinforcement Learning Status:
        Completed 300 out of 2000 training episodes
        Average Rewards over all training: -443.70
        Average Rewards for last 100 episodes: -434.06
        Episode took 7.76 seconds
Reinforcement Learning Status:
        Completed 400 out of 2000 training episodes
        Average Rewards over all training: -439.40
        Average Rewards for last 100 episodes: -426.51
        Episode took 8.81 seconds
Reinforcement Learning Status:
        Completed 500 out of 2000 training episodes
        Average Rewards over all training: -440.18
        Average Rewards for last 100 episodes: -443.28
        Episode took 7.20 seconds
Reinforcement Learning Status:
        Completed 600 out of 2000 training episodes
        Average Rewards over all training: -439.37
        Average Rewards for last 100 episodes: -435.35
        Episode took 7.44 seconds
Reinforcement Learning Status:
        Completed 700 out of 2000 training episodes
        Average Rewards over all training: -439.39
        Average Rewards for last 100 episodes: -439.48
        Episode took 7.91 seconds
Reinforcement Learning Status:
        Completed 800 out of 2000 training episodes
        Average Rewards over all training: -437.47
        Average Rewards for last 100 episodes: -424.06
        Episode took 8.25 seconds
Reinforcement Learning Status:
        Completed 900 out of 2000 training episodes
        Average Rewards over all training: -437.01
        Average Rewards for last 100 episodes: -433.33
        Episode took 6.54 seconds
Reinforcement Learning Status:
        Completed 1000 out of 2000 training episodes
        Average Rewards over all training: -436.59
        Average Rewards for last 100 episodes: -432.78
        Episode took 7.96 seconds
Reinforcement Learning Status:
        Completed 1100 out of 2000 training episodes
        Average Rewards over all training: -435.55
        Average Rewards for last 100 episodes: -425.17
        Episode took 7.11 seconds
Reinforcement Learning Status:
        Completed 1200 out of 2000 training episodes
        Average Rewards over all training: -435.42
        Average Rewards for last 100 episodes: -434.03
        Episode took 6.33 seconds
Reinforcement Learning Status:
        Completed 1300 out of 2000 training episodes
        Average Rewards over all training: -435.26
        Average Rewards for last 100 episodes: -433.25
        Episode took 9.40 seconds
Reinforcement Learning Status:
        Completed 1400 out of 2000 training episodes
        Average Rewards over all training: -435.02
        Average Rewards for last 100 episodes: -432.01
        Episode took 6.88 seconds
Reinforcement Learning Status:
        Completed 1500 out of 2000 training episodes
        Average Rewards over all training: -435.03
        Average Rewards for last 100 episodes: -435.13
        Episode took 6.22 seconds
Reinforcement Learning Status:
        Completed 1600 out of 2000 training episodes
        Average Rewards over all training: -433.45
        Average Rewards for last 100 episodes: -409.80
        Episode took 11.62 seconds
Reinforcement Learning Status:
        Completed 1700 out of 2000 training episodes
        Average Rewards over all training: -433.45
        Average Rewards for last 100 episodes: -433.34
        Episode took 7.63 seconds
Reinforcement Learning Status:
        Completed 1800 out of 2000 training episodes
        Average Rewards over all training: -433.41
        Average Rewards for last 100 episodes: -432.75
        Episode took 6.81 seconds
Reinforcement Learning Status:
        Completed 1900 out of 2000 training episodes
        Average Rewards over all training: -433.28
        Average Rewards for last 100 episodes: -431.05
        Episode took 10.11 seconds
Reinforcement Learning Status:
        Completed 2000 out of 2000 training episodes
        Average Rewards over all training: -433.31
        Average Rewards for last 100 episodes: -433.80
        Episode took 7.81 seconds
Training Done (turning off epsilon and alpha)
---------------------------------------------
Pacman died! Score: -437
Pacman died! Score: -440
Pacman died! Score: -427
Pacman died! Score: -378
Pacman died! Score: -418
Pacman died! Score: -397
Pacman died! Score: -446
Pacman died! Score: -424
Pacman died! Score: -429
Pacman died! Score: -448
Average Score: -424.4
Scores:        -437.0, -440.0, -427.0, -378.0, -418.0, -397.0, -446.0, -424.0, -429.0, -448.0
Win Rate:      0/10 (0.00)
Record:        Loss, Loss, Loss, Loss, Loss, Loss, Loss, Loss, Loss, Loss



CONSERVADOR
=======================================================================
> python pacman.py -p PacmanQAgent -x 2000 -n 2010 -l smallClassic -a epsilon=0.2,alpha=0.2,gamma=0.8
Beginning 2000 episodes of Training
Reinforcement Learning Status:
        Completed 100 out of 2000 training episodes
        Average Rewards over all training: -438.32
        Average Rewards for last 100 episodes: -438.32
        Episode took 7.08 seconds
Reinforcement Learning Status:
        Completed 200 out of 2000 training episodes
        Average Rewards over all training: -430.19
        Average Rewards for last 100 episodes: -422.07
        Episode took 7.85 seconds
Reinforcement Learning Status:
        Completed 300 out of 2000 training episodes
        Average Rewards over all training: -429.28
        Average Rewards for last 100 episodes: -427.46
        Episode took 8.24 seconds
Reinforcement Learning Status:
        Completed 400 out of 2000 training episodes
        Average Rewards over all training: -427.46
        Average Rewards for last 100 episodes: -421.99
        Episode took 7.12 seconds
Reinforcement Learning Status:
        Completed 500 out of 2000 training episodes
        Average Rewards over all training: -427.18
        Average Rewards for last 100 episodes: -426.07
        Episode took 9.17 seconds
Reinforcement Learning Status:
        Completed 600 out of 2000 training episodes
        Average Rewards over all training: -425.89
        Average Rewards for last 100 episodes: -419.43
        Episode took 8.92 seconds
Reinforcement Learning Status:
        Completed 700 out of 2000 training episodes
        Average Rewards over all training: -424.14
        Average Rewards for last 100 episodes: -413.62
        Episode took 10.06 seconds
Reinforcement Learning Status:
        Completed 800 out of 2000 training episodes
        Average Rewards over all training: -424.25
        Average Rewards for last 100 episodes: -425.03
        Episode took 8.67 seconds
Reinforcement Learning Status:
        Completed 900 out of 2000 training episodes
        Average Rewards over all training: -422.72
        Average Rewards for last 100 episodes: -410.48
        Episode took 8.35 seconds
Reinforcement Learning Status:
        Completed 1000 out of 2000 training episodes
        Average Rewards over all training: -422.37
        Average Rewards for last 100 episodes: -419.19
        Episode took 10.37 seconds
Reinforcement Learning Status:
        Completed 1100 out of 2000 training episodes
        Average Rewards over all training: -422.45
        Average Rewards for last 100 episodes: -423.32
        Episode took 7.70 seconds
Reinforcement Learning Status:
        Completed 1200 out of 2000 training episodes
        Average Rewards over all training: -422.22
        Average Rewards for last 100 episodes: -419.68
        Episode took 8.78 seconds
Reinforcement Learning Status:
        Completed 1300 out of 2000 training episodes
        Average Rewards over all training: -422.25
        Average Rewards for last 100 episodes: -422.54
        Episode took 7.78 seconds
Reinforcement Learning Status:
        Completed 1400 out of 2000 training episodes
        Average Rewards over all training: -421.49
        Average Rewards for last 100 episodes: -411.62
        Episode took 8.64 seconds
Reinforcement Learning Status:
        Completed 1500 out of 2000 training episodes
        Average Rewards over all training: -420.62
        Average Rewards for last 100 episodes: -408.47
        Episode took 10.63 seconds
Reinforcement Learning Status:
        Completed 1600 out of 2000 training episodes
        Average Rewards over all training: -420.77
        Average Rewards for last 100 episodes: -423.06
        Episode took 7.15 seconds
Reinforcement Learning Status:
        Completed 1700 out of 2000 training episodes
        Average Rewards over all training: -420.26
        Average Rewards for last 100 episodes: -412.14
        Episode took 8.44 seconds
Reinforcement Learning Status:
        Completed 1800 out of 2000 training episodes
        Average Rewards over all training: -420.53
        Average Rewards for last 100 episodes: -424.99
        Episode took 11.68 seconds
Reinforcement Learning Status:
        Completed 1900 out of 2000 training episodes
        Average Rewards over all training: -419.84
        Average Rewards for last 100 episodes: -407.43
        Episode took 8.68 seconds
Reinforcement Learning Status:
        Completed 2000 out of 2000 training episodes
        Average Rewards over all training: -419.92
        Average Rewards for last 100 episodes: -421.47
        Episode took 7.03 seconds
Training Done (turning off epsilon and alpha)
---------------------------------------------
Pacman died! Score: -382
Pacman died! Score: -424
Pacman died! Score: -458
Pacman died! Score: -392
Pacman died! Score: -211
Pacman died! Score: -458
Pacman died! Score: -503
Pacman died! Score: -406
Pacman died! Score: -379
Pacman died! Score: -386
Average Score: -399.9
Scores:        -382.0, -424.0, -458.0, -392.0, -211.0, -458.0, -503.0, -406.0, -379.0, -386.0
Win Rate:      0/10 (0.00)
Record:        Loss, Loss, Loss, Loss, Loss, Loss, Loss, Loss, Loss, Loss



