
#### Motivation
Just the other day I found myself watching my baby nephew play with a bubble gun in my wife's parent's living room. He walked back and forth in one direction or another, as he frantically laughed pulling the trigger and spraying bubbles up into the air. I wondered if I might be able to model this experience as a physical system and see what mathematics and physics popped out. 

#### Introduction
I'll start by laying out some constraints and simplifying assumptions. My goal was to develop the model entirely from first principles and not rely on any known physics formulae, other than known mathematical functions. The constraints are as follows: 

---

**Constraints**

- Bubbles only blow when the baby is stationary, i.e., not while moving
- I'll ignore the z-coordinate of the gun, i.e., we'll focus on the planar physics in the xy-plane. This makes sense because the height of the baby and subsequently the gun does are negligible in their effect of the physics.
- I'll assume the baby's walk is random and prefer no direction or region of space 
- I'll consider the baby as a point moving through space in the xy-plane and assign it a force vector representing the force from the gun. 
- I'll constrain the geometry and boundary of the problem within a rectangular region (living room) with a height of (0,b) and a width of (a,0) such that the upper right corner is the point (a, b)
- The baby can only take small steps, i.e., the baby can't "jump" from one part of the room to the other part in a single time step.

	<img src="https://raw.githubusercontent.com/Koruption/pure-conjecture/refs/heads/master/assets/Baby%20With%20A%20Bubble%20Gun.png" alt="My Image" style="width: auto; height: 300px;">
	
The image above describes the geometry of the problem and the baby and gun direction at different times $t_i$

---

**Some Guiding Questions**
Below are some questions I came up with at the outset of the problem as a way of helping orient my model and give me some direction. 

Question 1 (Q1): What is the probability of finding the particle at some point $p=(x,y)$?

Question 2 (Q2): What does the distribution of bubbles look like for large time scales? 

Question 3 (Q3): What does the aggregate or macroscopic force profile look like for large time scales vs. small time scales? 

Question 4 (Q4): What known physics emerge from this model? 

Note: For the remaining portions of this journal I'll refer to the baby as "the particle" or "particle" or just "baby" and sometimes use them interchageably, it should be noted that they are the same thing. 

---

#### Derivation
My first intuition was to look at what happens at the limiting boundaries of the system, i.e., for large time scales and very small time scales. Considering the latter, I was led to the following conjecture: 

**Conjecture 1** (C1): For large time scales as $t\to\infty$ the distribution of a point being at some position $p_i=(x,y)$ should become uniform and depend solely on the geometry of the boundary and not time (time dependence vanishes), i.e., no position should be any more likely than the other. Thus, the probability $P(p_i)\to 0$ for large time scales.   

This conjecture makes sense intuitively since in order to measure where the particle might be at any sufficiently large timescale would require defining regions in which the particle could be. The regions themselves would have to be sufficiently small to provide an accurate probability measure. Thus, as the time goes to infinity, the regions need to get smaller and smaller, and hence the probability vanishes. 

Okay, while I was now convinced of the conjecture for large time scales, I realized that this couldn't possibly be the case for small time scales. I knew from watching the baby that there was no way the probability of him being across the room in his next step or a nanosecond later was possible, hence those positions should not be as likely as the positions nearest him at that time, thus the distribution can't be uniform for small time scales.

**Conjecture 2 (C2):** For small time scales i.e., as $t\to 0$ or more specifically as $dt\to 0$, the distribution describing the point's distribution in space is non-uniform, and rely heavily on time and less on the geometry of the boundary.

From (C2) I began to think about the problem in very small time windows, and asked “what's the probability of finding the particle near its current position after we move forward by some arbitrarily small amount of time?” Mathematically we would, given that the particle was at position $p_t=(x_t, y_t)$ at time t, what is the probability of it being at some point $p_{\epsilon}=(x_t+\epsilon(x), y_t+\epsilon(y))$ after a very small change in time, where $\epsilon(x)$ and $\epsilon(y)$ are small random displacements. Surely this probability isn't less likely than being across the room, thus I was led to the following probability expression in the short time regime: $$ P(p_{\epsilon} \,\vert\, p_t) \approx \frac{k}{e^{\beta t}}$$
which physically means that the probability of finding the particle near $p_t$ decays exponentially with time, which makes sense in the short time regime as the baby's legs are small, and he can't jump across the room. Similarly, the particle can't jump to another point far away, hence if the time window is sufficiently small, this should capture the probability evolution. This should also be true in the case of studying the system with respect to a fixed reference point, in the small time regime.

Now the conditional probability can't be true for both timescale regimes so we need to account for that. These conjectures and observations led me to propose a Gaussian distribution to model the particle's position at any time t as: $$\rho(x,y,t) = \frac{1}{4\pi\sigma^2(t)}e^{-\frac{(x-x_t)^2+(y-y_t)^2}{4\sigma^2(t)}} $$ with $\mu = 0$ since the particle has no bias on where it should be. This is a good start, but there are still a lot of pieces missing, namely how does the distribution change with time or account for regime changes? The changes should match up with the conjectures (C1 & C2) I made through observations earlier. To do this I spent a considerable amount of time thinking about the expression for variance. Originally I sought an expression which unified both the large and small timescale regimes, providing a way to effectively transition between the two in a single equation, i.e., effectively “turn off” part of the expression at large timescales and “turn on” another at small timescales. However, this proved to be difficult as any expression I constructed had competing time dependent terms which wouldn't work out if I took $t\to\infty$ or $t\to 0$. Ultimately I resorted to defining the variance as a piecewise function of time as: $$ \sigma^2(t) =\begin{cases} 
      \frac{A}{e^{\beta t}} & t\leq t_c \\
      Be^{\beta t} & t\gt t_c 
   \end{cases} $$
where $t_c$ is the “crossover” timescale separating the small- and large timescale regimes, where $A$,$B$ are constants chosen to ensure smooth continuity at $t=t_c$, and $\beta$ is a tuning parameter which can be matched to experimental results. We now have a way of capturing both regime's dynamics, such that as $t\to t_c^{+}$ from above, the distribution's variance is dominated by the exponential decay, and similarly as $t->t_c^{-}$ the variance spreads rapidly (exponentially) leading to a stationary and uniform-like distribution, i.e., every position is equally likely and the probability of any specific position vanishes. 

At this point I've derived part of the model, namely describing probabilistically the position of the particle over time and in different regimes, but we don't yet have an accurate picture of the gun, without which we have no bubbles! For my force derivation my first insight was recognizing that we can't simply define a static force field over the entire geometry, it just doesn't make sense since the force is largely dependent on the baby and hence the position of the point at some time $t$. So I started thinking about the force "popping up" at random points in space, which aligned with our notion about the dynamics of how our particle spatially behaves. I realized I couldn't just define a force vector without taking the particle's position into account, which subsequently means that we have to account for its probabilistic nature. It wouldn't make sense to say the force itself is probabilistic with its own distribution, because it isn't so far as I can tell, but rather it's entirely deterministic in the sense that once the baby is stationary at some point in space, the force emerges at that point from the gun! This naturally led me to think about the force as a sort of delta function $\delta(r-r_0)$, that collapses the force to the most likely position of the particle at time $t$. In my opinion, this was an elegant and accurate description of the force position at any time, so I moved on to deriving a descriptive expression for the force itself. 

I imagined that the effect of the force on the bubbles would largely be planar, so that we could ignore any z-component and hence keep consistent with our xy-plane constraints. In 1-Dimension I imagined the force (wind) from the gun decreasing in proportion to the distance from the muzzle, reasoning that the decrease was something like $1/r$ where $r$ was the distance from the muzzle. This seemed reasonable since the bubble are initially driven forward in the plane by the force of the gun, but its effects decrease with as the bubbles move further away from the muzzle, where ambient forces such as temperature, gravity, etc., dominate. Thus, I defined force as: $$ \textbf{F}(\textbf{r}) = \frac{k}{||\delta^2(\textbf{r} - \textbf{r}_{\sigma} ) - \textbf{r}_i||}\hat{d}$$
where $\textbf{r}_{\sigma}$ is the most probable position of the particle at time $t$ in the plane, $\hat{d}$ is the direction of the gun, $\textbf{r}_i$ some position away from the gun, and $k$ some magnitudinal coefficient. From this expression we've defined a force whose effect is 0 everywhere except at the most probable particle position at any time t, which can be interpreted from the delta function's collapsing the force to that point. Now one thing I want to be sure to be sure to discuss is the notion of the direction of the gun $\hat{d}$ since it's a subtle, yet important detail. So far we've discussed the probabilistic nature of the particle's position and the nature of the force at that position, but the causal relation of the force's direction has not been. Here I thought, “Does this direction depend on the position of the particle?” Surely not, the particle distribution only tells us where the particle could be and how it might spread in different regimes, but it's just a point, its distribution implies no constraints on the direction of the force, i.e., at most we can say that the force “follows” the point, but it is independent in its orientation at that point. Which direction does the force go at that point, well surely the baby knows, maybe he's got his own reasoning for why he shot in one direction or the other. Perhaps he prefers shooting bubbles at his uncle (I was unfortunate), or his aunt, but these are trivialities and not important to the discussion, so instead we'll assume the baby shoots in uniformly random direction $\theta_i$ with the distribution $\rho(\theta_{i})=\frac{1}{2\pi}$ so the force at any point is randomly sampled from the unit circle between $0\leq\theta_i\leq 2\pi$. We could complicate the direction distribution's profile by saying $\theta_i$'s are constrained by the particle's distance to the boundary, so that at the right wall the available angles to sample should be defined by the half plane $\frac{\pi}{2}\leq\theta_i\leq\frac{3\pi}{2}$ and similarly on the left wall, $\frac{3\pi}{2}\leq\theta_i\leq\frac{\pi}{2}$ and the top and bottom wall can be defined in a similar fashion. This is accurate, but adds unnecessary complexity so we'll simply say that the force is 0 at the boundaries, since we lose no interesting physics there, and call it a day. 

**Discussion & Implications**
Putting this all together, we can begin to answer, some, of my initial questions, and conjecture about the rest. While I haven't tried to make any explicit calculations, we can already see how the system behaves without it. 

If we let our timescale $t\to t_c^{-}$ our variance shifts to an exponential decay, which means as the system evolves over this small time window, the probability of finding the particle and subsequently its force nearby is more likely than finding it across the room. This aligns with my conjecture (C2) and gives us a way of studying the dynamics in a more deterministic way. Over a sufficiently small window $t_i+\epsilon(t)$ for a particle at position $p=(x_t,y_t)=\textbf{r}_\sigma$ we can expect there to be a force concentration. We can sum up the force contribution over this window to get the net force: $$F_{\text{net}}=\int_{t_i+\epsilon(t)} \textbf{F}(\textbf{r})\,dr = \int_{t_i+\epsilon(t)} \frac{k}{||\delta(\textbf{r}-\langle x_t,y_t \rangle) - \textbf{r}_i||}\hat{d}_i$$
since we shouldn't expect the bubbles to wander too far (in the plane) from the muzzle of the gun in this window, i.e., $\textbf{r}_i \sim 0$, these forces reduce to their magnitude and directional component $k\hat{d}$ and thus the net force is simply $$F_{\text{net}}=\int_{t_i+\epsilon(t)} k\hat{d}_i$$
Since the direction of these forces are random $\theta_i$ but constant in $k$, we can reason that $F_{\text{net}}\to 0$ as $t\to\infty$ so we get an emergent conservation law! 

The probability of a force being at some point $p=(x_t,y_t)$ should then be higher in this regime, can be written as $$P\big(F_{\epsilon(p_t)}\big)=\int_{t_i+\epsilon(t)} \epsilon(p_t)\rho(x,y,t)$$
which should not be 0 in the small time window, but decay exponentially in time, i.e., $P\big(F_{\epsilon(p_t)}\big) \to 0$ as $t\to t_c$. This makes sense since we expect the net force to vanish, but if there were always some non-zero probability of a force at a point, then we might have non uniformity in force concentration and perhaps the conservation would not hold.

Moving to the large timescale regime, i.e., for $t>t_c$ the probability of our force “popping up” at any given point $p_t=(x_t,y_t)$ goes to 0 for sufficiently large time windows. However, since for any point $p_t$ we can find a small window where the dynamics converge to the $t\leq t_c$ regime, it should be the case that $$F_{\text{total net}}=\sum_{j=0}^n \bigg(\int_{t_j+\epsilon_j(t)} \textbf{F}(\textbf{r})\,dr \bigg)=0$$
and hence globally the forces cancel out. This shouldn't be surprising since the directions of the forces are uniformly random and constant and hence every direction the baby can shoot, will have been shot in. 

**Conclusion** 
At this point I've said everything I wanted to about the equations, my derivations, and reasoning without making any explicit and tedious calculations! I've also answered most of the guiding questions I proposed at the start. All that is left is to have a discussion about what physics has already, or, could emerge from the model. To conclude I'll imagine a few scenarios and see what I can find. Suppose our geometry, i.e., our boundary wasn't fixed, but instead could be pushed by our forces. Then, we'd essentially arise at a system that looks strikingly similar to a gas in a box that can expand and push the boundaries. In that case, we'd likely find solely from this model's predictions, that the box would scale uniformly since the internal net force is 0 and directions are equally likely. There are more connections to be made, specifically in discussing the evolution of our density, the regime changes, the transition in behavior at different scales, the force collapsing, etc., but for now I'll leave it there.

