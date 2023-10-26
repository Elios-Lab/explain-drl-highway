# Explaining a Deep Reinforcement Learning (DRL)-based Automated Driving Agent in Highway Simulations
This repo contains the code to perform a quantitative interpretation of an autonomous agent trained through Deep Reinforcement Learning (DRL) in the [highway-env](https://github.com/eleurent/highway-env) simulation environment. The framework features three types of views for analyzing the data: episode timeline, frame by frame, and aggregated statistical analysis. Beside domain-specific signals (e.g., position and speed of the vehicles), the quantities considered and compared include attention and [SHAP](https://shap.readthedocs.io/en/latest/index.html) values. At every frame, SHAP values are computed on the value network, which outputs the Q value of each possible action (i.e., the overall expected reward taking that action). For the attention values, we take the output of the attention layer, which is a probability distribution across vehicles.

To compare SHAP (per feature) and attention (per vehicle) values, we defined the SHAP value of a vehicle as the SHAP of its most important feature. Such per-vehicle SHAP values are then converted into probabilities through softmax.

$SHAP(V_i)=MAX_{f_i \in \{features\:of\:V_i\}}\{SHAP(f_i)\}$

In order to perform the analysis we make use of slightly modified versions of the [highway-env](https://github.com/eleurent/highway-env) 1.4 and the [rl-agents](https://github.com/eleurent/rl-agents) 1.0 packages.

The notebook contains the results of an experiment with 150 test episodes of 80 frame length each. Besides, Excel files are available for a segmentation analysis (considering different attention/SHAP/traffic/action conditions) and for a correlation analysis between conditions.

More details will be available as soon as the related scientific article is published. 

## System requirements
- Tested on Windows, macOS and Ubuntu 20.04
- Python 3.8
## Installation
After you downloaded the repo, all the required packages can be installed through the following command:
```
pip install -r requirements.txt
```
A Python virtual environment or a conda environment is suggested to avoid conflicts with other pre-existent packages.
## Execution
The entire analysis can be performed by using the `Analysis.ipynb` Jupyter notebook provided in the repository.
## Foundation repositories
### [highway-env](https://github.com/eleurent/highway-env) 
highway-env provides the highway gym environment. The changes we made are listed as follows:
- `highway-env.py`: We set `vehicles_density` to 0.6, `collision_reward` to -2, `high_speed_reward` to 1, and `right_lane_reward` to 0.
- `observation.py` and `graphics.py`: We edited the code so to render the vehicles' speeds and the action taken at each step. 
### [rl-agents](https://github.com/eleurent/rl-agents)
rl-agents is a collection of reinforcement learning agents that can train on the highway-env environment. The changes we made are listed as follows:
- `env_obs_attention.json`: We changed the highway environment parameters as follows:
    ```
    {
        "id": "highway-v0",
        "import_module": "highway_env",
        "lanes_count": 3,
        "vehicles_count": 15,
        "policy_frequency": 1,
        "duration": 80,
        "observation": {
            "type": "Kinematics",
            "vehicles_count": 8,
            "features": [
                "presence",
                "x",
                "y",
                "vx",
                "vy",
                "cos_h",
                "sin_h"
            ],
            "absolute": false
        }
    }
    ```
- `evaluation.py`: We edited the code so to keep trace of the q-value, image, action and state logs, which are needed for the agent analysis.

## Citing
Please, refer to [this paper](https://ieeexplore.ieee.org/document/10077125) for citing.
```
@ARTICLE{10077125,
  author={Bellotti, Francesco and Lazzaroni, Luca and Capello, Alessio and Cossu, Marianna and De Gloria, Alessandro and Berta, Riccardo},
  journal={IEEE Access}, 
  title={Explaining a Deep Reinforcement Learning (DRL)-Based Automated Driving Agent in Highway Simulations}, 
  year={2023},
  volume={11},
  number={},
  pages={28522-28550},
  doi={10.1109/ACCESS.2023.3259544}}
```
