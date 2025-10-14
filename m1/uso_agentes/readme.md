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

Analisis 2
################################################################################################
> Understan de the pacman code and find where the statuses, actions and rewards are implemented.

⏺ I'll analyze the Pacman codebase to understand where statuses, actions, and rewards are implemented. Let me start by exploring the structure of the
  project.

⏺ Bash(find . -name "*.py" | head -20)
  ⎿  ./qlearningAgents.py          
     ./pacman.py
     ./pacmanAgents.py
     … +17 lines (ctrl+r to expand)

⏺ Search(pattern: "*.py")
  ⎿  Found 28 files (ctrl+r to expand)

⏺ Let me examine the key files that likely contain the core game mechanics, starting with game.py, pacman.py, and environment.py:

⏺ Read(game.py)
  ⎿  Read 779 lines (ctrl+r to expand)

⏺ Read(pacman.py)
  ⎿  Read 739 lines (ctrl+r to expand)

⏺ Read(environment.py)
  ⎿  Read 57 lines (ctrl+r to expand)

⏺ Now let me examine additional key files to understand the reinforcement learning specific components:

⏺ Read(mdp.py)
  ⎿  Read 68 lines (ctrl+r to expand)

⏺ Read(gridworld.py)
  ⎿  Read 590 lines (ctrl+r to expand)

⏺ Now let me look at the reinforcement learning agent files to see how rewards and learning work:

⏺ Read(learningAgents.py)
  ⎿  Read 100 lines (ctrl+r to expand)

⏺ Read(qlearningAgents.py)
  ⎿  Read 100 lines (ctrl+r to expand)

⏺ Now let me search for more details about how rewards are implemented in the Pacman specific environment:

⏺ Search(pattern: "scoreChange|reward|score", glob: "*.py", output_mode: "content")
  ⎿  Found 92 lines (ctrl+r to expand)

⏺ Based on my analysis of the Pacman codebase, here's where the statuses, actions, and rewards are implemented:

  Game States/Statuses

  Main Implementation: game.py and pacman.py

  - GameState class (pacman.py:61-263): Core state representation
    - isWin() (pacman.py:221): Terminal win condition
    - isLose() (pacman.py:218): Terminal loss condition
    - State includes: agent positions, food, capsules, score, walls
  - GameStateData class (game.py:396-539): Internal state data
    - _win and _lose flags (game.py:415)
    - score and scoreChange tracking (game.py:408, 416)

  Actions

  Main Implementation: game.py

  - Directions class (game.py:53-73): Available actions
    - NORTH, SOUTH, EAST, WEST, STOP
    - Direction mappings (LEFT, RIGHT, REVERSE)
  - Actions class (game.py:307-394): Action handling utilities
    - getPossibleActions() (game.py:352): Legal moves based on walls
    - directionToVector() (game.py:347): Convert actions to movement vectors
  - Agent action selection:
    - PacmanRules.getLegalActions() (pacman.py:347): Pacman-specific legal actions
    - GhostRules.getLegalActions() (pacman.py:406): Ghost-specific legal actions

  Rewards

  Main Implementation: Multiple files with different reward systems

  Pacman Game Rewards (pacman.py)

  - Food consumption: +10 points (pacman.py:381)
  - Winning bonus: +500 points for eating all food (pacman.py:388)
  - Time penalty: -1 point per move (pacman.py:122, 274)
  - Capsule collection: +200 points when eating scared ghost (pacman.py:462)
  - Death penalty: -500 points when killed by ghost (pacman.py:469)

  Gridworld Rewards (gridworld.py)

  - Living reward: Configurable penalty for each step (gridworld.py:32, 43)
  - Terminal rewards: Numeric values in grid cells (gridworld.py:92-94)
  - Environment reward calculation: getReward() (gridworld.py:80-94)

  Reinforcement Learning Integration

  - Reward observation: learningAgents.py:212 - calculates reward as score difference
  - Q-learning update: qlearningAgents.py:141 - uses reward in Q-value updates
  - Environment interface: environment.py:39 - doAction() returns (reward, nextState) pair

  The reward system supports both discrete game scoring (Pacman) and continuous reinforcement learning through score differentials.

> Find where the parameters x, n, l, epsilon, alpha, gamma, are handled.

⏺ I'll search through the codebase to find where these reinforcement learning parameters are handled.

⏺ Search(pattern: "epsilon|alpha|gamma|discount", glob: "*.py", output_mode: "content")
  ⎿  Found 131 lines (ctrl+r to expand)

⏺ Search(pattern: "\-x|\-n|\-l", glob: "*.py", output_mode: "content")
  ⎿  Found 29 lines (ctrl+r to expand)

⏺ Now let me look at command line argument parsing to find these specific parameters:

⏺ Read(gridworld.py)
  ⎿  Read 50 lines (ctrl+r to expand)

⏺ Read(pacman.py)
  ⎿  Read 20 lines (ctrl+r to expand)

⏺ Search(pattern: "numTraining|numGames", glob: "*.py", output_mode: "content", head_limit: 10)
  ⎿  Found 10 lines (ctrl+r to expand)

⏺ Based on my analysis of the codebase, here's where each parameter (x, n, l, epsilon, alpha, gamma) is handled:

  Parameter x (numTraining)

  Command Line: -x, --numTraining (pacman.py:549)
  - Usage: Number of training episodes (suppresses output during training)
  - Default: 0
  - Implementation:
    - Passed to agent constructor (pacman.py:580)
    - Controls quiet mode during training (pacman.py:683)
    - Used in runGames() to suppress graphics for first x games (pacman.py:675)

  Parameter n (numGames OR noise)

  In Pacman: -n, --numGames (pacman.py:519)

  - Usage: Total number of games to play
  - Default: 1
  - Implementation: Controls main game loop (pacman.py:682)

  In Gridworld: -n, --noise (gridworld.py:388)

  - Usage: Probability of unintended movement direction
  - Default: 0.2
  - Implementation: Set on MDP (gridworld.py:460)

  Parameter l (learningRate)

  Command Line: -l, --learningRate (gridworld.py:395)
  - Usage: TD (Temporal Difference) learning rate
  - Default: 0.5
  - Implementation:
    - Passed to Q-learning agent in qLearnOpts (gridworld.py:492)
    - Used as alpha parameter in agent constructor

  Parameter epsilon (exploration rate)

  Command Line: -e, --epsilon (gridworld.py:392)
  - Usage: Probability of taking random action in ε-greedy exploration
  - Default: 0.3
  - Implementation:
    - Agent initialization: learningAgents.py:45, qlearningAgents.py:156
    - Action selection: qlearningAgents.py:119 - util.flipCoin(self.epsilon)
    - Training control: Set to 0.0 after training ends (learningAgents.py:154)

  Parameter alpha (learning rate)

  Direct usage in agent classes:
  - Agent initialization: learningAgents.py:44, qlearningAgents.py:156
  - Q-value updates: qlearningAgents.py:141
  self.qValues[(state, action)] = (1-self.alpha) * self.getQValue(state, action) + self.alpha * (reward + self.discount *
  self.computeValueFromQValues(nextState))
  - Setter method: learningAgents.py:189-190
  - Training control: Set to 0.0 after training (learningAgents.py:155)

  Parameter gamma (discount factor)

  Command Line: -d, --discount (gridworld.py:382)
  - Usage: Discount factor for future rewards
  - Default: 0.9
  - Implementation:
    - Value iteration: valueIterationAgents.py:83, 106
    - Q-learning updates: qlearningAgents.py:142
    - Agent initialization: Stored as self.discount (learningAgents.py:46)
    - Alternative name: Often referenced as gamma in option dictionaries (gridworld.py:491)

  Parameter Flow:

  1. Command line parsing → Agent options dictionary → Agent constructor → Algorithm implementation
  2. Parameters can be set via command line or directly in agent constructor
  3. GUI controls available in crawler display (graphicsCrawlerDisplay.py:51-65)

> would you be able to determine how the pacman behavior changes as the parameters value do as well?

⏺ I'll analyze how Pacman's behavior changes with different parameter values by examining the code implementation and understanding the reinforcement
  learning dynamics.

⏺ Read(qlearningAgents.py)
  ⎿  Read 50 lines (ctrl+r to expand)

⏺ Read(learningAgents.py)
  ⎿  Read 30 lines (ctrl+r to expand)

⏺ Read(valueIterationAgents.py)
  ⎿  Read 30 lines (ctrl+r to expand)

⏺ Based on my analysis of the code implementation, here's how Pacman's behavior changes with different parameter values:

  Epsilon (ε) - Exploration Rate

  Code Location: qlearningAgents.py:119 - util.flipCoin(self.epsilon)

  Lower Epsilon (0.0 - 0.2):

  - Behavior: More exploitative - follows learned policy strictly
  - Early stages: Poor performance due to insufficient exploration
  - Later stages: Consistent, optimal behavior once good policy is learned
  - Risk: Gets stuck in suboptimal strategies

  Higher Epsilon (0.7 - 1.0):

  - Behavior: More exploratory - takes random actions frequently
  - Early stages: Better exploration, discovers new strategies
  - Later stages: Inconsistent, suboptimal performance due to random choices
  - Risk: Never converges to optimal policy

  After Training: Set to 0.0 (learningAgents.py:154) - pure exploitation

  Alpha (α) - Learning Rate

  Code Location: qlearningAgents.py:141 - Q-value update formula

  Lower Alpha (0.1 - 0.3):

  - Learning: Slow, cautious updates to Q-values
  - Stability: More stable, less sensitive to individual experiences
  - Convergence: Takes longer but more reliable
  - Behavior: Conservative policy changes, gradual improvement

  Higher Alpha (0.7 - 1.0):

  - Learning: Fast, aggressive updates to Q-values
  - Stability: Less stable, very sensitive to recent experiences
  - Convergence: Faster initial learning but may oscillate
  - Behavior: Rapid policy changes, potentially erratic

  After Training: Set to 0.0 (learningAgents.py:155) - no more learning

  Gamma (γ) - Discount Factor

  Code Location: qlearningAgents.py:142, valueIterationAgents.py:83, 106

  Lower Gamma (0.1 - 0.5):

  - Strategy: Myopic - focuses on immediate rewards
  - Behavior: Prioritizes nearby food, avoids long-term planning
  - Risk-taking: More willing to take risks for immediate gains
  - Ghost avoidance: Less concerned about future ghost encounters

  Higher Gamma (0.8 - 0.99):

  - Strategy: Far-sighted - considers long-term consequences
  - Behavior: Plans multi-step strategies, considers future rewards
  - Risk-taking: More cautious, avoids actions that lead to future danger
  - Ghost avoidance: Strongly considers future ghost positions

  Gamma = 1.0:

  - Strategy: Equal weight to all future rewards
  - Risk: May cause convergence issues in infinite environments

  NumTraining (x) - Training Episodes

  Code Location: learningAgents.py:152-155

  Low NumTraining (10 - 50):

  - Learning: Insufficient learning time
  - Performance: Poor initial performance, inconsistent strategies
  - Exploration: Stops exploring before finding good policies
  - Q-values: Incomplete, inaccurate value estimates

  High NumTraining (500 - 2000):

  - Learning: Extensive learning period
  - Performance: Better converged policies, more consistent behavior
  - Exploration: Sufficient time to discover optimal strategies
  - Q-values: More accurate, well-trained estimates

  Noise (n) - Movement Uncertainty

  Code Location: gridworld.py:143, 153 - transition probabilities

  Low Noise (0.0 - 0.1):

  - Movement: Deterministic, reliable actions
  - Strategy: Can use precise positioning strategies
  - Learning: Faster convergence due to predictable outcomes
  - Risk: Overconfident in narrow corridors

  High Noise (0.4 - 0.8):

  - Movement: Unpredictable, often goes in wrong direction
  - Strategy: Must account for movement uncertainty
  - Learning: Slower convergence, learns robust policies
  - Behavior: Avoids risky positions near ghosts/walls

  Learning Rate (l) - Same as Alpha

  When used in gridworld (gridworld.py:395), has same effects as alpha parameter above.

  Combined Effects Examples:

  Aggressive Learner: High α, High ε, Low γ

  - Learns fast but focuses on immediate rewards, explores extensively
  - Good for dynamic environments, poor for complex strategies

  Conservative Learner: Low α, Low ε, High γ

  - Learns slowly but develops stable long-term strategies
  - Good for complex environments requiring planning

  Balanced Learner: Medium values (α=0.5, ε=0.3, γ=0.9)

  - Reasonable exploration-exploitation trade-off
  - Good general-purpose configuration

  The key insight is that these parameters control the exploration-exploitation trade-off, learning speed vs stability, and short-term vs long-term thinking
  in Pacman's decision-making process.

################################################################################################




SMALL GRID

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


CONSERVADOR GANADOR
=====================================================================================
> python pacman.py -p PacmanQAgent -a epsilon=0.05,alpha=0.1,gamma=0.95 -x 2000 -n 2010 -l smallGrid
Beginning 2000 episodes of Training
Reinforcement Learning Status:
        Completed 100 out of 2000 training episodes
        Average Rewards over all training: -510.02
        Average Rewards for last 100 episodes: -510.02
        Episode took 0.38 seconds
Reinforcement Learning Status:
        Completed 200 out of 2000 training episodes
        Average Rewards over all training: -506.51
        Average Rewards for last 100 episodes: -503.01
        Episode took 0.59 seconds
Reinforcement Learning Status:
        Completed 300 out of 2000 training episodes
        Average Rewards over all training: -482.01
        Average Rewards for last 100 episodes: -432.99
        Episode took 0.67 seconds
Reinforcement Learning Status:
        Completed 400 out of 2000 training episodes
        Average Rewards over all training: -454.48
        Average Rewards for last 100 episodes: -371.89
        Episode took 0.73 seconds
Reinforcement Learning Status:
        Completed 500 out of 2000 training episodes
        Average Rewards over all training: -435.77
        Average Rewards for last 100 episodes: -360.93
        Episode took 0.71 seconds
Reinforcement Learning Status:
        Completed 600 out of 2000 training episodes
        Average Rewards over all training: -421.43
        Average Rewards for last 100 episodes: -349.71
        Episode took 0.70 seconds
Reinforcement Learning Status:
        Completed 700 out of 2000 training episodes
        Average Rewards over all training: -392.47
        Average Rewards for last 100 episodes: -218.76
        Episode took 0.75 seconds
Reinforcement Learning Status:
        Completed 800 out of 2000 training episodes
        Average Rewards over all training: -365.90
        Average Rewards for last 100 episodes: -179.87
        Episode took 0.79 seconds
Reinforcement Learning Status:
        Completed 900 out of 2000 training episodes
        Average Rewards over all training: -344.42
        Average Rewards for last 100 episodes: -172.64
        Episode took 0.88 seconds
Reinforcement Learning Status:
        Completed 1000 out of 2000 training episodes
        Average Rewards over all training: -319.02
        Average Rewards for last 100 episodes: -90.36
        Episode took 0.79 seconds
Reinforcement Learning Status:
        Completed 1100 out of 2000 training episodes
        Average Rewards over all training: -287.11
        Average Rewards for last 100 episodes: 31.99
        Episode took 0.81 seconds
Reinforcement Learning Status:
        Completed 1200 out of 2000 training episodes
        Average Rewards over all training: -263.01
        Average Rewards for last 100 episodes: 2.04
        Episode took 0.80 seconds
Reinforcement Learning Status:
        Completed 1300 out of 2000 training episodes
        Average Rewards over all training: -243.44
        Average Rewards for last 100 episodes: -8.62
        Episode took 0.80 seconds
Reinforcement Learning Status:
        Completed 1400 out of 2000 training episodes
        Average Rewards over all training: -233.20
        Average Rewards for last 100 episodes: -100.02
        Episode took 0.78 seconds
Reinforcement Learning Status:
        Completed 1500 out of 2000 training episodes
        Average Rewards over all training: -210.76
        Average Rewards for last 100 episodes: 103.46
        Episode took 0.81 seconds
Reinforcement Learning Status:
        Completed 1600 out of 2000 training episodes
        Average Rewards over all training: -190.43
        Average Rewards for last 100 episodes: 114.51
        Episode took 0.76 seconds
Reinforcement Learning Status:
        Completed 1700 out of 2000 training episodes
        Average Rewards over all training: -173.12
        Average Rewards for last 100 episodes: 103.75
        Episode took 0.78 seconds
Reinforcement Learning Status:
        Completed 1800 out of 2000 training episodes
        Average Rewards over all training: -148.15
        Average Rewards for last 100 episodes: 276.36
        Episode took 0.75 seconds
Reinforcement Learning Status:
        Completed 1900 out of 2000 training episodes
        Average Rewards over all training: -127.37
        Average Rewards for last 100 episodes: 246.68
        Episode took 0.77 seconds
Reinforcement Learning Status:
        Completed 2000 out of 2000 training episodes
        Average Rewards over all training: -106.13
        Average Rewards for last 100 episodes: 297.36
        Episode took 0.72 seconds
Training Done (turning off epsilon and alpha)
---------------------------------------------
Pacman emerges victorious! Score: 495
Pacman emerges victorious! Score: 495
Pacman emerges victorious! Score: 495
Pacman emerges victorious! Score: 503
Pacman emerges victorious! Score: 495
Pacman emerges victorious! Score: 503
Pacman emerges victorious! Score: 495
Pacman emerges victorious! Score: 503
Pacman emerges victorious! Score: 503
Pacman emerges victorious! Score: 499
Average Score: 498.6
Scores:        495.0, 495.0, 495.0, 503.0, 495.0, 503.0, 495.0, 503.0, 503.0, 499.0
Win Rate:      10/10 (1.00)
Record:        Win, Win, Win, Win, Win, Win, Win, Win, Win, Win




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


CONSERVADOR 2
=======================================================================
> python pacman.py -p PacmanQAgent -a epsilon=0.05,alpha=0.1,gamma=0.95 -x 2000 -n 2010 -l smallClassic
Beginning 2000 episodes of Training
Reinforcement Learning Status:
        Completed 100 out of 2000 training episodes
        Average Rewards over all training: -430.27
        Average Rewards for last 100 episodes: -430.27
        Episode took 3.31 seconds
Reinforcement Learning Status:
        Completed 200 out of 2000 training episodes
        Average Rewards over all training: -425.88
        Average Rewards for last 100 episodes: -421.48
        Episode took 3.72 seconds
Reinforcement Learning Status:
        Completed 300 out of 2000 training episodes
        Average Rewards over all training: -416.39
        Average Rewards for last 100 episodes: -397.41
        Episode took 3.78 seconds
Reinforcement Learning Status:
        Completed 400 out of 2000 training episodes
        Average Rewards over all training: -414.14
        Average Rewards for last 100 episodes: -407.40
        Episode took 4.44 seconds
Reinforcement Learning Status:
        Completed 500 out of 2000 training episodes
        Average Rewards over all training: -411.91
        Average Rewards for last 100 episodes: -402.99
        Episode took 4.30 seconds
Reinforcement Learning Status:
        Completed 600 out of 2000 training episodes
        Average Rewards over all training: -413.15
        Average Rewards for last 100 episodes: -419.35
        Episode took 3.36 seconds
Reinforcement Learning Status:
        Completed 700 out of 2000 training episodes
        Average Rewards over all training: -409.20
        Average Rewards for last 100 episodes: -385.49
        Episode took 4.55 seconds
Reinforcement Learning Status:
        Completed 800 out of 2000 training episodes
        Average Rewards over all training: -408.82
        Average Rewards for last 100 episodes: -406.16
        Episode took 4.04 seconds
Reinforcement Learning Status:
        Completed 900 out of 2000 training episodes
        Average Rewards over all training: -406.91
        Average Rewards for last 100 episodes: -391.67
        Episode took 3.39 seconds
Reinforcement Learning Status:
        Completed 1000 out of 2000 training episodes
        Average Rewards over all training: -406.86
        Average Rewards for last 100 episodes: -406.42
        Episode took 4.84 seconds
Reinforcement Learning Status:
        Completed 1100 out of 2000 training episodes
        Average Rewards over all training: -404.92
        Average Rewards for last 100 episodes: -385.50
        Episode took 4.01 seconds
Reinforcement Learning Status:
        Completed 1200 out of 2000 training episodes
        Average Rewards over all training: -403.46
        Average Rewards for last 100 episodes: -387.35
        Episode took 5.31 seconds
Reinforcement Learning Status:
        Completed 1300 out of 2000 training episodes
        Average Rewards over all training: -403.17
        Average Rewards for last 100 episodes: -399.76
        Episode took 3.27 seconds
Reinforcement Learning Status:
        Completed 1400 out of 2000 training episodes
        Average Rewards over all training: -403.25
        Average Rewards for last 100 episodes: -404.18
        Episode took 3.08 seconds
Reinforcement Learning Status:
        Completed 1500 out of 2000 training episodes
        Average Rewards over all training: -402.90
        Average Rewards for last 100 episodes: -398.07
        Episode took 5.43 seconds
Reinforcement Learning Status:
        Completed 1600 out of 2000 training episodes
        Average Rewards over all training: -402.98
        Average Rewards for last 100 episodes: -404.10
        Episode took 3.74 seconds
Reinforcement Learning Status:
        Completed 1700 out of 2000 training episodes
        Average Rewards over all training: -402.77
        Average Rewards for last 100 episodes: -399.52
        Episode took 3.68 seconds
Reinforcement Learning Status:
        Completed 1800 out of 2000 training episodes
        Average Rewards over all training: -402.09
        Average Rewards for last 100 episodes: -390.44
        Episode took 6.02 seconds
Reinforcement Learning Status:
        Completed 1900 out of 2000 training episodes
        Average Rewards over all training: -401.85
        Average Rewards for last 100 episodes: -397.54
        Episode took 3.11 seconds
Reinforcement Learning Status:
        Completed 2000 out of 2000 training episodes
        Average Rewards over all training: -401.14
        Average Rewards for last 100 episodes: -387.66
        Episode took 3.72 seconds
Training Done (turning off epsilon and alpha)
---------------------------------------------
Pacman died! Score: -399
Pacman died! Score: -411
Pacman died! Score: -412
Pacman died! Score: -463
Pacman died! Score: -378
Pacman died! Score: 11
Pacman died! Score: -399
Pacman died! Score: -443
Pacman died! Score: -396
Pacman died! Score: -432
Average Score: -372.2
Scores:        -399.0, -411.0, -412.0, -463.0, -378.0, 11.0, -399.0, -443.0, -396.0, -432.0
Win Rate:      0/10 (0.00)
Record:        Loss, Loss, Loss, Loss, Loss, Loss, Loss, Loss, Loss, Loss



