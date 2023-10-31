# MDcourse

Dynamics and collective behaviour of the system of interacting particles, even on molecular scale, in the majority of cases may be fairly described with Classical Mechanics. It would seem that we just need to numerically integrate the system of differential equations of motions, and that's it! But we know that dynamics of systems in Classical Mechanics have remarkable properties such as the laws of conservation of energy and phase volume etc.
Will any numerical scheme for ordinary differential equations preserve such fundamental things? To which extent the obtained particle trajectories will be accurate? How to get experimental observable quantities, and to which statistical ensembles they are related? All these questions take us back to the basics of the Molecular Dynamics method, and it appears that its foundation is mainly connected to Hamiltonian Mechanics and Statistical Thermodynamics rather than to the field of Numerical Methods for ordinary differential equations. In this course we will go through these foundations, to give students a feeling what is under the hood of the modern super-efficient molecular dynamics codes together with the understanding of the inherent limitation of the methods. At the end of the course will also consider the famous Monte-Carlo method that has no dynamics inside, which is however connected to Molecular dynamics and remarkably these methods were developed nearly simultaneously.

To display the formulas correctly, open the website on [GitHub pages](https://vsevolodkleshchenko.github.io/MDcourse/).

## Lectures

* **Lecture 0**. Some historical introduction.
* **Lecture 1**. [Equations of motion.](lectures/lecture1/lecture1.md)
* **Lecture 2**. [Numerical scheme in Liouville formalism.](lectures/lecture2/lecture2.md)
* **Lecture 3**. [Potential energy, forces, boundary conditions.](lectures/lecture3/lecture3.md)
* **Lecture 4**. [Controlling temperature.](lectures/lecture4/lecture4.md)
* **Lecture 5**. Monte Carlo method.
* **Lecture 6**. Practice notes.

## Practice
1. Symplectic integrators (see Lecture 2) [[Colab](https://colab.research.google.com/github/vsevolodkleshchenko/MDcourse/blob/main/practice/practice2.ipynb), [nbviewer](https://nbviewer.org/github/vsevolodkleshchenko/MDcourse/blob/main/practice/practice2.ipynb)]
1. Simulations in HOOMD-blue. Velocity distribution. [[Colab](https://colab.research.google.com/github/vsevolodkleshchenko/MDcourse/blob/main/practice/MD_tutorial_and_velocity_distributions.ipynb), [nbviewer](https://nbviewer.org/github/vsevolodkleshchenko/MDcourse/blob/main/practice/MD_tutorial_and_velocity_distributions.ipynb)]

## Literature
1. D. Frenkel, B. Smit, Understanding Molecular Simulation, 2nd Edition, Academic Press, New York, 2001.
2. Tuckerman, Mark. Statistical mechanics: theory and molecular simulation. Oxford university press, 2010.
3. Leimkuhler, Benedict, and Sebastian Reich. Simulating hamiltonian dynamics. No. 14. Cambridge university press, 2004.
4. M. Allen, D. Tildesley, Computer Simulation of Liquids, Clarendon Press, Oxford, 1987.
5. В. Замалин, Г. Норман, В. Филинов, Метод Монте-Карло в статистической термодинамике, Москва, Наука, 1977.
6. T. Schlick, Molecular Modeling and Simulation: An Interdisciplinary Guide, Springer 2nd ed. 2010
7. Berendsen, Herman. Simulating the physical world. Cambridge: Cambridge University Press, 2007.
