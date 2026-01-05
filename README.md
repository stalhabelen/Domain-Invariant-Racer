# Domain-Invariant Racer

This repo is a RL project where I trained a PPO agent on **Gymnasium CarRacing-v3** and tried to make it less sensitive to visual changes.

Motivation was basically **closing the sim-to-reality gap** (at least the *visual* part of it). In simulation everything looks “clean”, but in real-world settings you get camera noise, lighting changes, compression artifacts, etc. So instead of training only on clean frames, we used a simple **domain randomization** idea: add visual noise during training so the policy learns features that stay useful even when the image distribution shifts.

The agent was trained for **2,000,000 timesteps** (2M iterations).

## Contents
- PPO training (Stable-Baselines3)
- Observation noise for domain randomization
- Robustness evaluation: reward vs. noise intensity (sigma)
- Value-based attention / saliency visualization
- Short rollout demo video
  

## Results

### What worked (why this helps sim-to-real)
One nice outcome is that the agent didn’t just memorize colors.  
Even when the observation colors/noise change, it still performs reasonably well because it learned more *stable* cues in the scene.

From the value-based attention map, the model focuses more on the **road geometry** rather than specific textures:
- the **car heading / orientation**
- the **road boundaries and curvature**
- and especially the **corner / chicane area** (you can see the attention getting denser there)

So instead of relying on “this exact shade of green/gray means grass/road”, it learns the structure that actually matters for control. This is exactly the kind of behavior we want when trying to reduce the **visual sim-to-reality gap**.


### Robustness vs Visual Noise
![Robustness Curve](Domain-Invariant-Racer/assets/robustness_curve.png)

### Agent View + Value-Based Attention
![Saliency](Domain-Invariant-Racer/assets/saliency_map.png)

## Demo
Video file: [assets/demo_a100-episode-0.mp4](Domain-Invariant-Racer/assets/demo_a100-episode-0.mp4)

## Repo structure
```text
Domain-Invariant-Racer/
  assets/
    demo_a100-episode-0.mp4
    robustness_curve.png
    saliency_map.png
  notebooks/
    Domain_Invariant_Racer.ipynb
```
## Setup

Install the main dependencies:
```bash pip install "gymnasium[box2d]" stable-baselines3 shimmy moviepy```

If Box2D causes issues because of swig:
```brew install swig```

## Run
Open and run the notebook:
notebooks/Domain_Invariant_Racer.ipynb
Run cells top-to-bottom for training, evaluation, plots, and saliency visualization.


## Notes
- Training time depends on your machine.
- The demo video in assets/ is small and included for quick preview.

## Acknowledgements
- Gymnasium (CarRacing-v3)
- Stable-Baselines3 (PPO)
