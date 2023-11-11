# Lecture 2. Numerical scheme in Liouville formalism

## About the nature of equations

The considered equations of motion have a Lyapunov divergence.

Let $r$ be the exact solution of the equation:

$$
r(t) = f(r(0), p(0), t)
$$

There is also solution, obtained with a small shift $\epsilon$ of the initial state:

$$
r'(t) = f(r(0), p(0) + \epsilon, t)
$$

Then their difference is:

$$
|\Delta r(t)| = |r'(t) - r(t)| \approx \epsilon \exp(\lambda t)
$$

Here $\lambda$ are Lyapunov eigenvalues, which are determined by this system and are becomes large, due to which there is a big difference between the final positions with a small shift in the initial states.

![Divergence of trajectories of water molecules](images/fig3_divergence.png)

For example, on the graphs below of the dependence of the mean square of the coordinate difference on time for a system of four water molecules, it can be seen that with small differences of $\varepsilon$ compared to the bond length of 1 Angstrom in the initial positions, after a certain number of steps, the measured quantities become completely independent.

To calculate the average value of any quantity, this is not important, since the main thing is to stay on a given hypersurface, however, rapid divergence due to small initial offsets becomes  important when it is necessary to calculate correlation functions.

As a standard, if the values were correlated, then there is some decline. When modeling, it is necessary that the coordinate does not diverge during the characteristic correlation time.

In addition, even if the trajectory does not differ much from the true one, the question arises about long-term stability - is there a large distance $\lvert E(x(t +\Delta t)) - E_0\rvert$ from a given plane.

There are observations that are not strictly proven that when using algorithms with the following properties:

- Reversibility in time $\Delta t\leftrightarrow - \Delta t$ 
- Symplecticity or energy conservation $E = const$
- Phase space conservation $\Delta q\Delta p = const$

It turns out that:

1. The trajectories obtained using such algorithms are shadow trajectories that do not lag far behind the real ones
2. These methods exactly preserve the Hamiltonian $\tilde H = H+O(\Delta t^2)$, if there are no rounding errors. (proved)


### Where does this come from?

Let the Hamiltonian $H(q, p)$ be given, then:

$$
\dot q = \frac{\partial H}{\partial p}; \quad \dot p = - \frac{\partial H}{\partial q}
$$

The evolution of a function of coordinates can be written using the Liouville operator $L$:

$$
\frac{da(q, p)}{dt} = \sum_{\alpha} \frac{\partial a}{\partial q} \dot q + \frac{\partial a}{\partial p} \dot p = 
\sum \frac{\partial H}{\partial p} \frac{\partial a}{\partial q} - \frac{\partial H}{\partial q} \frac{\partial a}{\partial p} = \{a, H\} = i L a
$$

It turns out a differential equation with known initial conditions, the solution of which can be found:

$$
\frac{da}{dt} = i L a \quad t = 0: x_0 \\
a(x(t)) = \text{e}^{iLt}a(x(t))
$$

Based on the reflections above, it is possible to draw up a scheme for a numerical solution:

$$
x = \begin{pmatrix}q \\ p\end{pmatrix} \Rightarrow x(t) = \text{e}^{iLt}x(0)
$$

To understand the usefulness of this formulation, we divide the Liouville operator into two parts corresponding to each of the terms in Poisson brackets:

$$
iL = iL_1 + iL_2 = \frac{\partial H}{\partial p} \frac{\partial }{\partial q} - \frac{\partial H}{\partial q} \frac{\partial }{\partial p}
$$

For example, consider a particular one-dimensional problem:

$$
H = \frac{p^2}{2m} + U(r) \Rightarrow
iL_1 = \frac{p}{m} \frac{\partial}{\partial r}; \quad iL_2 = F(r) \frac{\partial}{\partial p}
$$

Note that these two operators do not commutate:

$$
[iL_1, iL_2] \ne 0
$$

From which it follows that (See Baker–Campbell–Hausdorff formula)

$$
\text{e}^{iL_1 + iL_2} \ne \text{e}^{iL_1} + \text{e}^{iL_2} 
$$

Then it is necessary to use Trotter's theorem:

$$
\text{e}^{A+B} = \lim_{p \rightarrow \infty} \left[ \text{e}^{\frac{B}{2p}}\text{e}^{\frac{A}{p}}\text{e}^{\frac{B}{2p}}\right]^p
$$

In our case, instead of $p$, there will be $\frac{t}{\Delta t}= M$ - the number of steps

$$
\text{e}^{iLt} = \text{e}^{iL_1t +iL_2t} = \lim_{p \rightarrow \infty} \left[ \text{e}^{\frac{iL_2t}{2p}}\text{e}^{\frac{iL_1t}{p}}\text{e}^{\frac{iL_2t}{2p}}\right]^p = 
\lim_{\Delta t \rightarrow 0} \left[ \text{e}^{\frac{iL_2 \Delta t}{2}}\text{e}^{iL_1 \Delta t}\text{e}^{\frac{iL_2 \Delta t}{2}}\right]^{\frac{t}{\Delta t}}
$$

For this limit , there is the following estimate:

$$
\text{e}^{iLt} = \left[ \text{e}^{\frac{iL_2 \Delta t}{2}}\text{e}^{iL_1 \Delta t}\text{e}^{\frac{iL_2 \Delta t}{2}}\right]^{M} + O(M\Delta t^3)
$$

That is, the evolution of the system from the state at time $t$ to the state at time $t + \Delta t$ is given by the operator:

$$
\text{e}^{\frac{iL_2 \Delta t}{2}}\text{e}^{iL_1 \Delta t}\text{e}^{\frac{iL_2 \Delta t}{2}} + O(\Delta t^3)
$$

Consider, for example, action of the operator $\text{e}^{iL_2\frac{\Delta t}{2}}$.
Taylor series of an exponential function

$$
\text{e}^{c \frac{\partial}{\partial x}}g(x) = \sum \frac{1}{n!} c^n \frac{\partial^n g(x)}{\partial x^n} = g(x+c)
$$

It turns out that in fact this operator acts as a shift, that is:

$$
\text{e}^{iL_2\frac{\Delta t}{2}} \begin{pmatrix} x \\ p\end{pmatrix} \rightarrow \begin{pmatrix} x \\ p  + \frac{F(x) \Delta t}{2} \end{pmatrix}
$$

Similarly, the $L_1$ operator will shift the position by coordinate.

<!-- That is, the action of the approximate operator $\tilde L$, similar to $\tilde H$, with a certain accuracy, will consist in the sequential application of the momentum shift, the coordinate shift, and again the momentum shift. -->

## Integrators

### Revisiting velocity-Verlet method

The state of the system at $t=\Delta t$ can be obtained from the state at $t=0$ with a help of Liouville operator

$$\begin{pmatrix}r(\Delta t) \\ p(\Delta t)\end{pmatrix} = \text{e}^{iL\Delta t} \begin{pmatrix}r_0 \\ p_0\end{pmatrix}  = \underbrace{\text{e}^{iL_2\frac{\Delta t}{2}}\underbrace{\text{e}^{iL_1\Delta t}\underbrace{\text{e}^{iL_2\frac{\Delta t}{2}} \begin{pmatrix}r_0 \\ p_0\end{pmatrix}}_{1}}_{2}}_{3} $$

Here and after $p_t = p(t)$, $r_t=r(t)$.
Acting with operators step-by-step:
1. $$\text{e}^{iL_2\frac{\Delta t}{2}} \begin{pmatrix}r_0 \\ p_0\end{pmatrix} = \text{e}^{F(r_0)\frac{\Delta t}{2} \frac{\partial}{\partial p}} \begin{pmatrix}r_0 \\ p_0\end{pmatrix} \\ =  \begin{pmatrix}r_0 \\ p_0 + F(r_0)\frac{\Delta t}{2}\end{pmatrix}$$
2. $$\text{e}^{iL_1\Delta t} \begin{pmatrix}r_0 \\ p_0 + F(r_0)\frac{\Delta t}{2}\end{pmatrix} = \text{e}^{\frac{p_0}{m}\Delta t \frac{\partial}{\partial r}} \begin{pmatrix}r_0 \\ p_0 + F(r_0)\frac{\Delta t}{2}\end{pmatrix} \\ = \begin{pmatrix}r_0 + \frac{p_0}{m}\Delta t\\ p_0 + F\left(r_0 + \frac{p_0}{m}\Delta t\right)\frac{\Delta t}{2}\end{pmatrix}$$
3. $$\text{e}^{iL_2\frac{\Delta t}{2}}\begin{pmatrix}r_0 + \frac{p_0}{m}\Delta t\\ p_0 + F\left(r_0 + \frac{p_0}{m}\Delta t\right)\frac{\Delta t}{2}\end{pmatrix} =  \text{e}^{F(r_0)\frac{\Delta t}{2} \frac{\partial}{\partial p}} \begin{pmatrix}r_0 + \frac{p_0}{m}\Delta t\\ p_0 + F\left(r_0 + \frac{p_0}{m}\Delta t\right)\frac{\Delta t}{2}\end{pmatrix} \\ = \begin{pmatrix}r_0 + \frac{1}{m}\left(p_0 + F(r_0)\frac{\Delta t}{2}\right)\Delta t\\ p_0 + F(r_0)\frac{\Delta t }{2} + F\left(r_0 + \frac{1}{m}\left(p_0 + F(r_0)\frac{\Delta t}{2}\right)\Delta t\right)\frac{\Delta t}{2}\end{pmatrix} = \begin{pmatrix}r_{\Delta t} \\ p_{\Delta t}\end{pmatrix}$$

By assigning $v(t)=\dfrac{p(t)}{m}$, the result can be rewritten

$$\begin{cases}r_{\Delta t} = r_0 + v_0\Delta t + \frac{F(r_0)}{m}\frac{\Delta t ^2}{2} \\ v_{\Delta t} = v_0 + \frac{\Delta t}{2}\left(\frac{F(r_0)}{m} + \frac{F(r_{\Delta t})}{m}\right)\end{cases}$$

Timestamps can be changed to indexes (considering time steps are equal)

$$\begin{cases}r_{i+1} = r_i + v_i\Delta t + \frac{F(r_i)}{m}\frac{\Delta t ^2}{2} \\ v_{i+1} = v_i + \frac{\Delta t}{2}\left(\frac{F(r_i)}{m} + \frac{F(r_{i+1})}{m}\right)\end{cases}$$

One can see this iteration formulas coincide with Velocity-Verlet method formulas (see previous lecture) and represents one step of integration algorithm.

### Revisiting Leapfrog Verlet

It is possible to obtain leapfrog integration from the previous method.
Suppose two time steps:
1. $t=-\Delta t$ to $t=0$ 
2. $t=0$ to $t=\Delta t$.

![Two steps](images/fig1.png)

It is possible to "shift" the scheme by introducing auxiliary time steps $t=-\frac{\Delta t}{2}$ and $t=\frac{\Delta t}{2}$:

![Auxiliary steps](images/fig2.png)

Relation between system state at $t=\dfrac{\Delta t}{2}$ and  $t=\dfrac{-\Delta t}{2}$ is

$$\begin{pmatrix}r_{\Delta t/2} \\ p_{\Delta t/2}\end{pmatrix} = \text{e}^{iL_2\frac{\Delta t}{2}}\text{e}^{iL_2\frac{\Delta t}{2}}\text{e}^{iL_1\Delta t}\begin{pmatrix}r_{-\Delta t/2} \\ p_{-\Delta t/2}\end{pmatrix} = \text{e}^{iL_2\Delta t}\text{e}^{iL_1\Delta t}\begin{pmatrix}r_{-\Delta t/2} \\ p_{-\Delta t/2}\end{pmatrix}$$

where $\text{e}^A\text{e}^A = \text{e}^{2A}$ is used (due to the fact $A$ and $A$ commute). 
Similarly to the above, the action of the operators can be described step by step

$$\text{e}^{iL_2\Delta t}\text{e}^{iL_1\Delta t}\begin{pmatrix}r_{-\Delta t/2} \\ p_{-\Delta t/2}\end{pmatrix} = \text{e}^{iL_2\Delta t} \begin{pmatrix}r_{-\Delta t/2} + \frac{ p_{-\Delta t/2}}{m}\Delta t \\ p_{-\Delta t/2}\end{pmatrix} \\ = \begin{pmatrix}r_{-\Delta t/2} + \frac{ p_{-\Delta t/2} + F\left(r_{-\Delta t/2}\right)\Delta t}{m}\Delta t \\ p_{-\Delta t/2} + F\left(r_{-\Delta t/2}\right)\Delta t\end{pmatrix} = \begin{pmatrix}r_{\Delta t/2} \\ p_{\Delta t/2}\end{pmatrix}$$

From the last equality it follows that

$$\begin{cases}p_{\Delta t/2} = p_{-\Delta t/2} + F\left(r_{-\Delta t/2}\right)\Delta t \\ r_{\Delta t/2} = r_{-\Delta t/2} + \frac{p_{\Delta t/2}}{m}\Delta t \end{cases}$$

What $p_{\pm\Delta t/2}$ and $r_{\pm\Delta t/2}$ actually mean?

$$\begin{pmatrix}r_{\Delta t/2} \\ p_{\Delta t/2}\end{pmatrix} = \text{e}^{iL_2\frac{\Delta t}{2}} \begin{pmatrix}r_0 \\ p_0\end{pmatrix} = \begin{pmatrix}r_0 \\ p_0 + F(r_0)\frac{\Delta t}{2}\end{pmatrix} $$

Therefore, $r_{\Delta t/2} = r_0$, $p_{\Delta t/2} = p_0 + F(r_0)\frac{\Delta t}{2}$.
In the same way it can be shown that $r_{-\Delta t/2} = r_{-\Delta t}$, $p_{-\Delta t/2} = p_{-\Delta t} + F\left(r_{-\Delta t}\right)\frac{\Delta t}{2}$.

Now it is possible to write the method explicitly

$$\begin{cases}p_{\Delta t/2} = p_{-\Delta t/2} + F\left(r_{-\Delta t}\right)\Delta t \\ r_0 = r_{-\Delta t} + \frac{p_{\Delta t/2}}{m}\Delta t \end{cases}$$

Initial value of auxiliary momentum can be computed as $p_{-\Delta t/2} = p_{-\Delta t} + F(r_{-\Delta t})\frac{\Delta t}{2}$.

Rewriting with indexes instead of timestamps

$$\begin{cases}p_{i+1/2} = p_{i-1/2} + F(r_{i-1})\Delta t \\ r_i = r_{i-1} + \frac{p_{i+1/2}}{m}\Delta t \end{cases}$$

$$p_{i-1/2} =p_{i-1} + F(r_{i-1})\frac{\Delta t}{2}$$

For clarity we shift the indices of the "real" $r$ variable, while keeping the indices of the auxiliary variable $p_{i\pm1/2}$.

$$\begin{cases}p_{i+1/2} = p_{i-1/2} + F(r_i)\Delta t \\ r_{i+1} = r_{i} + \frac{p_{i+1/2}}{m}\Delta t \end{cases}$$
$$p_{i-1/2} =p_{i} + F(r_{i})\frac{\Delta t}{2}$$

These formulas express leapfrog integration method.
The initial value for $i=0$: $p_{-1/2} = p_0 + F_0\frac{\Delta t}{2}$.

### Integration errors 

The methods Described above are symplectic integrators.
One of the properties of such integrators is preservation on the total energy of the system. In other words, the system neither warms up nor freezes with time.

When using the approximation

$$\text{e}^{iLt}\approx \left[\text{e}^{iL_2\frac{\Delta t}{2}}\text{e}^{iL_1\Delta t}\text{e}^{iL_2\frac{\Delta t}{2}}\right]^M = \text{e}^{i\widetilde{L}t}$$

it turns out that the system numerically evolves not with the original operator $L$, but with some $\widetilde{L}$, which corresponds to some Hamiltonian $\widetilde{H}$ not equal to the original $H$.
The difference between $\widetilde{H}$ and $H$ determines the order of the symplectic integrator.
For example, the semi-implicit Euler method is the first-order integrator. Semi-implicit Euler method can be obtained by considering the approximation

$$\text{e}^{i\widetilde{L}\Delta t} = \text{e}^{iL_1\Delta t}\text{e}^{iL_2\Delta t}$$

and using the same approach as for velocity-Verlet method above.
It can be proven that in this case effective Hamiltonian differs from $H$ by some term proportional to $\Delta t$: $\widetilde{H}=H + O(\Delta t)$.
For velocity-Verlet method, which is second-order method, $\widetilde{H}=H + O(\Delta t^2)$.

Symplectic methods can be combined with each other to obtain high-order integrator and to get $\widetilde{L}$ closer to $L$.
For example, [in this work](https://doi.org/10.1016/0375-9601(90)90092-3) the method of designing symplectic integrators of 4, 6 and 8 orders is presented.
The 4th order integrator requires multiplication of 7 matrix exponentials.
As the order of the integrator increases, the formulas for one step grow.
