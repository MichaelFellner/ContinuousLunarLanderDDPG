# Continuous Lunar Lander with DDPG

This repo implements a Deep Deterministic Policy Gradient (DDPG) agent for the continuous version of OpenAI Gym's LunarLander environment.

A lot of people tackle the discrete version of LunarLander using DQN-style approaches, but the continuous version poses a different challenge. The lander’s main and side engines need to be controlled using continuous values, making the space of valid actions much harder to explore. You can’t just enumerate a few actions and brute-force your way through—this problem requires stable function approximation, continuous control, and a good exploration strategy.

---


## Key Challenges

**1. Action range and environment dynamics**\
Mapping the actor’s output to a valid action range took some work. LunarLanderContinuous expects actions between [-1, 1], but how those actions affect the lander isn’t obvious. The side engines in particular are sensitive and can spin you out with just a small push.

**2. DDPG instability**\
Like a lot of people who’ve worked with DDPG, I ran into instability. Without target networks and soft updates, things quickly fall apart. I had to tune tau, batch size, learning rate, and network architecture to get things working.

**3. Exploration**\
Standard epsilon-greedy doesn’t work in continuous settings. I added Ornstein-Uhlenbeck noise to the actor’s actions to get temporally correlated exploration. Even then, exploration was still inefficient for a while—early episodes barely did anything useful.

**4. Sparse rewards**\
You don’t get much signal until you’re close to landing, and that means long stretches of time where the agent doesn’t know if it’s doing well. This led to plateaus and required longer training times to see consistent improvement.

---

## How to run it

```bash
git clone https://github.com/MichaelFellner/ContinuousLunarLanderDDPG.git
cd ContinuousLunarLanderDDPG
pip install -r requirements.txt
python main.py
```

This will start training a DDPG agent from scratch.

---

## Notes

You’ll want to monitor reward trends over time and not expect immediate improvements. It’s a slow process. One thing I found helpful was periodically visualizing a rollout to see if the lander was actually improving, even if the rewards were noisy.

---

## Future directions

- Try TD3 or SAC for more stable learning
- Visualize the agent’s behavior over time
- Add better logging and evaluation routines
- Test out different noise processes or curriculum learning

---

## Contact

If you're working on something similar or just messing around with continuous control problems, feel free to reach out or fork this and play around with it.

