**Overview**
I present a method for measuring average global change of a manifold under iterative deformation transformation, which provides a new perspective on the analysis of iterative mappings of manifolds and global curvature. This framework can be applied to a multitude of problems in cosmology, manifold learning, and computational geometry.

**Background**
A common question in applied differential geometry is, given a manifold $M$ (base manifold) with metric $g_{ij}$ and some transformation $T_{T(\textbf{p})}: M \to N$ mapping $M$ into (possibly onto) some other manifold $N$, how does this transformation affect the global geometry of the base manifold? One way is to look at how the curvature changes under this transformation, which is to say we measure how the vectors in the tangent spaces $T(\textbf{p})$ at each point $\textbf{p}$ of the base manifold change under the transformation.

Typically, this measuring of global change under the mapping would be done by comparing the metric(s) $g_{ij}$ in the base manifold to the metric(s) $g'_{ij}$ in the target manifold $N$. This is done by ***pulling back*** the metric $g'_{ij}$ onto M and comparing the two there. While, this is elegant, for non-trivial geometries and non-isometric transformations, computing these comparisons or even finding a closed form of the original metric becomes increasingly challenging and requires solving challenging partial differential equation.

**Concept**
Rather than trying to acquire a closed form of the metric $g_{ij}$ then comparing $g_{ij}$ with $g'_{ij}$ under the mapping $T$, we can instead establish a smooth global averaged measure of change in a way that is independent of the metric. Recall that under a transformation $T: M \to N$, the *Jacobian* of $T$ is the differential *pushforward* of T, denoted as 

$$
dT_p:T_pM\to T_{T(p)}N
$$

which we identify as a transformation taking tangent vectors at points in 

$M$, to tangent vectors at $T(p)$ in N. In local coordinates we define the Jacobian as 
$$
J_i^a = \frac{\partial T^a}{\partial x^i}
$$

. Now in a purely abstract setting, we might not be so concerned with these derivates, but more so in how the volume elements transform with it, which tells us something about the transformation $T$ i.e., namely whether it's an isometry, conformal, or neither. However, if our goal is to get an average sense of how the geometry changes under the mapping, we can establish a system for doing so by acknowledging that in a discretized setting, the Jacobian is computable or can be approximated at each point. If we restrict ourselves to a region $\omega \subset M$ where $\omega$ is chosen in a way that is computationally feasible, i.e., by uniform sampling, adaptive, etc, then the total volume change ($d V'$) can be given as follows 
$$
dV' = \int_{\omega}|\text{det J}|dV
$$

. This gives us a measure of global change under the transformation, but not yet a local measure for developing a global average. To do this, we note that we are integrating over a region, and hence summing over some smaller region is just a discretization, thus we have 
$$
\sum_{p\in\omega}^{n} |\text{det J}_p|
$$

where $n\leq\omega$. Since $n$ is the number of points, we can define an average change by simply dividing the sum by $1/n$, thus we have identified the average change under the transformation as 
$$
\frac{1}{n}\sum_{p\in\omega}^{n} |\text{det J}_p|
$$

 and switching back to a continuous setting, this we have 
$$
dV_{\text{avg}}' = \frac{1}{\text{vol}(\omega)}\int_{\omega}|\text{det J}|dV
$$

thus, we have constructed a way to measure the average change in a purely computational manner given an analytic transformation $T: M \to N$

**Iterative Mappings**
So far we have constructed the technique around a single transformation, but we can just as easily extend it to successive iterations of a mapping. Suppose we apply a transformation $T:M\to N$ in succession $n-$times, we'll denote this as $T^n:M_i\to N_i$ for $i\leq n$. It should be noted that we apply each successive map to the same region, points sampled from this region can change, but the region should remain consistent. It'll help to denote our average change $dV_{\text{avg}}'$ as a function $\phi(T): T \to \mathbb{R}$ which is a function that maps a transformation to a real number such that it averages up all the jacobians at each point under the transformation. Then, it's clear to see that $\phi(T^n): T \to \mathbb{R}$ is just the sum of the averages under each transformation divided by $n$, thus we define the average change after n-successive applications of the map $T$ as

$$
\mathbb{E}\left(\phi(T^n) \right)= \frac{1}{n}\sum_{i=0}^n \phi(T^i)
$$


Note we'll adopt the expectation value notation to be more concise. If $T$ is some transformation that moves our manifold forward in time by some evolutionary dynamics, then this tells us the average change in our geometries over time. One natural question we might ask is what happens to the expectation value in the limiting case of $n$, this formulation gives us a natural way of answering that question both qualitatively and quantitatively. This might be seen as computationally efficient analogy to studying trajectories in the phase space of a dynamical system. We can study long term behavior by determining whether this expectation is bounded for large $n$, by introducing an infinitesimal perturbation $\delta F /\delta T$. If $T$ is a mapping $T:M\to N$ and our averaging function depends on the mapping, $\phi(T)$, then we have 
$$
\phi(T+\delta T)-\phi(T)=\int_M  \frac{\delta F}{\delta T}(x)\delta T(x)dx
$$

thus the change in expectation over successive iterations is 
$$
\mathbb{E}\left(\phi(T+\delta T)-\phi(T)\right) = \mathbb{E}\left(\phi(T+\delta T)\right)- \mathbb{E}\left(\phi(T)\right)
$$

For the $i^{th}$ iteration of the mapping, if we let $T+\delta T = T^{i}+ T^{i+1}$ we can rewrite the change in expectation as 
$$
\delta F /\delta T=\mathbb{E}\left(\phi(T^{i}+ T^{i+1})\right)- \mathbb{E}\left(\phi(T^i)\right)
$$

. From this, we have an effective framework for developing probabilistic bounding arguments on long term behavior of any general Riemannian geometry under iterative mappings.

**Computational Notes**
The bulk of the computational complexity for the technique comes from computing the derivatives in the jacobian, which can be made fast, but grows linearly $O(n)$ with the number of $n -$points sampled from the region of the manifold $M$ you are studying. For setups where you are applying iterative maps, this dependence grows as $O(nm)$ where $m$ is the number of iterations. Note that the number of points $n$ should be the same for each mapping, i.e., its fixed for all iterations $m$. Assuming the manifold is discretized into an $n \,\text{x} \,n$ grid, the overall complexity would typically be $O(mn^2)$, but because of the average doesn't require all points in the grid, we can reduce the polynomial complexity to something linear.

The method could be extended to incorporate more geometric information from the spectral decompositions of the jacobians, but this would require diagonalization at each step in the averaging process, which can be costly depending on your environment. However, independent nature of the technique lends itself to parallelization naturally. In theory, you could batch sample points in your region across different threads and compute the jacobians and their respective decompositions in parallel, then join them back for averaging on the main thread.
