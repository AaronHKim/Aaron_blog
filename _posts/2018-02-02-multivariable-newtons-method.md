---
title: "Newton's Method Nonlinear Method for Scalars"
date: 2018-02-02
#tags: [post]
header:
  #image:
excerpt: "This is a program intended to solve for critical points of multivariable
functions by applying Newton's Method."
mathjax: "true"
published: true
toc: true
toc_sticky: true
---

## Introduction

#### Linear Newton's Method

In a previous program we showed how to solve Newton's method for a single
variable by doing the following:

1. Have a continuous function for which there exists a minima, call it
$f(x)$
2. If we find the first derivative of the function, we find the critical
points of that function which can be a local minimum. i.e. $f'(x)=0$
3. We rename $f'(x)$ as $g(x)$ or $f'(x) = g(x)$ and solving for
where this g(x) intersects the origin (where it is zero).
4. We procedurally solve for this by solving for tangent points to $g(x)$
at a point. If you examine a given curve $g(x)$, if you use point slope
form: $y-g(x) = g'(x^{(k)}(x-x^{(k)}$, we find where it intersects the
axis by setting $y=0$ giving us: $g(x)= g'(x^{(k)}(x-x^{(k)})$, and to
find the next iterate, or the next approximation we set $x=x^{(k+1)})$.
This gives us the following formula: $$ g(x) =
g'(x^{(k)})(x^{(k+1)}-x^{(k)}) $$.
5. We can also solve for this explicitly by solving for $x^{(k+1)}$:
$$ x^{(k+1)} = x^{(k)}-\frac{g(x^{(k)})}{g'(x^{(k)})} $$

#### Multivariable Newton's Method

Here we would like to solve for the multivariable case, and we use a similar
logic:

1. Have a continuous multivariable function that has a minima call it $f(x)$
2. Find the gradient of $f(x)$ and set it equal to $g(x)$
3. Apply the point slope form to $g(x)$, set $y=0$ and set $x= x^{(k+1)}$
4. It is important to note that if you apply the gradient twice, you would
expect to get the Jacobian. If the function is scalar, you get the Jacobian which
has the form of the transpose of the Hessian. If the Hessian is symmetric, then
the two are equivalent. The Hessian it turns out is always symmetric if
Schwarz's Theorem or Clairaut's Theorem on equality of mixed partials holds
5. This gives us the following:
  * $\begin{bmatrix}\nabla{g(x^{(k)})}\end{bmatrix} (x^{(k+1)}-x^{(k)})=-g(x^{(k)}))$ (***implicit***)
  * $(x^{(k+1)})=x^{(k)}-\begin{bmatrix}\nabla{g(x^{(k)})}\end{bmatrix}^{-1}g(x^{(k)}))$ (***explicit***)
6. Using the equation above, if we assume that the Hessian is symmetric, then
we can swap $\begin{bmatrix}\nabla{g(x^{(k)})}\end{bmatrix}$ with the Hessian
$Hf$ to get the following equations:
  * $\begin{bmatrix}Hf(x^{(k)})\end{bmatrix} (x^{(k+1)}-x^{(k)})=-g(x^{(k)}))$ (***implicit***)
  * $(x^{(k+1)})=x^{(k)}-\begin{bmatrix}Hf(x^{(k)})\end{bmatrix}^{-1}g(x^{(k)}))$ (***explicit***)

## Example problem:
Computer the first two terms $x^{(1)}$, $x^{(2)}$ of the Newton's Method
sequence $x^{(k)}$ for minimizing the function
$$f(x_1,x_2)=2x_1^4+x_2^2-4x_1x_2+5x_2$$ with the initial point $x^{(0)}=(0,0)$

So we would like to find $g(x^{(k)})$ so that we can apply the formula above,
and then get $\nabla{g(x^{(k)})} = Hf(x_1,x_2)$, so we need to solve for
$\nabla{f(x_1,x_2)}$:

$$\begin{bmatrix} \frac{\partial f}{\partial x_1} \\
\frac{\partial f}{\partial x_2} \end{bmatrix} =
\begin{bmatrix}  8x_1^3 - 4x_2 \\ 2x_2-4x_1+5
 \end{bmatrix}$$

and we find that the Hessian $Hf(x_1,x_2)$ =

$$\begin{bmatrix} \frac{\partial^2 f}{\partial x_1^2} & \frac{\partial^2 f}{\partial x_1 \partial x_2}\\
\frac{\partial^2 f}{\partial x_2 \partial x_1} & \frac{\partial^2 f}{\partial x_2^2} \end{bmatrix} =
\begin{bmatrix}  24x_1^2 & -4 \\ -4 & 2
 \end{bmatrix}$$

So the final set up to solve this explicitly looks like the following:

$$x^{(k+1)}=x^{(k)}- \begin{bmatrix} 24(x_1^{(k)})^2 & -4 \\ -4 & 2\end{bmatrix}^{-1}
g(x^{(k)})$$

Our first iteration should look like the following:

$$x^{(1)}= \begin{bmatrix} 0 \\ 0 \end{bmatrix} - \begin{bmatrix} 0 & -4 \\ -4 & 2\end{bmatrix}^{-1}
\begin{bmatrix} 0 \\ 5 \end{bmatrix}$$

$$ = -\begin{bmatrix} \frac{-1}{8} & \frac{-1}{4} \\ \frac{-1}{4} & 0 \end{bmatrix}^{-1}
\begin{bmatrix} 0 \\ 5 \end{bmatrix}$$

$$ = \begin{bmatrix} \frac{5}{4} \\ 0 \end{bmatrix}$$

So we see our results show that our minimum converges to the below:

|k|$x_1$|$x_2$|
|:---:|:---:|:---:|
|1| 0.0000000|0.0000000|
|2| 1.2500000|0.0000000|
|3| 0.7203390|-1.0593220|
|4|-0.9026063|-4.3052127|
|5|-1.8840203|-6.2680406|
|6|-1.5157418|-5.5314836|
|7|-1.3941221|-5.2882442|
|8|-1.3805712|-5.2611424|
|9|-1.3804089|-5.2608179|

A plot of the above converging looks like the following:

  <img src="{{ site.baseurl}}/images/operations_reasearch/newtons_method/x_1_vs_x_2.png">

We can see the trace of Newton's Method along the surface in the plot below:
<img src="{{ site.baseurl }}/images/operations_reasearch/newtons_method/surface_plot.png">
Finally we can see that we indeed have a minimum (since it is a bit difficult to tell) below:
<img src="{{ site.baseurl }}/images/operations_reasearch/newtons_method/lowest_point_in_z.png">

##### Verifying Results Using Calculus
checking for critical points using the gradient:

$$\begin{bmatrix}\frac{\partial f}{\partial x_1} \\
\frac{\partial f}{\partial x_2} \end{bmatrix} =
\begin{bmatrix}  8x_1^3 - 4x_2 \\ 2x_2-4x_1+5
 \end{bmatrix}= \begin{bmatrix}  0 \\ 0 \end{bmatrix}$$

Solving the system of linear equations for the critical points:

$$\begin{pmatrix}
  8x_1^3 - 4x_2 &= \,0 \\
  2x_2-4x_1+5 &= \,0
\end{pmatrix}$$

$$\begin{pmatrix}
  8x_1^3 - 4x_2 &= \,0 \\
  +(4x_2-8x_1+10) &= \,0
\end{pmatrix}$$

$$\begin{pmatrix}
  8x_1^3 - 8x_1 + 10 &= \,0 \\
  x_1 \approx -1.38041 &, x_2 \approx -5.26082
\end{pmatrix}$$

Then solving the determinant of the Hessian (which we found above):

$$\begin{vmatrix}  24x_1^2 & -4 \\ -4 & 2
 \end{vmatrix} = 16(4x_1^2 - 1)$$

 So at the critical point above, we find that $D(-1.38041,-5.26082) = 105.95404 >0$, and
 $\frac{\partial^{2} f(x_1,x_2)}{\partial x_1^2} = 121.95403 >0$ so we have a local minimum giving us
 our confirmation that the numerical approximation works.


#### INITIAL PARAMETERS
* x - the initial guess
* epsilon - the error tolerance
* f(x) - the gradient of f(x) which we set equal to g(x) above
* hf - the Hessian

```matlab
x = [0,0];
epsilon = 1*10^-6;
f=@(x) [8*x(1)*x(1)*x(1) - 4*x(2);2*x(2)-4*x(1)+5];
hf=@(x) [24*x(1)^2, -4; -4, 2];

```
#### Applying the function
* x_k:  [2 x n] vector holding approximated fixed points
* k:    [1 x n] vector holding the total iterations
* norm_residual_k: [1 x n] vector holding the error for each iteration

```matlab
[x_k,k,norm_residual_k] = newton_mthd_nonlinear(x,f,hf,epsilon);
```
#### PLOT PARAMETERS
Plotting the estimated fixed point vs the iteration number
```matlab
figure(1)
plot(k,x_k(1,:),k,x_k(2,:))
figure(2)
plot(k,norm_residual_k)
figure(3)

[x_p,y_p] = meshgrid(-3:.2:1.5,-20:5);
z_surf = 2.*x_p.*x_p.*x_p.*x_p + y_p.*y_p - 4.*x_p.*y_p + 5*y_p;
surf(x_p,y_p,z_surf,'FaceAlpha',.6)

quadric_x_1=x_k(1,:).*x_k(1,:).*x_k(1,:).*x_k(1,:);
square_x_2=x_k(2,:).*x_k(2,:);
z_path = 2*quadric_x_1 + square_x_2 - 4*x_k(1,:).*x_k(2,:)+ 5*(x_k(2,:));
hold on
plot3(x_k(1,:),x_k(2,:),z_path, '-o','Color','r','MarkerSize',10,'MarkerFaceColor', 'r')
plot3(x_k(1,1),x_k(2,1),5,'o','Color','g','MarkerSize',10,'MarkerFaceColor', 'g')
plot3(x_k(1,9),x_k(2,9),-20.4141,'o','Color','y','MarkerSize',15,'MarkerFaceColor', 'y')
hold off
```

<img src="{{ site.baseurl }}/images/operations_reasearch/newtons_method/approximate_root_vs_iteration.png">
##### Above we see the $x_1,x_2$ values at every iteration through Newtons Method
<img src="{{ site.baseurl }}/images/operations_reasearch/newtons_method/norm_residue_bounds_error.png">
##### Above we see the infinity norm of the residual which bounds the error. We see that at eventually the error goes to zero

### Fixed Point Iteration Function
#### INPUTS (4):
1. The initial guess
2. The function we are trying to find the fixed point of
3. The Jacobian (or Hessian) of the function we are trying to find the minimum of
4. The error tolerance (desired accuracy)

#### OUTPUTS (2):
1. The error estimate at each iteration
2. The iteration number
3. The norm of the residual which is and upper bound of the error
(if the norm of the residual converges, then the error also converges)

```matlab
function [x,k,norm_residual] = newton_mthd_nonlinear(x,f,jf,epsilon)
  k=1;
  residual = f(x(:,k));
  % this line is here to guarantee the loop runs at least once
  norm_residual(1,k) = norm(residual(:,k),Inf);

  while norm_residual(1,k)>epsilon
      v(:,k)=-1*inv(jf(x(:,k)))*f(x(:,k));
      x(:,k+1) = x(:,k)+v(:,k);
      residual(:,k+1) = f(x(:,k+1));
      k=k+1;
      norm_residual(1,k) = norm(residual(:,k),Inf);
  end

  % outputing k iterates as a vector (formatting purposes)
  k=[1:k];

end
```
