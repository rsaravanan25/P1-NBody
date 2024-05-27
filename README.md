# Project 1: NBody

This is the directions document for Project P1 NBody in CompSci 201 at Duke University, Fall 2023.

## Outline
- [Background](#background)
- [Starter Code and Using Git](#starter-code-and-using-git)
- [Developing, Running, Testing Code](#developing-running-testing-code)
  - [`CelestialBody` Variables, Constructor, and Getter Methods](#celestialbody-variables-constructor-and-getter-methods)
  - [Additional `CelestialBody` Methods](#additional-celestialbody-methods)
  - [The `NBody` Class](#the-nbody-class)
  - [`NBody` Methods](#nbody-methods)
  - [Note on Running the Simulation](#note-on-running-the-simulation)

## Background (optional but encouraged)

This assignment heavily borrows from Princeton and Berkeley Computer Science and the work of Robert Sedgewick, Kevin Wayne and Josh Hug.

**Context:** In 1687, Isaac Newton formulated the principles governing the motion of two particles under the influence of their mutual gravitational attraction in his famous Principia. However, Newton was unable to solve the problem for three particles. Indeed, in general, solutions to systems of three or more particles must be approximated via numerical simulations.
For a more complete understanding of the Physics you can reference [this document][Physics].

In this assignment, you will write a program to simulate the motion of _N_ objects in a plane, mutually affected by gravitational forces, and animate the results. Such methods are widely used in cosmology, semiconductors, and fluid dynamics to study complex physical systems. Ultimately, you will be creating a driver program `NBody.java` that draws an animation of bodies moving in space interacting with each other subject to interacting and mutual gravitational forces. These celestial bodies (ex. planet or sun) are modeled by the class `CelestialBody.java` that you'll implement and test independently of the simulation.

Below you can expand to see an animation of a completed project running with some planets in our solar system. The animation repeats after one earth year, your program continues until the simulation completes.

### Example Simulation of Complete Project

<div align="center">
  <img width="500" height="500" src="p1-figures/planets.gif">
</div>



## Developing, Running, Testing Code

You're given the outline of a class `CelestialBody` with stub or missing methods and a missing constructor. You'll add code so that the class `CelestialBody.java` works as described below. This class represents a celestial body such as a planet or a sun.  

Finally, you will create a class `NBody.java` that drives a simulation between planets, suns, and celestial bodies interacting. This class will read a file of data that specifies the initial positions and masses of the bodies and then simulates their interaction over a set time period. The simulation will also animate the interactions between the bodies.

### CB1: `CelestialBody` Variables, Constructor, and Getter Methods

This section introduces the `CelestialBody` class and describes its instance variables, constructor, and getter methods, which you will need to implement. 

<details>
<summary>CelestialBody Instance variables</summary>

The outline below shows the constructor, methods, and instance variables (or fields)  of the `CelestialBody` class. All instance variables should be `private`. All methods should be `public` (if you write helper methods they should be `private`). *When you clone the project, you'll get a `CelestialBody` class with the instance variables and stub methods already implemented*.

<div align="center">
  <img width="400" height="400" src="p1-figures/celestialBodyMethods.png">
</div>

There are six instance variables: `myXPos`, `myYPos`, `myXVel`, `myYVel`, `myMass`, `myFileName`. The first five have type `double`, the last is a `String`.
</details>

<details>
<summary>CelestialBody Constructor</summary>

There is one constructor: it has six parameters, one for each instance variable. The signatures of is shown below. 

<div align="center">
  <img width="576" height="248" src="p1-figures/celestialBodyConst1.png">
</div>

*You'll write code to assign valuesl to *each of six* instance variables using the six parameters.*

</details>

<details>
<summary>CelestialBody Getter Methods</summary>
You'll also write six _getter methods_ specified in the class. The body of each method is a single return statement, returning the value of the corresponding instance variable. These getter methods allow the values of `private` instance variables to be accessed outside the class. For example, the method `getXVel()` is shown below. These are getter methods because they *do not allow client programs to set the values, only to get the values*.

<div align="center">
  <img width="310" height="115" src="p1-figures/getXVel.png">
</div>

</details>

When you've implemented the constructor and the six getter methods you should be able to run the program in `TestBodyConstructorGetters.java` to see if your code is correct. When it reports that everything works you can proceed to the next step in implementing the `CelestialBody` class. The report from running `TestBodyConstructorGetters` indicates whether each getter method passes.

## Additional `CelestialBody` Methods: update instance variables

Now that you have successfully coded the constructor and getter methods for the `CelestialBody` class, you'll proceed to implement the additional methods described here.

### CB2: The method CelestialBody.calcDistance

<br>

 <div align="center">
  <img width="420" height="128" src="p1-figures/calcDistance.png">
</div>

<br> 

This method returns the distance between two `CelestialBody` objects. Use the standard distance formula to determine the distance between `this` body (using `myXPos` and `myYPos` or `this.myXPos` and `this.myYPos`) and the `CelestialBody` object referenced by the parameter `b`. The distance is the value of $`r`$ in the formula below where

```math
r^2=dx^2 + dy^2
```

where $`dx`$ is delta/difference between $`x`$-coordinates, similarly for $`dy`$.  You can use the static method `Math.sqrt` to calculate the square root of a number.

### CB3 The method CelestialBody.calcForceExertedBy

<br>

<div align="center">
  <img width="372" height="58" src="p1-figures/calcForceExertedBy.png">
</div>

<br>

This method calculates and returns the force exerted on `this` body by the body referenced by the parameter `b`. You should calculate the force using the formula below. You can read about the physics of the formula in the [NBody Physics document][Physics].

```math
F = G\frac{m_1m_2}{r^2}
```
 
Here $`m_1`$ and $`m_2`$ are the masses of the two bodies, $`G`$ is the gravitational constant ($`6.67 \cdot 10^{-11}\frac{N-m^2}{kg^2}`$), and $`r`$ is the distance between the two objects. Call `calcDistance` to determine this distance. You can specify $`G`$ as $`6.67 \cdot 10^{-11}`$ (alternatively, 6.67*1e-11) using scientific notation in Java. 

When you've implemented this method, test it by running `TestCalcForceExertedBy.java`.

### CB4: The methods CelestialBody.calcForceExertedByX and calcForceExertedByY

<br> 

<div align="center">
  <img width="428" height="20" src="p1-figures/calcForceExertedByX.png">
</div>

<br> 

These two methods describe the force exerted in the X and Y directions, respectively. The signature of `calcForceExertedByX` is shown above; `calcForceExertedByY` has a similar signature. 

You can obtain the $`x`$- and $`y`$-components from the total force using the formulas below, where $`F`$ is the value returned by `calcForceExertedBy`, $`r`$ is the distance between two bodies, and $`F_x`$ and $`F_y`$ are the values to be returned by `calcForceExertedByX` and `calcForceExertedByY`, respectively. Note that $`dx`$ and $`dy`$ in the formula are the differences between $`x`$ and $`y`$ coordinates, respectively, between the original body (`this`, the object on which the method is called) and the exerting body (the argument to the method).

```math
F_x = F\frac{dx}{r}\\~\\
F_y = F\frac{dy}{r}
```

Note: Be careful with the signs! In particular, be aware that $`dx`$ and $`dy`$ are signed (positive or negative). By convention, we define the positive $`x`$-direction as towards the right of the screen, and the positive $`y`$-direction as towards the top.

*Also note:* While mathematically `F/r * dx` is the same as `F*dx/r`, because of roundoff error these may not be the same computationally. You should use `F*dx/r` in your method. *Please be attentive to this in your code!*

You can test these methods using the program in `TestCalcForceExertedByXY.java`.

### CB5: The method CelestialBody.calcNetForceExertedByX and calcNetForceExertedByY

<br>

<div align="center">
  <img width="428" height="20" src="p1-figures/calcNetForceExertedByY.png">
</div>

<br>

This method returns the total/net force exerted on this body by all the bodies in the array parameter. The principle of superposition ([see Physics][Physics]) says that the net force acting on a `CelestialBody` object by many other bodies is the sum of the pairwise forces acting on the `CelestialBody` by each body. So you'll need to sum the forces returned by `calcForceExertedByX` (or `Y`) in calculating the value to return. 

You must make sure _**NOT to include the force exerted by a body on itself!**_ The universe might collapse (Infinite/NaN error) if an object attracted itself. If you loop over each element in array `bodies`, you'll need to check explicitly with code that looks something like the following.

<br> 

<div align="center">
  <img width="289" height="39" src="p1-figures/forLoop.png">
</div>

You can test the code for this method by running the program in `TestCalcNetForceExertedByXY.java`.

### CB6: The method CelestialBody.update

<div align="center">
  <img width="379" height="39" src="p1-figures/update.png">
</div>

This method is a so-called _mutator_. It doesn't return a value, but updates the state/instance variables of the `CelestialBody` object on which it's called. 

This method will be called during the simulation to update the body's position and velocity with small time steps (the value of the first parameter, `deltaT`). The values of parameter `xforce` and `yforce` are the net forces exerted on this body by all other bodies in the simulation. When code calls the update method from `NBody.java`, you will determine the values of the arguments passed as these two parameters by calling `calcNetForceExertedByX` (or `Y`). In the formulas you're writing below the parameter `xforce` is $`F_x`$ and `yforce` is $`F_y`$. 

This update method updates the instance variables `myXPos`, `myYPos`, `myXVel`, and `myYVel` in four steps.

1. First, calculate the acceleration using Newton's second law of motion where $`m`$ is the mass of the `CelestialBody`. This creates two variables for acceleration in the $`x`$ and $`y`$ directions.

```math
a_x = \frac{F_x}{m}\\~\\
a_y = \frac{F_y}{m}
```
 
2. You'll then calculate values for new `myXVel` and `myYVel`, we'll call these `nvx` and `nvy` where the $`n`$ is for new, using the relationship between acceleration and velocity, e.g., `nvx = myXVel + deltaT*ax`.

3. You'll use `nvx` (and a corresponding `nvy`) to calculate new values for `myXPos` and `myYPos` using the relationship between position and velocity, e.g., `nx = myXPos + deltaT*nvx`.

4. _**After**_ you've calculated `nx`,`ny`,`nvx`, and `nvy`, you'll assign these to the instance variables `myXPos`, `myYPos`, `myXVel`, and `myYVel`, respectively. The key in *after* is that you do not update instance variable values until you've calculated each of the `nx`, `ny`, `nvx`, and `nvy` values.

These steps will update the position and velocity of the body making the simulation possible. You can test this method using `TestUpdate.java`.

### The method CelestialBody.draw


This **void** method is described below in the section for `NBody` that describes where to call the CelestialBody.draw method. _**This method is already written, you don't need to write or edit it.**_

After developing, implementing, testing, and debugging these `CelestialBody` methods you're ready to move to the simulation code.

## The `NBody` Class

The `NBody` class will use `CelestialBody` objects to run the simulation.

This class consists only of `static` methods, including the main method that runs the simulation. Your task will be to implement the three `static` methods that have been outlined for you in the starter code. That code has `// TODO` comments indicating where you need to make edits.

<div align="center">
  <img src="p1-figures/NBodyMethods.png">
</div>

### Details on Data Format

The data for planets, suns, and celestial bodies in general is in the format shown below. All files in the folder data are in this format. This is the file `planets.txt`:

<div align="center">
  <img src="p1-figures/format.png">
</div>

The first value is an integer _**n**_, the number of bodies for which data is given in the file. The next value is a `double`, the radius of the universe for the simulation. This value is used to set the scale for the animation.

Next, there are _**n**_ lines, one line for each `CelestialBody`. Each line contains six values as shown above. The first five values are `doubles`: the first two are initial x and y coordinates; the next two are initial x and y velocities; the next is the mass of the `CelestialBody`. The last value on a line is a `String` specifying the file in the images folder used for the animation of the simulation.


## NBody Methods

This section describes the details of each of the three `static` methods you need to implement for the `NBody` class.

### The method NBody.readRadius

Given a file name, this method should return a double corresponding to the radius of the universe in that file, e.g. `readRadius("./data/planets.txt")` should return $`2.50 \cdot 10 ^{11}`$ (alternatively, 2.50e+11). You'll need to read the `int` value that's the number of bodies, then read the `double` value for the radius using the `Scanner` already created in the starter code. Use `s.nextInt()` and `s.nextDouble()` for the `Scanner` variable `s` to read an `int` and `double` value, respectively. Your code in `readRadius` must read both values, but only the radius is returned. The number of bodies (first value in a data file) is ignored.

You can test your method using the provided `TestReadRadius.java` program.

### The method NBody.readBodies

This method returns an array of `CelestialBody` objects using the data read from the file. For example, `readBodies("./data/planets.txt")` should return an array of 5 `CelestialBody` objects. You will use the number of bodies (first value in data file) to create a `CelestialBody []` array of the correct size to return. When created, each value in the array will be `null`, but you will read the values on each line and use these as parameters when you call `new` and create a `CelestialBody` object with the parameters on each line of the file.

As you iterate through the information for each of the `CelestialBody` objects in the file (one per line), you will find the `nextInt()`, `nextDouble()`, and `next()` methods in the Scanner useful in reading `int`, `double`, and `String` values, respectively. Note that `next()` returns a `String`.

You can test this method using the supplied `TestReadBodies.java` class. 

## Note on Running the Simulation

When the simulation is over your code prints out the final state of the universe in the same format as the input, you can expand below for an example.


<summary>Example Simulation Output</summary>

|             |             |             |           |           |          |
| :---        |    :----:   |       :---: |  :---:    | :---:     | ---:     |
| 5           |             |             |           |           |          |
| 2.50e+11  |            |             |             |           |           |
| 1.4631e+09 | 1.4943e+11 | -2.9831e+04 | 4.0749e+02 | 5.9740e+24 |earth.gif |
|-1.1174e+11 |-1.9803e+11 |  2.0989e+04 | -1.1953e+04 |  6.4190e+23 |   mars.gif |
| 2.4125e+10 | 5.2103e+10 | -4.3685e+04 | 2.0627e+04 | 3.3020e+23 | mercury.gif |
| 5.6664e+05 | 7.0808e+06 | 1.0861e-01 | 1.0639e-01 | 1.9890e+30  |    sun.gif |
| 1.0555e+11 | 2.3363e+10 |-7.5708e+03 | 3.4204e+04 | 4.8690e+24 |   venus.gif |

When the simulation finishes, you'll need to close/quit the graphics window to be able to run another simulation. Use the red X button in the upper left of the graphics window to dismiss the window.
