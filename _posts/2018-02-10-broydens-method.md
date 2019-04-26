---
title: "Broydens Method"
date: 2018-02-10
#tags: [post]
header:
  #image:
excerpt: "broydens method code"
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
This is a program intended to solve for critical points of multivariable function

#### INITIAL PARAMETERS
* x: [n x 1] initial guess
* epsilon: error tolerance
* f: the function we are trying to find minima for
* jf: the jacobian of f

```matlab
format long
x=[2;4];
f=@(x) [(x(1)*x(1))-(x(2)*x(2));(-x(1).*x(2)+1)];
jf=@(x) [2*x(1), -2*x(2);-x(2),-x(1)];
epsilon = 1*10^-6;
```

#### Applying the function
This method stores the two outputs:
* x_k: [n x k+1] matrix holding the approximated solutions to the system of linear equations
* k: the iteration number we are at
* norm_residual_k: a upper bound for the error given by the largest absolute value of the sum of terms in a row


```matlab
[x_k,k,norm_residual_k] = broydens_mthd(x,f,jf,epsilon);
```
#### PLOT PARAMETERS
In the first plot, we are graphing (iteration vs the solution of each term) to show how each term
in the solution vector converges.
In the second plot we are graphing the upperbound of the error vs iteration to show that the error converges
with successive iterations

```matlab
figure(1)
plot(k,x_k(1,:),k,x_k(2,:))
figure(2)
semilogy(k,norm_residual_k)
```
<img src="{{ site.baseurl }}/images/numerical_analysis/linear_methods/square_root_approximation/approximation_vs_error.png">
#### broydens method FUNCTION

##### This function has 4 inputs:
1. x: an [n x 1] initial guess
2. f: an [n x m] system of equations that we wish to solve for roots of
3. jf: the Jacobian of f, whos dimensions are determined by [# of variables x # of equations]
4. epsilon: the error tolerance we are willing to accept

##### This function has 3 outputs:
1. x: A [ n x k], matrix holding up to the kth fixed point approximation
2. k: the iteration number of the Method
3. norm_residual: the norm of the residual of the function which we find by plugging the vector into the function

```matlab
while norm_residual(1,k)>epsilon
    if k > 1
       y = f(x(:,k)) - f(x(:,k-1));
       d = x(:,k) - x(:,k-1);
       top = (d - [inv_a(:,:,k-1)]*y)*d'*[inv_a(:,:,k-1)];
       bottom = d'*[inv_a(:,:,k-1)]*y;
       inv_a(:,:,k) = [inv_a(:,:,k-1)]+(top)/(bottom);
    end
    v(:,k)=-1*[inv_a(:,:,k)]*f(x(:,k));
    x(:,k+1) = x(:,k)+v(:,k);
    residual(:,k+1) = f(x(:,k+1));
    k=k+1;
    norm_residual(1,k) = norm(residual(:,k));
end
k=[1:k];
end
```
##### Function Output

```matlab
x_k =

  Column 16

     1.000506585613172
     0.999493414386828
     1.000000000000000

```
