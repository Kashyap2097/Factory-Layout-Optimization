# Factory-Layout-Optimization

# Introduction
<div style="text-align: justify">
This is a README file for the project "Factory-Layout-Optimization". This project involves importing and installing various required modules and libraries for the project.
The facility layout problem (FLP) deals with the physical arrangement of non-overlapping facilities in a particular area to reduce the total material handling cost. An optimum design layout of facilities improves the material handling efficiency as well as plays an important role that affecting the total performance of the manufacturing system like productivity, material flow, etc. Recent advances in the field of reinforcement learning have shown the capability of solving complex allocation facilities problems by considering material handling cost minimization factors while ignoring other factors related to the facilities' size, shape, and the flexibility of a user-defined number of facilities components. This project focuses on the creation of a custom environment and producing optimum layout design for unequal size facilities by using a reinforcement learning algorithm called Deep Q Learning with a varying number of functional units with the flow of materials and distance between the facilities taken into consideration as an optimum criterion. The algorithm produced an optimum design layout within 2.5 million steps of training by stable baselines library and 0.5 million steps of training by own design deep learning architecture and presented a promising capacity for solving complex facilities design layout problems for real-world applications in the future.
</div>

# Required Modules
The following modules and libraries are required for the project:

  - PyDrive
  - google.colab
  - numpy
  - OpenCV (cv2)
  - matplotlib.pyplot
  - PIL (Image)
  - gym
  - random
  - ipywidgets
  - os, sys
  - imageio

# Documentation
There are different types of  classes corresponding to the unequal shape of the facilities
This claasses are:
  - `Horiz_Rectangle`
  - `Vert_Rectangle`
  - `Lshape`
  - `Big_square`
All classes has same methods
  - `__init__(self)`: Initializes the machine with random coordinates and calculates the centroid.
  - `compare_elements(self, other)`: Checks for overlapping with another machine and resets the position if necessary.
  - `step(self, action)`: Moves the machine according to the given action (0: move left, 1: move right, 2: move up, 3: move down).
  - `com_after_step(self, other)`: Checks if the machines are overlapping after the step and returns a boolean value.
  - `cal_distance(self, other)`: Calculates the distance between the centroids of two machine.
  - `reset_2(self)`: Resets the machine to its original position.
The environment follows the Gym interface, so you can use the reset(), step(), and render() methods to interact with it.

##Custom Environment:
The  `CustomEnv` class extends the gym.Env class and introduces a more complex environment where multiple types of objects (such as horizontal rectangles, vertical rectangles, squares, L-shapes, and big squares) can interact in the environment.
The `CustomEnv` environment has the following characteristics:
  - Observation space: 2D grid with shape (10, 10)
  - Action space: Discrete with 4 possible actions (0: move left, 1: move right, 2: move up, 3: move down)
  - Reward:
  - +1 for each step taken without overlapping with other rectangles
  - -1 for each step taken with overlapping
  - -0.1 multiplied by the distance to the target position at the end of the episode
  - Episode termination:
  - Overlapping with other rectangles
  - Reaching the maximum number of steps
  - Rendering: The environment supports rendering in a human-readable format.

##getOutputWindow Function:
The `getOutputWindow` function is a utility function that generates an output window image based on the objects in the `Object_List`. Each object in the list is represented by a specific color in the output window image.
###Usage:
To use the `getOutputWindow` function, you can import it and call it with the appropriate arguments:

##Render and Close Functions:
The `render` and `close` functions are utility functions for visualizing and closing the environment, respectively.
Render Function:
The `render` function is used to display the current state of the environment. It generates an output image using the `getOutputWindow` function and displays it using the appropriate visualization method.

##Machine Selection:
The code snippet allows the user to select a number of machines from a predefined list using dropdown menus.
###Usage:
1. The code prompts the user to enter the number of machines they want to select.
2. For each machine, a dropdown menu is displayed, allowing the user to choose from a list of available machine types.
3. The selected machine types are stored in a list for further processing.
###Example:
To use the code snippet, follow these steps:
1. Run the code.
2. Enter the number of machines you want to select when prompted.
3. For each machine, choose the desired machine type from the dropdown menu.
4. Once all the machines have been selected, the selected machine types are stored in the `wid_lst` list.

##Creating Custom Environment with Selected Machines:
The code snippet creates a custom environment using the selected machine types.
###Usage:
1. The code assumes that you have already selected the machine types and stored them in the `wid_lst` list.
2. The code iterates over the `wid_lst` list and extracts the selected values from the dropdown menus.
3. The extracted values are stored in the `val_lst` list.
4. The `val_lst` list contains the selected machine types.
5. The code creates an instance of the `CustomEnv` class, passing the `val_lst` list as an argument to initialize the environment with the selected machines.
6. The `finalobj` variable holds the created custom environment with the selected machines.
###Example:
To use the code snippet, make sure you have already run the previous code snippet to select the machine types.
1. Run the code.
2. The code will extract the selected machine types from the `wid_lst` list and store them in the `val_lst` list.
3. The `val_lst` list will be printed, showing the selected machine types.
4. The code will create a custom environment (`finalobj`) using the `CustomEnv` class and the selected machine types.
5. The `finalobj` variable can be used to interact with the custom environment.

##Machine Layout - RL Model Training and Testing:
The code snippet demonstrates the training and testing of a DQN (Deep Q-Network) RL (Reinforcement Learning) agent using a custom environment called "Machine Layout".
###Prerequisites:
- The code requires the `stable_baselines3` library. If it is not already installed, you can install it by running `!pip install stable-baselines3[extra]`.
- Additionally, the `tensorboard` library is required for visualization purposes. You can install it by running `pip install -U tensorboard`.
###Training:
1. The code initializes a custom environment (`finalobj`) using the selected machine types from the previous step.
2. The `check_env` function is used to check the custom environment and output additional warnings if needed.
3. The DQN agent model is created using the `"MlpPolicy"` with the `DQN` class. The model is initialized with the custom environment, and various parameters are set, such as verbosity, exploration settings, and tensorboard logging.
4. The `model.learn` function is called to train the DQN agent for a specified number of total timesteps. The training progress is logged at each interval.
5. After training, the trained model is saved as "machine layout".
###Visualization:
1. The trained model can be visualized using TensorBoard by running the command `!tensorboard dev upload --logdir ./DQN_Machinelayout_tensorboard/`.
2. Ensure that you have the `tensorboard` library installed before running the command.
3. TensorBoard provides visualizations of the training process and performance metrics.
###Testing:
1. The trained model is loaded using `DQN.load("machine layout")`.
2. The variable `episodes` specifies the number of episodes to run the testing loop.
3. The code runs a loop for the specified number of episodes.
4. For each episode, the environment is reset, and the RL agent interacts with the environment until the episode is done.
5. The RL agent's actions are determined using the trained model's `predict` function.
6. The RL agent's observations, rewards, and actions are stored in a list `L` for visualization purposes.
7. The episode score is calculated and displayed.

## Deep Q-Network (DQN) using Keras RL:
The code snippet demonstrates the implementation of the Deep Q-Network (DQN) algorithm using Keras RL. It trains and tests a DQN agent on a custom environment called "Machine Layout".
### Prerequisites:
- The code requires the installation of the following packages: `tensorflow==2.3.1`, `gym`, `keras-rl2`, and `gym[atari]`. You can install them by running `!pip install tensorflow==2.3.1 gym keras-rl2 gym[atari]`.
### Model Architecture:
1. The code defines a neural network model using the Keras API.
2. The model consists of convolutional layers, dropout layers, and dense layers.
3. The input shape of the model is based on the observation space of the custom environment.
4. The output layer has a number of units equal to the number of actions in the environment.
### Model Training:
1. The code builds a DQN agent using the model defined previously.
2. The agent's policy is set to the Epsilon-Greedy policy with linear annealing.
3. A sequential memory is created to store the agent's experiences.
4. The DQN agent is compiled with the Adam optimizer and an evaluation metric.
5. The agent is trained on the custom environment (`finalobj`) for a specified number of steps using the `fit` function.
6. Training progress is logged at specified intervals, and the history of training metrics is displayed.
7. A plot is generated to visualize the episode rewards over training steps.
### Model Testing:
1. The trained DQN agent is tested on the custom environment using the `test` function.
2. The agent's performance is evaluated by running a specified number of episodes.
3. The episode rewards and other performance metrics are displayed.
4. A plot is generated to visualize the episode rewards over testing steps.

