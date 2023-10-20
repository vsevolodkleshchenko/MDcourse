# Periodic boundary conditions

The simulated system must be limited in volume, otherwise all particles will eventually fly from each other to infinity.
One of the ways to limit the system is to introduce periodic boundary conditions.
In this case the system turns out to be sort of infinite.

The usage of periodic boundary conditions is possible if only the interaction between molecules is short-range and strongly localized.
For example, Lennard-Jones potential satisfies this condition

$$
U(r) = 4\epsilon\left[\left(\frac{\sigma}{r}\right)^{12} - \left(\frac{\sigma}{r}\right)^6\right]
$$

![Lennard-Jones potential](images/LJ_potential.png)

## Minimum-image convention

When using periodic boundary conditions, it is necessary to consider interactions with molecules not only within a single cell, but also among all copies of the cell.
However, in the case of short-range interactions one can consider the interaction only with those images of molecules which are at a distance smaller than the characteristic distance $r_c$.
This approximation is called minimum-image convention.
Maintenance of the minimum-image convention requires that the characteristic distance is smaller than the half characteristic size $L$ of the cell.

![Scheme](images/scheme.png)

The simulated cell does not have to be cubic, any unit cell can be used. 
Other unit cells can be used to reduce the number of particles that are located somewhere in the corners of the simulated cell and are not involved in the process of interest.

![Элементарные ячейки](images/fig4_draft.png)

Межмолекулярные взаимодействия необходимо обрезать, чтобы ускорить расчет.
Рассмотрим варианты обрезания.
1. Просто обрезать на расстоянии $r_c$.
Потенциал при $r>r_c$ считается нулем.
В таком случае возникает проблема с силами, которые являются производной потенциала: силы будут испытывать скачок, что не соответствует физике. 
1. Чтобы избежать этого, можно приподнять потенциал так, чтобы убрать ступеньку в потенциальной энергии.
Однако это приведет к тому, что поменяется энергетический баланс в системе и моделируемый потенциал будет сильно отличатся от реального. 
1. Добавим дифференцируемую переходную функцию на некотором интервале $[r_c, r_i]$.
Получается потенциал, не обладающий недостатками, указанными выше.

![Потенциал](images/fig6a.png)

# Other boundary conditions

1. **Open boundary conditions**.
In this case, if a molecule reaches a boundary, it will be removed from simulation. 
Могут быть использованы для моделирования капли какого-нибудь вещества в свободном пространстве.
Со временем число молекул в такой капле будет уменьшатся, что может негативно сказываться на результате расчета.
1. **Вакуум**.
Эти граничные условия накладывают ограничения на интерпретацию результатов. 
Используются для моделирования единичных молекул, однако в эксперименте такие молекулы будут находится в каком-нибудь растворителе, с молекулами которого они взаимодействует.
Это приводит к тому, что, например, спектр молекул в эксперименте будет отличатся от найденного численно.
1. **Граничные условия твердой стенки**. 
Условия, при которых молекулы упруго отталкиваются от границы.
При этом возникают проблемы с эффектами, связанными с размерами системы.
В случае использования жестких граничных условий получается, что в системе существует эффективное поверхностное натяжение, а сама рассматриваемая подсистема становится нанокаплей. 
В случае использования периодических граничных условий этого не происходит.

# Потенциалы

Чтобы считать силы, необходимо задать потенциальную энергию.
В молекулярной динамике потенциальная энергия получается полуэмпирически с использованием экспериментальных данных и квантовомеханических расчетов.

Потенциальная энергия является суммой потенциальных энергий растяжения, изгиба, кручения межатомных связей в молекуле, энергий межмолекулярного взаимодействия и энергий электростатического взаимодействия 
![Потенциалы](images/fig5.png)
Levitt, M. (2001). The birth of computational structural biology. Nat. Struct. Biol., 8, 392–393. doi: 10.1038/87545



# Выводы

Чтобы запустить моделирование необходимо
1. Задать систему:
   1. выбрать объем, задать граничные условия
   2. выбрать функции взаимодействия, которые будут использоваться для расчета сил
   3. задать начальные условия
2. Выбрать алгоритм, который будет рассчитывать эволюцию системы.