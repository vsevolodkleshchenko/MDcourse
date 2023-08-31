# Lecture 3

# NVE ensemble
Рассмотрим систему в которой взаимодействие частиц определяется силами $f(r_{ij})$. В рассматриваемой системе будет постоянное число частиц $N$, объем системы $V$ и энергия $E$. Объем системы будет учитываться с помощью периодических граничных условий, и для Гамильтониана можно записать:

$$
H(p, r) \rightarrow 
\begin{cases}
\dot p = \dots \\
\dot q = \dots
\end{cases}, \quad
H(p, r) = E
$$

С точки зрения термодинамики вычисляются средние величины с учетом эргодичности:

$$
\left<a(x)\right> = \frac{\int dx a(x) \delta(H(x) - E_0)}
{\int dx \delta(H(x) - E_0)} = \frac1T \int_0^T a(x(t)) dt
\approx \frac1M\sum_{n=1}^M a(n\Delta t)
$$

Где $x= \begin{cases} r \\ p \end{cases}$, $M$ - число шагов.

В реальной природе всегда есть термодинамическая баня. Обычно предполагается что есть меньшая подсистема в некоторой большой системе, которая обменивается с ней теплом, в результате чего есть некоторая температура $T$ и на каждую степень свободы приходится энергия $\frac{kT}2$.

Среднее по распределению Больцмана:

$$
\left<a(x)\right> = \frac{
    \int dx a(x) \exp(-\frac{\frac{p^2}{2m} + U(r)}{kT})}{
    \int dx \exp(-\frac{\frac{p^2}{2m} + U(r)}{kT})}
\frac1T \int_0^T a(x(t))
$$

Для импульса есть распределение Максвела:

$$
f(p) = \left(\frac1{2\pi nkT}\right)^{3/2}\exp\left(-\frac{p^2}{2mkT}\right)
$$

Окуда получается момент этого распределения:

$$
\Rightarrow \frac{3kT}2 = \frac{m\left<|v|^2\right>}2 \quad \rightarrow  \quad \left<p^2\right> = 3kTm, \left<p^4\right> = 15(kTm)^2
$$

Давайте держать среднюю скорость такой, то есть делать rescaling скоростей. Тогда получится некоторая другая система с нужной кинетической энергией, в которой будет неправильно то что она не будет гарантировать вот этого распределения. 

Самое простое это посмотреть на относительные флуктуации квадрата импульса $\frac{\sigma_{p^2}}{\left<p^2\right>^2}$ и всей кинетической энергии (кинетическая температра) $\frac{\sigma_{T_k}^2}{\left<T_k^2\right>^2}$.

Тогда квадрат скорости любой молекулы флуктуирует как:

$$
\frac{\sigma_{p^2}}{\left<p^2\right>^2} = \frac{\left<p^4\right> - \left<p^2\right>^2}{\left<p^2\right>^2} = \frac{15-9}{9} = \frac23
$$

И кинетическая температура, которая определяется как $\left<\sum\frac{mv^2}2\right> = \frac{3NkT}2$.

$$
T_k \sim \frac1N \sum p_i^2, \quad 
\left<T_k\right> \sim \frac1N \sum\left<p_i\right>^2 = \left<p^2\right>\\
\rightarrow \quad \left<T_k^2\right> \sim \frac1{N^2} \left<\left(\sum p_i^2\right)\right> = \frac1{N^2}\left(N\left<p^4\right> + N(N-1)\left<p^2\right>\left<p^2\right>\right)
$$

Тогда 

$$
\frac{\sigma_{T_k}}{\left<T_k\right>^2} = \frac{\left<T_k^2\right> - \left<T_k\right>^2}{\left<p^2\right>^2} = \frac1{N^2} 
\frac{\frac1{N^2}\left(N\left<p^4\right> + N^2\left<p^2\right>^2 - N\left<p^2\right>\right) - \left<p^2\right>^2}{\left<p^2\right>^2} = \\ = \frac{\left<p^4\right> - \left<p^2\right>^2}{N\left<p^2\right>^2} = \frac2{3N}
$$

Тогда если необходимо следить за температурой, нужно заботиться о том что бы все моменты были правильными. Соответственно если мы сделали метод который среднее будет держать постоянным, то вариация у него $0$.

Таким образом пользоваться можно только методами в которых доказано что все остается правильным.

- isokinetic method: сумма квадратов постоянна: 
$$
\left<p^2\right> = 0 \quad\Rightarrow\quad \sigma_{T_k} = 0
$$

Далее существует лучшие методы, которые условно можно разделить на два класса

- Anderson method: Давайте будем периодически случайные удары о молекулы вводить в систему.В какой-то момент импульс какой то молекулы меняется, при чем так что распределение остается правильным
- Anderson : чтобы наша система сэмплировала не постоянную энергию а по распределению при этом динамика сохранялась
$$
\delta(E-H(p, r)) \quad\rightarrow\quad \exp(-\frac{H(p(r))}{kT})
$$

Ткаим образом, постановка задачи: мы хотим в нашей системе из $N$ атомов (возможно с периодическими граничными условиями) каким -то образом получать одно за другим состояния (сэмплировать), так чтобы получалсь среднее именно в понимании $NVT$:
$$
\left<a(x(t))\right> = \frac1M \sum_{n=1}^M a(n\Delta t) = 
\frac{\int dx a(x) \exp(-\frac{H(x)}{kT})}{\int dx \exp(-\frac{H(x)}{kT})} = \left<a(x)\right>_{NVT}
$$

Все последующие алгоритмы называются термостатами, поскольку они приводят к тому что у системы есть постоянная темепература.

### Первый подход

Давайте раз в какое то время с вероятностью $\nu$ $([\nu] = 1/s)$ гипотетически с частицей что-то сталкивается, так что ее скорость изменяется как $v \rightarrow v_M$, где $v_M$ - скорость из распредеелния Максвела $f(v)$. Утверждается, что этого достаточно.

С алгоритмической точки зрения:

рассмотри изменние система из нулевого момента времения на шаг $\Delta t$:

$$
\left[r^n(0), p^n(0) \right] \rightarrow \left[r^n(\Delta t), p^n(\Delta t) \right]
$$

Тогда сталквиание будет работать так

```
for i in range(1, N):
    if test():
        v[i] = Gauss()  # Maxvell distribution
```
где `test()` - достигла ли величина $\nu \Delta t$ достаточного значения для применения столкновения.

Преимущества:
- Простой
- Дает правильно распределение

Минусы:
- Потеряна динамика: корреляционная функция скоростей будет экспоненциально спадать $\left<v(0)v(t)\right> \sim \exp(-\nu t)$.

### Второй подход Nose-Hoover
Хочется получить метод который также будет ходить между плоскостями, но при этом будет сохраняться динамика.

Для этого можно расширить систему, добавив переменную $s$, скалирующую скорости.

Рассмотрим Лагранжиан:
$$
\mathcal{L}_{Nose} = \sum_{i=2}^n \frac{m_i}2 (s\dot r_i^2) - U(r_N) + \frac{Q}2 \dot s ^2 - LkT \ln s
$$

$Q$ - аналог массы, задающий инерцию для координаты, $L$ - константа, смысл которой будет понятен позже.

Теперь для перехода к Гамильтониану с $6N + 2$ степенями свободы:
$$ 
\begin{cases}
    p_i = \frac{\partial \mathcal{L}}{\partial \dot r_i} = m_i s^2 \dot r_i \\
    p_s = \frac{\partial \mathcal{L}}{\partial \dot s} = Q\dot s
\end{cases} \Rightarrow
H = \sum \frac{p_i^2}{2m_is} + U(r^N) + \frac{p_s^2}Q + L kT\ln s
$$

Посмотри, как будет происходить интегрирование и вычисление средних исходя из него. Рассмотрим статсумму

$$
Q_N = \frac1N\int dp_s ds dp^N dr^N \delta (H_N(s, p_s, p, r) - E)
$$

Вводя $p' = \frac{p}{s}$ и $H'(p, r) = \sum \frac{p_i'^2}{2m_is} + U(r^N)$, получим

$$
Q_N = \frac1N\int dp_s ds dp'^N dr^N s^{3N} \delta \left(
    H'(p, r) + \frac{p_s^2}Q + L kT\ln s \right) = \\
\frac1N\int dp_s ds dp'^N dr^N s^{3N} \delta \left[LkT\left(\ln s  - \frac{-H'(p, r) - \frac{p_s^2}Q - E}{LkT} \right) \right]
$$
Обозначим $\ln(s_0) = \frac{-H'(p, r) - \frac{p_s^2}Q - E}{LkT}$ и проинтегрируем 
$$
Q_N = \frac1N\int\dots ds s^{3N} \delta \left(LkT\ln(\frac{s}{s_0})\right) = \frac1N\int ds \frac{s_0^{3N+1}}{LkT} dp_s dp'^N dr^N = \\
= \frac1{N!LkT}\int dp_s dp'^N dr^N \exp\left(
    -\frac{3N+1}{L} \frac{H(p', r) + p_s^2/Q - E}{kT} \right) \Rightarrow \\
Q_N = \frac{c}{N!}\int dp'^N dr^N \exp\left(
    -\frac{3N+1}{L} \frac{H(p', r) + p_s^2/Q - E}{kT} \right) 
$$
Если выбрать $L=3N+1$ то и получится то что нужно.

Получается что среднее значение какой то функции по траектории
$$
A(p', r) = \lim_{T\rightarrow\infty}\int_0^T dt A\left(\frac{p(t)}{s(t)}, r(t)\right) = \left<A\left(\frac{p}{s}, r\right)\right>
$$
а по распределению получается реально как среднее по ансамблю:
$$
\left<A\left(\frac{p}{s}, r\right)\right>_{Nose} = \frac{
    \int dp'^N dr^N A\left(p', r\right) \exp\left(-\frac{H(p', r)}{kT} \right)}{
    \int dp'^N dr^N \exp\left(-\frac{H(p', r)}{kT} \right)} = 
\left<A\left(p', r\right)\right>_{NVT}
$$

Что здесь реальное, а что здесь построение? С учетом всех переменных, из формы Лагранжиана, когда мы его расширяем, можно получить что $\dot r '= s\dot r$  и время шкалируется в зависимости от $s$:

$$
dt' = \frac{dt}{s} 
$$

Рассмотрим среднее и будем интегрировать по штрихованному времени, чтобы переехать в штрихованное время и ничего не испортить, т.е жить в тех координатах которые не растянуты и при этом выполняется правильное распределение по импульсам.
$$
\lim_{T'\rightarrow\infty} \frac1{T'}\int_0^{T'} dt' A\left(\frac{p(t')}{s(t')}, r(t') \right) = 
\lim_{T'\rightarrow\infty} \frac{T}{T'} \frac1{T} \int_0^{T} dt A\left(\frac{p(t)}{s(t)}, r(t)\right) \frac1{s(t)} = \\
= \frac{
    \lim_{T'\rightarrow\infty} \frac1{T} \int_0^{T} dt A\left(\frac{p(t)}{s(t)}, r(t)\right) \frac1{s(t)}}{
    \lim_{T'\rightarrow\infty} \frac1{T}\int_0^T\frac{dt}{s(t)} 
} = \frac{\left<\frac{A}{s}\right>}{\left<\frac1{s}\right>}
$$
В итоге получается не то что мы хотели: 
$$
\bar A = \lim_{T'\rightarrow\infty} \frac1{T'}\int_0^{T'} dt' A\left(\frac{p(t')}{s(t')}, r(t') \right) = 
\frac{\left<A\left(\frac{p}{s}, r\right)\frac1{s}\right>}{\left<\frac1{s}\right>} = \\ =
\frac{\int dp_s ds dp^N dr^N s^{3N}A\frac1{s}}
{\int dp_s ds dp^N dr^N s^{3N} \frac1{s}} = |L=3N| = 
\left<A\left(p', r\right)\right>_{NVT} \bigg|_{L=3N}
$$

Тем не менее получается, что вдоль штрихованной координаты получается нужный ансамбль

Если хотим записать без штрихованных коррдинат, то нужно записать уравнения Гамильтоновой механики. Для прямых координат:
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

При этом видно, что 
$$
\frac1{s} \left( \sum \frac{p_i^2}{m_is^2} - LkT \right) \bigg|_{L=3N} \sim 2 \frac{\sum p_i'^2}{2m} \rightarrow 3NkT
$$

Теперь для штрихованных координат:
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

Это преобразование Гамильтоновых уравнений сохраняет величину
$$
H'_{Nose} = \sum \frac{p_i^2}{m_is^2} + U(r')+\frac{s' p_s'^2}{2Q}+3NkT\ln s
$$

Если ввести величину $\xi=\frac{s'p'_s}{Q}$, то получаем:
$$
\begin{cases}
\dot r_i' = \frac{p_i'}{m_i} \\
\dot p_i' = - \frac{\partial U}{\partial r_i'} - \xi p_i \\
\dot \xi = \left(\sum\frac{p_i}{m_i}- 3NkT\right)\frac1{Q} \sim T_k-T_0
\end{cases}
$$
Заметим что $s$ тут уже не учитываетсяВ

<!-- $$
\begin{cases}
r' = r \\ p' = \frac{p}{s} \\ s' = s \\ p_s' = p_s
\end{cases} \rightarrow
v' = \frac{v}{s} \rightarrow \frac{dr'}{dt'} = \frac{dr}{dt\cdot s} 
\Rightarrow
dt' = \frac{dt}{s(t)}
$$ -->
