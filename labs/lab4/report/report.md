---
## Front matter
title: "Отчёт по лабораторной работе №4
Математическое моделирование"
subtitle: "Задача о погоне. Вариант №30"
author: "Выполнила: Данзанова Саяна Зоригтоевна, НПИбд-01-21"

## Generic otions
lang: ru-RU
toc-title: "Содержание"

## Bibliography
bibliography: bib/cite.bib
csl: pandoc/csl/gost-r-7-0-5-2008-numeric.csl

## Pdf output format
toc: true # Table of contents
toc-depth: 2
lof: true # List of figures
lot: true # List of tables
fontsize: 12pt
linestretch: 1.5
papersize: a4
documentclass: scrreprt
## I18n polyglossia
polyglossia-lang:
  name: russian
  options:
	- spelling=modern
	- babelshorthands=true
polyglossia-otherlangs:
  name: english
## I18n babel
babel-lang: russian
babel-otherlangs: english
## Fonts
mainfont: PT Serif
romanfont: PT Serif
sansfont: PT Sans
monofont: PT Mono
mainfontoptions: Ligatures=TeX
romanfontoptions: Ligatures=TeX
sansfontoptions: Ligatures=TeX,Scale=MatchLowercase
monofontoptions: Scale=MatchLowercase,Scale=0.9
## Biblatex
biblatex: true
biblio-style: "gost-numeric"
biblatexoptions:
  - parentracker=true
  - backend=biber
  - hyperref=auto
  - language=auto
  - autolang=other*
  - citestyle=gost-numeric
## Pandoc-crossref LaTeX customization
figureTitle: "Рис."
tableTitle: "Таблица"
listingTitle: "Листинг"
lofTitle: "Список иллюстраций"
lotTitle: "Список таблиц"
lolTitle: "Листинги"
## Misc options
indent: true
header-includes:
  - \usepackage{indentfirst}
  - \usepackage{float} # keep figures where there are in the text
  - \floatplacement{figure}{H} # keep figures where there are in the text
---

# Цель работы

Изучить понятие гармонического осциллятора, построить фазовый портрет и найти решение уравнения гармонического осциллятора.

# Задачи

1. Разобраться в понятии гармонического осциллятора

2. Ознакомиться с уравнением свободных колебаний гармонического осциллятора

3. Построить фазовый портрет гармонического осциллятора и решение уравнения на языках Julia и Open Modelica гармонического осциллятора для следующих случаев:

- Колебания гармонического осциллятора без затуханий и без действий внешней силы

- Колебания гармонического осциллятора c затуханием и без действий внешней силы

- Колебания гармонического осциллятора c затуханием и под действием внешней силы

# Задание

Вариант 30:

Постройте фазовый портрет гармонического осциллятора и решение уравнения гармонического осциллятора для следующих случаев:

1. Колебания гармонического осциллятора без затуханий и без действий внешней силы $\ddot{x}+4.3x=0$;
2. Колебания гармонического осциллятора c затуханием и без действий внешней силы $\ddot{x}+\dot{x}+20x=0$
3. Колебания гармонического осциллятора c затуханием и под действием внешней силы $\ddot{x}+\dot{x}+8.8x=0.7sin(3t)$

На интервале $t\in [0;61]$ (шаг $0.05$) с начальными условиями $x_0=-0.3, y_0=1.3$.

# Выполнение лабораторной работы

## Построение математической модели. Решение с помощью программ

### Julia

Код программы для первого случая:

```
# x'' + 4.3x = 0
using DifferentialEquations

function lorenz!(du, u, p, t)
    a = p
    du[1] = u[2]
    du[2] = -a*u[1]
end

const x = -0.3
const y = 1.3
u0 = [x, y]

p = (4.3)
tspan = (0.0, 61.0)
prob = ODEProblem(lorenz!, u0, tspan, p)
sol = solve(prob, dtmax = 0.05)

using Plots; gr()

#решение системы уравнений
plot(sol)
savefig("lab4_julia_1.png")

#фазовый портрет
plot(sol, vars=(2,1))
savefig("lab4_julia_1_phase.png")
```
Код программы для второго случая:

```
# x'' + x' + 20x = 0
using DifferentialEquations

function lorenz!(du, u, p, t)
    a, b = p
    du[1] = u[2]
    du[2] = -a*du[1] - b*u[1] 
end

const x = -0.3
const y = 1.3
u0 = [x, y]

p = (sqrt(1), 20)
tspan = (0.0, 61.0)
prob = ODEProblem(lorenz!, u0, tspan, p)
sol = solve(prob, dtmax = 0.05)

using Plots; gr()

#решение системы уравнений
plot(sol)
savefig("lab4_julia_2.png")

#фазовый портрет
plot(sol, vars=(2,1))
savefig("lab4_julia_2_ph.png")
```

Код программы для третьего случая:

```
# x'' + 5x' + x = 0.7sin(3t)
using DifferentialEquations

function lorenz!(du, u, p, t)
    a, b = p
    du[1] = u[2]
    du[2] = -a*du[1] - b*u[1] + 0.7*sin(3*t)
end

const x = -0.3
const y = 1.3
u0 = [x, y]

p = (sqrt(1), 8.8)
tspan = (0.0, 61.0)
prob = ODEProblem(lorenz!, u0, tspan, p)
sol = solve(prob, dtmax = 0.05)

using Plots; gr()

#решение системы уравнений
plot(sol)
savefig("lab4_julia_3.png")

#фазовый портрет
plot(sol, vars=(2,1))
savefig("lab4_julia_3_phase.png")
```

### Результаты работы кода на Julia

Первый случай: 

Колебания гармонического осциллятора без затуханий и без действий внешней силы

!["Решение уравнения для колебания гармонического осциллятора без затуханий и без действий внешней силы на языке Julia"](image/1.png){#fig:001}

!["Фазовый потрет для колебания гармонического осциллятора без затуханий и без действий внешней силы на языке Julia"](image/2.png){#fig:002}

Второй случай:

Колебания гармонического осциллятора c затуханием и без действий внешней силы

!["Решение уравнения для колебания гармонического осциллятора c затуханием и без действий внешней силы на языке Julia"](image/3.png){#fig:003}

!["Фазовый потрет для колебания гармонического осциллятора c затуханием и без действий внешней силы на языке Julia"](image/4.png){#fig:004}

Третий случай:

Колебания гармонического осциллятора c затуханием и под действием внешней силы

!["Решение уравнения для колебания гармонического осциллятора cc затуханием и под действием внешней силы на языке Julia"](image/5.png){#fig:005}

!["Фазовый потрет для колебания гармонического осциллятора c затуханием и под действием внешней силы на языке Julia"](image/6.png){#fig:006}

## OpenModelica

Код программы для первого случая:

```
//case1: x''+ 4.3x = 0
model lab4_1 
//x'' + g* x' + w^2* x = f(t) 
//w - частота 
//g - затухание 
parameter Real w = sqrt(4.30);  
parameter Real g =0;  

parameter Real x0 = -0.3; 
parameter Real y0 = 1.3; 

Real x(start=x0); 
Real y(start=y0); 

// f(t) 
function f 
input Real t ; 
output Real res; 
algorithm  
res := 0; 
end f; 

equation 
der(x) = y; 
der(y) = -w*w*x - g*y + f(time); 
end lab4_1;
```

Код программы для второго случая:

```
//case2: x'' + x' + 20x = 0
model lab4_2

parameter Real w = sqrt(20.00);  
parameter Real g = 1;  

parameter Real x0 = -0.3; 
parameter Real y0 = 1.3; 

Real x(start=x0); 
Real y(start=y0); 

// f(t) 
function f 
input Real t ; 
output Real res; 
algorithm  
res := 0; 
end f; 

equation 
der(x) = y; 
der(y) = -w*w*x - g*y + f(time); 

end lab4_2;
```

Код программы для третьего случая:

```
//case3: x'' + x' + 8.8x = 0.7sin(3t)
model lab4_3

parameter Real w = sqrt(8.80);  
parameter Real g = 1;  

parameter Real x0 = -0.3; 
parameter Real y0 = 1.3; 

Real x(start=x0); 
Real y(start=y0); 

// f(t) 
function f 
input Real t ; 
output Real res; 
algorithm  
res := 0.7*sin(3*t); // 3 случай 
end f; 

equation 
der(x) = y; 
der(y) = -w*w*x - g*y - f(time); 
end lab4_3
```

### Результаты работы кода на OpenModelica

Первый случай: 

Колебания гармонического осциллятора без затуханий и без действий внешней силы

!["Решение уравнения для колебания гармонического осциллятора без затуханий и без действий внешней силы на языке Open Modelica"](image/7.png){#fig:007}

!["Фазовый потрет для колебания гармонического осциллятора без затуханий и без действий внешней силы на языке Open Modelica"](image/8.png){#fig:008}

Второй случай:

Колебания гармонического осциллятора c затуханием и без действий внешней силы

!["Решение уравнения для колебания гармонического осциллятора c затуханием и без действий внешней силы на языке Open Modelica"](image/9.png){#fig:009}

!["Фазовый потрет для колебания гармонического осциллятора c затуханием и без действий внешней силы на языке Open Modelica"](image/10.png){#fig:010}

Третий случай:

Колебания гармонического осциллятора c затуханием и под действием внешней силы

!["Решение уравнения для колебания гармонического осциллятора cc затуханием и под действием внешней силы на языке Open Modelica"](image/11.png){#fig:011}

!["Фазовый потрет для колебания гармонического осциллятора c затуханием и под действием внешней силы на языке Open Modelica"](image/12.png){#fig:012}

# Анализ полученных результатов. Сравнение языков.

В итоге проделанной работы мы построили по три модели (включающих в себя два графика) на языках Julia и OpenModelica. Построение моделей колебания на языке OpenModelica занимает меньше строк, чем аналогичное построение на Julia.

# Вывод

В ходе выполнения лабораторной работы были построены решения уравнения гармонического осциллятора и фазовые портреты гармонических колебаний без затухания, с затуханием и при действии внешней силы на языках Julia и Open Modelica.

# Список литературы. Библиография

[1] Документация по Julia: https://docs.julialang.org/en/v1/

[2] Документация по OpenModelica: https://openmodelica.org/

[3] Решение дифференциальных уравнений: https://www.wolframalpha.com/

[4] Бутиков И. Е. Собственные колебания линейного осциллятора. 2011.