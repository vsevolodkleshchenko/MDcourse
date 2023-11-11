# Lecture 4. Controlling temperature

## NVE and NVT ensembles
Consider a system in which the interaction of particles is determined by forces $f(r_{ij})$. In this system there will be a constant number of particles $N$, the volume of the system $V$ and the energy $E$ (total isolation), i.e. *microcanonical ensemble*. The volume of the system will be taken into account using for example periodic boundary conditions.

For the Hamiltonian we can write:

$$
H(p, r) \rightarrow 
\begin{cases}
\dot p = - \frac{\partial H}{\partial q} \\
\dot q =  \frac{\partial H}{\partial p}
\end{cases}, \quad
H(p, r) = E
$$

In thermodynamics, assuming ergodicity, average value of some quantity $a$ can be found as following:

$$
\left<a(x)\right> = \frac{\int dx a(x) \delta(H(x) - E_0)}
{\int dx \delta(H(x) - E_0)} = \frac1T \int_0^T a(x(t)) dt
\approx \frac1M\sum_{n=1}^M a(n\Delta t),
$$

where $x= \begin{pmatrix} r \\ p \end{pmatrix}$, $M$ - the number of time steps $\Delta t$.

However, in the real system there is always a thermodynamic bath. Also, it is usually assumed that there is a smaller subsystem in some large system that exchanges heat with it, and as a result there is a temperature $T$. Therefore we would like to explicitly sample the *canonical ensemble* (NVT). For this purpose we need to be able to control the temperature of our
system.

## Velocity rescaling
 We already know that every degree of freedom has energy $\frac{kT}2$. 
The average of the Boltzmann distribution:

$$
\left<a(x)\right> = \frac{
    \int dx a(x) \exp(-\frac{\frac{p^2}{2m} + U(r)}{kT})}{
    \int dx \exp(-\frac{\frac{p^2}{2m} + U(r)}{kT})} = 
\frac1T \int_0^T a(x(t))
$$

For $p$ we use Maxwell distribution:
$$
f(p) = \left(\frac1{2\pi nkT}\right)^{3/2}\exp\left(-\frac{p^2}{2mkT}\right)
$$

and the momentum of this distribution can be obtained:

$$
\frac{3kT}2 = \frac{m\left<|v|^2\right>}2 \quad \rightarrow  \quad \left<p^2\right> = 3kTm, \left<p^4\right> = 15(kTm)^2
$$

If we want to keep the average speed, then we can do *velocity rescaling*. However, we should note, that as the result we may obtain another system with specified kinetic energy but wrong distribution.

The simplest way is to check the relative fluctuations of the square of the momentum $\frac{\sigma_{p^2}}{\left<p^2\right>^2}$ and total kinetic energy (kinetic temprature) $\frac{\sigma_{T_k}^2}{\left<T_k^2\right>^2}$.

Then the square of the velocity of any molecule fluctuates as:

$$
\frac{\sigma_{p^2}}{\left<p^2\right>^2} = \frac{\left<p^4\right> - \left<p^2\right>^2}{\left<p^2\right>^2} = \frac{15-9}{9} = \frac23
$$

And for kinetic temperature, which is defined as
$\left<\sum\frac{mv^2}2\right> = \frac{3NkT}2$:

$$
T_k \sim \frac1N \sum p_i^2, \quad 
\left<T_k\right> \sim \frac1N \sum\left<p_i\right>^2 = \left<p^2\right> \rightarrow \\
 \quad \left<T_k^2\right> \sim \frac1{N^2} \left<\left(\sum p_i^2\right)\right> = \frac1{N^2}\left(N\left<p^4\right> + N(N-1)\left<p^2\right>\left<p^2\right>\right)
$$

Then 

$$
\frac{\sigma_{T_k}}{\left<T_k\right>^2} = \frac{\left<T_k^2\right> - \left<T_k\right>^2}{\left<p^2\right>^2} = \frac1{N^2} 
\frac{\frac1{N^2}\left(N\left<p^4\right> + N^2\left<p^2\right>^2 - N\left<p^2\right>\right) - \left<p^2\right>^2}{\left<p^2\right>^2} = \\ = \frac{\left<p^4\right> - \left<p^2\right>^2}{N\left<p^2\right>^2} = \frac2{3N}
$$

If we need to control the temperature, it is necessary to take care that all the moments are correct. Accordingly, if we have made a method that will keep the average value constant, then the variation is $0$.

Thus, you can use only methods in which it is proved that everything remains correct.

- *Isokinetic method*: the sum of squares is constant. This is not very good method, because if $\left<p^2\right> = 0$ then $\sigma_{T_k} = 0 $ 

Further, there are better methods that can be conditionally divided into two classes:

- Stochastic methods: In *Stochastic collisions* (*Anderson thermostat*) method periodically random impacts on molecules into the system are introduced. At some point, the momentum of some molecule changes, so that the distribution remains correct.
- Extended methods: for example *Nose-Hoover thermostat*, where system samples not constant energy, but by distribution, while the dynamics are reserved.

$$
\delta(E-H(p, r)) \quad\rightarrow\quad \exp(-\frac{H(p(r))}{kT})
$$

Thus, we can formulate the statement of the problem: we want to obtain somehow one state after another (sample) in our system of $N$ atoms (possibly with periodic boundary conditions) so that the average is obtained precisely in the understanding of $NVT$:

$$
\left<a(x(t))\right> = \frac1M \sum_{n=1}^M a(n\Delta t) = 
\frac{\int dx a(x) \exp(-\frac{H(x)}{kT})}{\int dx \exp(-\frac{H(x)}{kT})} = \left<a(x)\right>_{NVT}
$$

All theese algorithms are called thermostats because they result in a constant temperature being maintained in the system.

## Stochastic collisions

Let's assume that periodically with probability $\nu$ $([\nu] = 1/s)$ something collides with the particle, so that its velocity changes as $v\rightarrow v_M$, where $v_M$ is the velocity from the Maxwell distribution $f(v)$.

From an algorithmic point of view, let's consider the change of the system when moving from the $0$ moment of time to  $\Delta t$:

$$
\left[r^n(0), p^n(0) \right] \rightarrow \left[r^n(\Delta t), p^n(\Delta t) \right]
$$

Then the colliding will work as follows:

```
for i in range(1, N):
    if is_reached():
        v[i] = Gauss()  # Maxvell distribution
```
where *is_reached* - is a function that determines has the value of $\nu\Delta t$ reached a sufficient value to apply the collision.

Advantages:
- Simple
- Сorrect distribution is maintained

Disadvantages:
- Lost dynamics: the velocity correlation function must decrease exponentially $\left<v(0)v(t)\right> \sim \exp(-\nu t)$

## Nose-Hoover thermostat
<!-- Also, we want to get a method that will also 'walk between planes', but at the same time the dynamics will be preserved. -->
We want to get such a method that the dynamics will be preserved. To achieve it, the system can be expanded by adding a variable $s$ that scales velocities.

Consider the Lagrangian:

$$
\mathcal{L}_{Nose} = \sum_{i=2}^n \frac{m_i}2 (s\dot r_i^2) - U(r_N) + \frac{Q}2 \dot s ^2 - LkT \ln s
$$

Here $Q$ is an analogue of mass, $L$ is a constant, the meaning of which will be understood later.

Now, to move on the Hamiltonian with $6N +2$ degrees of freedom:
$$ 
\begin{cases}
    p_i = \frac{\partial \mathcal{L}}{\partial \dot r_i} = m_i s^2 \dot r_i \\
    p_s = \frac{\partial \mathcal{L}}{\partial \dot s} = Q\dot s
\end{cases} \Rightarrow
H = \sum \frac{p_i^2}{2m_is} + U(r^N) + \frac{p_s^2}Q + L kT\ln s
$$

Let's see how the integration and calculation of averages will take place based on it. Consider the partition function:

$$
Q_N = \frac1N\int dp_s ds dp^N dr^N \delta (H_N(s, p_s, p, r) - E)
$$

Introducing $p' = \frac{p}{s}$ и $H'(p, r) = \sum \frac{p_i'^2}{2m_is} + U(r^N)$, we obtain:

$$
Q_N = \frac1N\int dp_s ds dp'^N dr^N s^{3N} \delta \left(
    H'(p, r) + \frac{p_s^2}Q + L kT\ln s \right) = \\
\frac1N\int dp_s ds dp'^N dr^N s^{3N} \delta \left[LkT\left(\ln s  - \frac{-H'(p, r) - \frac{p_s^2}Q - E}{LkT} \right) \right]
$$

Denote $\ln(s_0) = \frac{-H'(p, r) - \frac{p_s^2}Q - E}{LkT}$ then integration will be as follows:

$$
Q_N = \frac1N\int\dots ds s^{3N} \delta \left(LkT\ln(\frac{s}{s_0})\right) = \frac1N\int ds \frac{s_0^{3N+1}}{LkT} dp_s dp'^N dr^N = \\
= \frac1{N!LkT}\int dp_s dp'^N dr^N \exp\left(
    -\frac{3N+1}{L} \frac{H(p', r) + p_s^2/Q - E}{kT} \right) \Rightarrow \\
Q_N = \frac{c}{N!}\int dp'^N dr^N \exp\left(
    -\frac{3N+1}{L} \frac{H(p', r) + p_s^2/Q - E}{kT} \right) 
$$

If we take $L=3N+1$ then we will get what we need. It turns out that the average value of some function along the trajectory is

$$
A(p', r) = \lim_{T\rightarrow\infty}\int_0^T dt A\left(\frac{p(t)}{s(t)}, r(t)\right) = \left<A\left(\frac{p}{s}, r\right)\right>
$$

and the average value of the distribution is actually obtained the same as the average of the ensemble:

$$
\left<A\left(\frac{p}{s}, r\right)\right>_{Nose} = \frac{
    \int dp'^N dr^N A\left(p', r\right) \exp\left(-\frac{H(p', r)}{kT} \right)}{
    \int dp'^N dr^N \exp\left(-\frac{H(p', r)}{kT} \right)} = 
\left<A\left(p', r\right)\right>_{NVT}
$$

Taking into account all variables, when we expand the Lagrangian, using its form, we can get that $\dot r '= s\dot r$ and the time is scaled depending on $s$:

$$
dt' = \frac{dt}{s} 
$$

Consider the mean value. We will integrate according to the scaled time so that not to spoil anything, and to obtain correct distribution.

$$
\lim_{T'\rightarrow\infty} \frac1{T'}\int_0^{T'} dt' A\left(\frac{p(t')}{s(t')}, r(t') \right) = 
\lim_{T'\rightarrow\infty} \frac{T}{T'} \frac1{T} \int_0^{T} dt A\left(\frac{p(t)}{s(t)}, r(t)\right) \frac1{s(t)} = \\
= \frac{
    \lim_{T'\rightarrow\infty} \frac1{T} \int_0^{T} dt A\left(\frac{p(t)}{s(t)}, r(t)\right) \frac1{s(t)}}{
    \lim_{T'\rightarrow\infty} \frac1{T}\int_0^T\frac{dt}{s(t)} 
} = \frac{\left<\frac{A}{s}\right>}{\left<\frac1{s}\right>}
$$

As a result, we obtain not what we wanted:

$$
\bar A = \lim_{T'\rightarrow\infty} \frac1{T'}\int_0^{T'} dt' A\left(\frac{p(t')}{s(t')}, r(t') \right) = 
\frac{\left<A\left(\frac{p}{s}, r\right)\frac1{s}\right>}{\left<\frac1{s}\right>} = \\ =
\frac{\int dp_s ds dp^N dr^N s^{3N}A\frac1{s}}
{\int dp_s ds dp^N dr^N s^{3N} \frac1{s}} = |L=3N| = 
\left<A\left(p', r\right)\right>_{NVT} \bigg|_{L=3N}
$$

Nevertheless, it turns out that the desired ensemble is obtained along the scaled coordinate

If we want to derive it without scaled coordinates, then we need to use the equations of Hamiltonian mechanics. For straight coordinates:

$$
\begin{cases}
\frac{dr_i}{dt} = \frac{p_i}{m_is^2} \\
\frac{ds}{dt} = \frac{p_s}{Q}
\end{cases} \quad ; \quad
\begin{cases}
\frac{\partial p_i}{\partial t} = -\frac{\partial U(r)}{\partial r_i} \\
\frac{\partial p_s}{\partial t} = \frac1{s} \left( \sum \frac{p_i^2}{m_is^2} - LkT \right)
\end{cases}
$$

It can be seen that
$$
\frac1{s} \left( \sum \frac{p_i^2}{m_is^2} - LkT \right) \bigg|_{L=3N} \sim 2 \frac{\sum p_i'^2}{2m} \rightarrow 3NkT
$$

Now, for the scaled coordinates:
$$
\begin{cases}
\frac{dr'_i}{dt} = \frac{p'_i}{m_is^2} \\
\frac1{s}\frac{ds'}{dt'} =s' \frac{p'_s}{Q}
\end{cases} \quad ; \quad
\begin{cases}
\frac{\partial p_i}{\partial t'} = -\frac{\partial U}{\partial r_i} - \frac{sp'_s}{Q}p'_i \\
\frac{s}{Q}\frac{\partial p_s}{\partial t} = \frac1{Q} \left( \sum \frac{p'_i}{m_i} - 3NkT \right)
\end{cases}
$$

This transformation of the Hamiltonian equations preserves the considered value

$$
H'_{Nose} = \sum \frac{p_i^2}{m_is^2} + U(r')+\frac{s' p_s'^2}{2Q}+3NkT\ln s
$$

Introducing $\xi=\frac{s'p'_s}{Q}$, we can obtain:
$$
\begin{cases}
\dot r_i' = \frac{p_i'}{m_i} \\
\dot p_i' = - \frac{\partial U}{\partial r_i'} - \xi p_i \\
\dot \xi = \left(\sum\frac{p_i}{m_i}- 3NkT\right)\frac1{Q} \sim T_k-T_0
\end{cases}
$$

Note that $s$ is no longer taken into account here. In the scaled coordinates we get the average of the desired ensemble.
