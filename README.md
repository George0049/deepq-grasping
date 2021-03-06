# deepq-grasping

Implements off-policy models from: https://arxiv.org/abs/1802.10264. 

This project can also interface with the robot & environment provided [here](https://github.com/google-research/google-research/tree/master/dql_grasping), which is shown running in the image below.

<p align="center">
  <img src="./docs/grasping_visdom.JPG"/>
</p>

Still a slight WIP; more details and instructions to come. 

## Dependencies

__Note__: Must be running Linux due to dependence on Ray package for parallel policy execution.

```
conda create -n deepq python=3.6
pip install numpy ray gym pybullet psutil
```

And then follow the commmand from [here](https://pytorch.org/) to install the appropriate version of PyTorch 1.0.

# To Run:

This project only implements the offline grasping approach, meaning we will first collect experience offline, and then index this as an experience replay buffer that doesn't change. 

__Note__: The command line can be used to specify a number of additional arguments. See parallel.py for details. 

## Collect Experience

First collect experience using a biased downward policy. The following command will spawn _N_ remote servers that each run a different environment instance, and are used to collect experience in parallel.

```python collect.py --remotes=1 --outdir=data100K```

## Train a Model

Once data has been collected, you can begin training off-policy DQL models by selecting one from the list:

```python parallel.py --remotes=1 --data-dir=data100K --model=[dqn, ddqn, ddpg, supervised, mcre, cmcre]``` 

If running a visdom server, you can replace ```parallel.py``` with ```parallel_vis.py``` to watch task execution. 

# Acknowledgements

Thanks to Eric Jang for model discussion, and the Ray team for helping to debug. 
