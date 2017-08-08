---
layout: lesson
root: .
---

This tutorial aims to provide an introduction to C++ by developing a real application that models heat transfer during welding.
We will develop a simple heat transfer model based on equations of heat conduction in solids. The spatial variation of the heating
or cooling rate in a Cartesian coordinate system can be related to the second derivative of the temperature gradients in the
x-, y-, and z-directions[^1]:

<img src="{{ site.github.url }}/fig/spatial_variation.png" height="25%" width="25%"/>

where $$ T $$ is temperature, $$ t $$ is time, $$ \lambda $$ is thermal conductivity, $$ rho $$ is density, $$ c $$ is specific heat capacity, and 
$$ a $$ is the thermal diffusivity of the material being welded.

One solution of the generic equation for steady-state distribution of temperature in a plate during arc welding was given
by Rosenthal[^2] as:

<img src="{{ site.github.url }}/fig/temperature_distribution.png" height="35%" width="35%"/>

where $$ T{x,R} $$ is the temperature as a function of the radial distance ($$ R $$) and distance ($$ x $$) along the welding centerline,
$$ T_0 $$ is the preheat temperature, $$ V $$ is the arc voltage, $$ I $$ is the welding current, $$ v $$ is the welding speed, 
$$ a $$ is the thermal diffusivity, and $$ \eta $$ is the arc efficiency.

See the Introduction to Integrated Weld Modeling[^3] for more details on the model we will be using.

We will begin by looking at the basic features of C++, including variables, constants, operators, flow of control, and functions. 
Once we have enough background information, we will begin creating the heat transfer model based on the equation above. The goal
will be to create a program that, give initial starting conditions, will output a file that describes the temperature isotherms
during the welding process. We will then couple that with the the Python plotting program we developed in the Python module to
generate a contour lot of the temperature gradients.

[^1]: O. Grong, Metallurgical Modeling of Welding, 2nd ed., The Institute of Materials, United Kingdom, 1997
[^2]: D. Rosenthal, Mathematical Theory of Heat Distribution during Welding and Cutting, Weld. J., Vol 20, 1941, p 220s–234s
[^3]: S. Babu, Introduction to Integrated Weld Modeling, ASM Handbook, Metals Process Simulation, Volume 22B, D.U. Furrer and S.L. Semiatin, editors