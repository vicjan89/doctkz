Начальный ток короткого замыкания
==================================

Расчёт сети имеет вид: 
    
Ток КЗ рассчитывается в два этапа:
    - рассчитать составляющую тока КЗ :math:`I''_{kI}` от всех источников напряжения
    - рассчитать составляющую тока КЗ :math:`I''_{kII}` от всех источников тока
    
Эти два тока затем составляют полный начальный ток КЗ  :math:`I''_{k} = I''_{kI} + I''_{kII}`.

.. _c:

Эквивалентные источники напряжения
----------------------------------

Для расчёта составляющей тока КЗ от источников напряжения, все эти источники заменяются на один эквивалентный источник напряжения :math:`V_Q` подключенный в месте повреждения.
Величина напряжения на повреждённой шине принимается равной:

.. math::

    V_Q =
    \left\{
    \begin{array}{@{}ll@{}}
      \frac{c \cdot \underline{V}_{N}}{\sqrt{3}} & \text{for three phase short circuit currents} \\
      \frac{c \cdot \underline{V}_{N}}{2} & \text{for two phase short circuit currents}
    \end{array}\right.
     
где :math:`V_N` - номинальное напряжение повреждённой шины и c это коэффициент коррекции напряжения, - коэффициент коррекции напряжения, который учитывает рабочие отклонения от номинального напряжения в сети..

Коэффициенты коррекции напряжения :math:`c_{min}` для минимального и :math:`c_{max}` для максимального токов короткого замыкания определяются для каждой шины в зависимости от уровня напряжения.
В низковольтных сетях существует дополнительное различие между сетями с допустимым снижением напряжения 6% по сравнению с 10% для :math:`c_{max}`:


.. |cmin| replace:: :math:`c_{min}`
.. |cmax| replace:: :math:`c_{max}`

+--------------+---------------+--------+--------+
|Уровень напряжения            | |cmin| | |cmax| |
+==============+===============+========+========+
|              | Допуск 6%     |        |  1.05  |
|< 1 кВ        +---------------+  0.95  +--------+
|              | Допуск 10%    |        |        |
+--------------+---------------+--------+  1.10  +
|> 1 кВ                        |  1.00  |        |
+--------------+---------------+--------+--------+


Составляющая источника напряжения 
---------------------------------

Чтобы рассчитать составляющую всех источников напряжения сделаны следующие допущения:

1. Токи нагрузки на всех шинах не учитываются
2. Игнорируются все источники тока
3. Напряжние на повреждённой шине равно :math:`V_Q`
   
Для расчёта тока КЗ на шине :math:`j`необходимо решить следующее матричное уравнение:

.. math::
   
   \begin{bmatrix}
    \underline{Y}_{11} & \dots & \dots & \underline{Y}_{n1} \\[0.3em]
    \vdots & \ddots & & \vdots \\[0.3em]
    \vdots & & \ddots & \vdots \\[0.3em]
    \underline{Y}_{1n} & \dots & \dots & \underline{Y}_{nn}
    \end{bmatrix}
    \begin{bmatrix}
    \underline{V}_{1}  \\
    \vdots  \\
    V_{Qj}  \\
    \vdots  \\
    \underline{V}_{n}
    \end{bmatrix}  
    = 
    \begin{bmatrix}
    0 \\
    \vdots  \\
    \underline{I}''_{kIj} \\
    \vdots  \\
    0 
    \end{bmatrix}

где :math:`\underline{I}''_{kIj}` - составляющая источника напряжения в ток короткого замыкания на шине :math:`j`.
Напряжения на неповреждённых шинах и ток в повреждённой шине неизвестны. Чтобы найти :math:`\underline{I}''_{kIj}`
мы умножаем его на инвертированную матрицу проводимостей (матрицу сопротивлений):
    
.. math::
   
    \begin{bmatrix}
    \underline{V}_{1}  \\
    \vdots  \\[0.4em]
    V_{Qj}  \\[0.4em]
    \vdots  \\
    \underline{V}_{n}
    \end{bmatrix}  
    = 
    \begin{bmatrix}
    \underline{Z}_{11} & \dots & \dots & \dots & \underline{Z}_{n1} \\
    \vdots & \ddots &  & & \vdots \\
    \vdots & & \underline{Z}_{jj} & & \vdots \\
    \vdots & & & \ddots & \vdots \\
    \underline{Z}_{1n} & \dots & \dots & \dots & \underline{Z}_{nn}
    \end{bmatrix}
    \begin{bmatrix}
    0 \\
    \vdots  \\[0.25em]
    \underline{I}''_{kIj} \\[0.25em]
    \vdots  \\
    0 
    \end{bmatrix}

Ток короткого замыкания для шины j определяется как:

.. math::
   
   I''_{kIj} = \frac{V_{Qj}}{Z_{jj}}

Для расчета вектора токов короткого замыкания на всех шинах уравнение можно расширить следующим образом:

.. math::
   
    \begin{bmatrix}
    \underline{V}_{Q1} & \dots & \underline{V}_{n1} \\[0.4em]
    \vdots & \ddots & \vdots \\[0.4em]
    \underline{V}_{1n} & \dots & \underline{V}_{Qn}
    \end{bmatrix}  
    = 
    \begin{bmatrix}
    \underline{Z}_{11} & \dots & \underline{Z}_{n1} \\[0.8em]
    \vdots & \ddots & \vdots \\[0.8em]
    \underline{Z}_{1n} & \dots & \underline{Z}_{nn}
    \end{bmatrix}
    \begin{bmatrix}
    \underline{I}''_{kI1} & \dots & 0 \\[0.8em]
    \vdots & \ddots & \vdots \\[0.8em]
    0 & \dots & \underline{I}''_{kIn}
    \end{bmatrix}

из которого следует:
    
.. math::
   
    \begin{bmatrix}
    I''_{kI1} \\[0.25em]
    \vdots  \\[0.25em]
    I''_{kIn} \\
    \end{bmatrix}
    = 
    \begin{bmatrix}
    \frac{V_{Q1}}{Z_{11}}  \\
    \vdots  \\
    \frac{V_{Qn}}{Z_{nn}} 
    \end{bmatrix}

Таким образом все токи короткого замыкания могут быть рассчитаны одновременно с помощью одной инверсии матрицы проводимостей.

Если указано сопротивление в месте повреждения то оно добавляется к диагонали матрицы сопротивления. Затем токи короткого замыкания на всех шинах рассчитываются как:

.. math::
     
    \begin{bmatrix}
    I''_{kI1} \\[0.25em]
    \vdots  \\[0.25em]
    I''_{kIn} \\
    \end{bmatrix}
    = 
    \begin{bmatrix}
    \frac{V_{Q1}}{Z_{11} + Z_{fault}}  \\
    \vdots  \\
    \frac{V_{Qn}}{Z_{nn} + Z_{fault}} 
    \end{bmatrix}
    

Составляющая источников тока
-----------------------------

Для расчёта составляющей источников тока в полном токе КЗ все источники напряжения все источники напряжения замкнуты накоротко и учитываются только источники тока. Тогда токи шин выражаются как:

.. math::
    \begin{bmatrix}
    I_1 \\[0.2em]
    \vdots  \\[0.2em]
    I_m \\[0.2em]
    \vdots  \\
    I_n
    \end{bmatrix}
    =
    \begin{bmatrix}
    0 \\[0.2em]
    \vdots  \\[0.2em]
    \underline{I}''_{kIIj} \\[0.2em]
    \vdots  \\
    0
    \end{bmatrix}
    -
    \begin{bmatrix}
    I''_{kC1} \\[0.2em]
    \vdots  \\[0.2em]
    \underline{I}''_{kCj} \\[0.2em]
    \vdots  \\
    I''_{kCn}
    \end{bmatrix}
    =
    \begin{bmatrix}
    -I''_{kC1} \\[0.2em]
    \vdots  \\[0.2em]
    \underline{I}''_{kIIj} - \underline{I}''_{kCj} \\[0.2em]
    \vdots  \\
    -I''_{kCn}
    \end{bmatrix}

где :math:`I''_{kC}` - токи КЗ, которые подаются преобразовательным элементом на каждую шину, :math:`\underline{I}''_{kIIj}` - вклад преобразовательных элементов на повреждённую шину :math:`j`.
Если известно что напряжение на повреждённой шине равно нулю, уравнения сети имеет следующий вид:

.. math::

    \begin{bmatrix}
    \underline{V}_{1}  \\
    \vdots  \\[0.4em]
    0  \\[0.4em]
    \vdots  \\
    \underline{V}_{n}
    \end{bmatrix}  
    = 
    \begin{bmatrix}
    \underline{Z}_{11} & \dots & \dots & \dots & \underline{Z}_{n1} \\
    \vdots & \ddots &  & & \vdots \\
    \vdots & & {Z}_{jj} & & \vdots \\
    \vdots & & & \ddots & \vdots \\
    \underline{Z}_{1n} & \dots & \dots & \dots & \underline{Z}_{nn}
    \end{bmatrix}
    \begin{bmatrix}
    -I''_{kC1} \\[0.2em]
    \vdots  \\[0.2em]
    \underline{I}''_{kIIj} - \underline{I}''_{kCj} \\[0.2em]
    \vdots  \\
    -I''_{kCn}
    \end{bmatrix}

Из строки :math:`j` уравнения следует:

.. math::

    0 = \underline{Z}_{jj} \cdot \underline{I}''_{kIIj} - \sum_{m=1}^{n}{\underline{Z}_{jm} \cdot \underline{I}_{kCj}}

которое можно преобразовать в:
    
.. math::
 
    \underline{I}''_{kIIj} = \frac{1}{\underline{Z}_{jj}} \cdot \sum_{m=1}^{n}{\underline{Z}_{jm} \cdot \underline{I}_{kC, m}}

Чтобы рассчитать все токи при КЗ на каждой шине одновременно можно обобщить в следующее матричное уравнение:

.. math::

    \begin{bmatrix}
    \underline{I}''_{kII1} \\[0.5em]
    \vdots  \\[0.5em]
    \vdots  \\[0.5em]
    \underline{I}''_{kIIn}
    \end{bmatrix} = 
    \begin{bmatrix}
    \underline{Z}_{11} & \dots & \dots & \underline{Z}_{n1} \\[0.3em]
    \vdots & \ddots & & \vdots \\[0.3em]
    \vdots & & \ddots & \vdots \\[0.3em]
    \underline{Z}_{1n} & \dots & \dots & \underline{Z}_{nn}
    \end{bmatrix}
    \begin{bmatrix}
    \frac{I''_{kC1}}{\underline{Z}_{11}} \\[0.25em]
    \vdots  \\
    \vdots  \\[0.25em]
    \frac{I''_{kCn}}{\underline{Z}_{nn}}
    \end{bmatrix}
