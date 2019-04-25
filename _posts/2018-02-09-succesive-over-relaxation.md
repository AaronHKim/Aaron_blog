---
title: "Successive Over Relaxation"
date: 2018-02-08
#tags: [post]
header:
  #image:
excerpt: "successive over relaxation code"
mathjax: "true"
published: true
toc: true
toc_sticky: true

sidebar:
  #- title: "Title"
  #  image: http://placehold.it/350x250
  #  image_alt: "image"
  #  text: "Some text here."
  #- title: "Another Title"
  #  text: "More text here."
  - title: "Numerical Analysis"
    nav: sidebar-numerical
---
#### INTRODUCTION
A recursive matrix root finding algorithm that takes in an initial
matrix, and solves for a fixed point solution for a system of linear
equations. It solves the following problem: x(:,k+1) = A * x(:,k) + c(:,1)
Conjugate Gradient solves this equation by solving the equation for
steepest decent of the conjugate A-conjugate search directions, assuming
they exist.


#### INITIAL PARAMETERS
* A: [n x n] initial matrix
* b: [n x 1] vector solution
* x: [n x 1] The fixed point vector being guessed
* epsilon: an error tolerance

$$A = \begin{bmatrix}
 4 & 2 & 0\\
2 & 10 & 3\\
0 & 3 & 5\\
\end{bmatrix} b = \begin{bmatrix}
0 \\
-9 \\
9 \\
\end{bmatrix}
x = \begin{bmatrix}
0\\
0\\
0\\
\end{bmatrix}$$

```matlab
a=[4,2,0;2,10,3;0,3,5];
b=[0;-9;9];
x=zeros(size(a,1),1);
epsilon = 1*10^-6;
```

#### Applying the function
This method stores the two outputs:
* k: the iteration at which the vector converged
* x: the approximated vector fixed points at each iteration

```matlab
[k,x]=conjugate_gradient(a,b,x,epsilon);
```

#### Gauss Elimination FUNCTION

##### This function has 1 input:
1. a: [n x n] matrix we are trying to find the fixed point of
2. b: [n x 1] vector we are trying to find solutions for
3. x: [n x k+1] vector that holds the fixed point approximations for each
      iteration
4. epsilon: The error tolerance

##### This function has 1 output:
1. k: [1 x 1] the iteration number   
2. x: [n x k+1] matrix holding the iterative approximation for the fixed point at iteration k

```matlab
function [k,x]=conjugate_gradient(a,b,x,epsilon)
    %INITIALIZING VARIABLES
    %scalar holding the iteration number
    k=1;
    % The initial equation we are solving the convergence for
    r = a.*x(:,k)-b;
    % the initial direction of steepest decent is -r
    d(:,1)=-r(:,1);

    % Sets a measurement function which can be thought of as distance in
    % cartesian coordinates as the error tolerance.
    while norm(r(:,k))>epsilon

        % The step size (a constant)
        lambda(k)=((-transpose(d(:,k))*r(:,k)))/(transpose(d(:,k))*a*d(:,k));
        % The updated approximation x(k+1)= x(k) + the stepsize and distance
        x(:,k+1)=x(:,k)+lambda(k)*d(:,k);
        % The residual of the new aproximation (the error)
        r(:,k+1)=a*x(:,k+1)-b;
        % A constant to make sure that the distance is in the direction of
        % a conjugate
        alpha(k)=(transpose(r(:,k+1))*a*d(:,k))/(transpose(d(:,k))*a*d(:,k));
        % The updated distance for iteration k+1
        d(:,k+1)=-r(:,k+1)+alpha(k)*d(:,k);
        %iteration update
        k=k+1;
    end
end
```
##### Function Output
```matlab
>> conjugategradient
[ 0,    0, 0.48175182,  1.0]
[ 0, -2.0, -1.8394161, -2.0]
[ 0,  2.0,  3.0437956,  3.0]

```
