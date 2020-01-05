# IDEDelay

[![Stable](https://img.shields.io/badge/docs-stable-blue.svg)](https://YukNakata.github.io/IDEDelay.jl/stable)
[![Dev](https://img.shields.io/badge/docs-dev-blue.svg)](https://YukNakata.github.io/IDEDelay.jl/dev)
[![Build Status](https://travis-ci.com/YukNakata/IDEDelay.jl.svg?branch=master)](https://travis-ci.com/YukNakata/IDEDelay.jl)

**IDEDelay** is a package to numerically compute solutions of Delay Differential Equations, in particular, including *Distributed Delay*.

# Requirement

* The package is created using Julia 1.3
* Julia (VERSION ≥ v"1.0" probably)

# Installation

Entering Pkg mode ()
```bash
add https://github.com/YukNakata/IDEDelayVec.jl
```

# Example

## Distributed DDE (scalar)


```math
$$
y'(t)=-r \int_{t-1}^t \sin(y(s))ds
$$
```


```bash
using IDEDelayVec
using Plots

tspan = [0 20];
idefun(t,y,int) = -2.5*int;
K(t,s,y)        = sin(y); 
delays(t)       = t-1;
history(t)      = 1.5;
stepsize              = 1e-3;

sol = ide_delay_rk(idefun,K,delays,history,tspan,stepsize);

# Output vector of times t
print(sol[1])
# Output vector of solutions y
print(sol[2])

# Output 
sol
# create a line plot
plot(sol, linewidth=2, xlabel="t", ylabel="y", title="IDE Delay Runge-Kutta")

```

## Distributed DDE system


```math
$$
y_1'(t)=-r_{11} \int_{t-1}^t \sin(y_1(s))ds-r_{12} \int_{t-1}^t \sin(y_2(s))ds\\
y_2'(t)=-r_{11} \int_{t-1}^t \sin(y_1(s))ds-r_{12} \int_{t-1}^t \sin(y_2(s))ds\\
$$
```


```bash
using IDEDelayVec
using Plots

tspan = [0 50];
idefun(t,y,int) = [ int[1];
                    int[2]]
K(t,s,y)        = [ -6*sin(y[1])-9*sin(y[2]);
                    -9*sin(y[1])-7*sin(y[2])];
delays(t)       = t-1;
history(t)      = [5.5*sin(t);
                    -5.0*sin(t)];
stepsize		= 1e-3;

sol = ide_delay_rk(idefun,K,delays,history,tspan,stepsize);

# Output vector of times t
# Output vector of solutions y
#print(sol[1])
#print(sol[2])

# create a line plot
plot(sol, linewidth=2, xlabel="t", ylabel="y", title="IDE Delay Runge-Kutta")
```
# Note

* Extension to a system of RE and DDE is planned.


# Author

* Alexander Lobaskin (Saint Petersburg State University)
* Alexey Eremin (Saint Petersburg State University)
* Yukihiko Nakata (Shimane University)

# License

"IDEDelay" is under [MIT license](https://en.wikipedia.org/wiki/MIT_License).
