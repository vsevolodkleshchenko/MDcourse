<!-- #region -->
## OUTLINE of the cource

### Abstract
Dynamics and collective behaviour of the system of interacting particles, even on molecular scale, in the majority of cases may be fairly described with Classical Mechanics. It would seem that we just need to numerically integrate the system of differential equations of motions, and that's it! But we know that dynamics of systems in Classical Mechanics have remarkable properties such as the laws of conservation of energy and phase volume etc.
Will any numerical scheme for ordinary differential equations preserve such fundamental things? To which extent the obtained particle trajectories will be accurate? How to get experimental observable quantities, and to which statistical ensembles they are related? All these questions take us back to the basics of the Molecular Dynamics method, and it appears that its foundation is mainly connected to Hamiltonian Mechanics and Statistical Thermodynamics rather than to the field of Numerical Methods for ordinary differential equations. In this course we will go through these foundations, to give students a feeling what is under the hood of the modern super-efficient molecular dynamics codes together with the understanding of the inherent limitation of the methods. At the end of the course will also consider the famous Monte-Carlo method that has no dynamics inside, which is however connected to Molecular dynamics and remarkably these methods were developed nearly simultaneously.

### Outline
0. Some historical introduction, I will think about it. (Lect 0, or big annotaion or begginig of lect 1)

1. Problem statement, what quantities we expect to obtain. Scheme of the basic Molecular dynamics algorithm.  Algorithms from numerical point of view, order of algorithms, accuracy. (Lect 1) 

*suggestion: add general chart of MD (init->update->forces->update...), add brick-schematic of Verlet algs*

2. Properties of equations, Lyapunov instabilities, properties of Hamiltonian mechanics, Liouville formulation. Obtaining numerical scheme in Liouville formalism, demostration that it is a Verlet algorithm. (lect 2) 

*maybe calculate our own divergence with simple system of say 10 LJ atoms. If you have time, the same for energy consevation, making short program in C is a good idea*

3. Other parts of the general MD simulation chart: periodic boundary conditions, empirical potentials (force fields), neighbour lists for short range interactions, *handling long and interactions (lattice methods, Ewald summation)*. (lect 3)

*suggestion: I will add the last part, look at finnish lections*

4. Changing conditions: constant temperature and constant pressure. Different thermostatting algorithms, Liouville formulation, what is a real coordinate?

*comment: here is a little bit hard but instructive math*

5. Monte-Carlo methods for integrals and in Statistical thermodynamics. Markov chains and detailed balase, how to calculate observables. Extensions for different ensembles, what is real in this case?


6. Practical notes. Packages available, state of the art codes, what can be calculated and what is difficult. student projects.

*comment: I will do it someday*


<!-- #endregion -->
