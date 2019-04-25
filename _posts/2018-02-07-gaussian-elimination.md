---
title: "Gaussian Elimination"
date: 2018-02-07
#tags: [post]
header:
  #image:
excerpt: "Gaussian Elimination Code"
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
A standard method for reducing matricies that can be solved easily. This
makes sure that the lower triangle of the matrix is all set to zeros,
which allows for a quick recursive iteration to solve the variables for
each variable algebrically. Gauss-Jordan performs a similar task.

#### INITIAL PARAMETERS
* B - the matrix we would like to reduce

$$\begin{matrix}
 3 & -1 &  1 &  2 &  0 & 10\\
-2 & -4 &  2 & -1 & -2 &  1\\
-4 &  3 &  2 &  1 &  0 & -17\\
 1 & -2 &  4 &  1 & -3 &  4\\
-3 &  4 & -2 &  2 &  1 & -19\\
\end{matrix}$$
```matlab
B = [3 -1 1 2 0 10; -2 -4 2 -1 -2 1; -4 3 2 1 0 -17; 1 -2 4 1 -3 4; -3 4 -2 2 1 -19];
```


#### Applying the function
This method stores the three outputs:
* approx: vector of approximated values from the bisection method.
* error:  vector of errors for each iteration
* length: number of iterations done in the algorithm



```matlab
[approx, error, length] = square_root_approx_true(initial_guess, square_root, total_iterations);
```
#### PLOT PARAMETERS

The plot function plots the x-axis values, then the y-axis values
the below plots two different plots on the same graph.
We are plotting the approximate values and the error on the same graph
```matlab
figure(1)
plot(length,approx,length,error)
title('square root approximation and error')
xlabel('iteration number')
ylabel('approximate value')
legend('approximate value','error')
saveas(figure(1),[pwd '/approximation_vs_error.png']);
```
<img src="{{ site.baseurl }}/images/numerical_analysis/linear_methods/square_root_approximation/approximation_vs_error.png">

#### SECANT METHOD FUNCTION

##### This function has 3 inputs:
1. The initial guess for the recursive approximation
2. The true value of the approximation (to compare to the error)
3. The number of desired iterations


##### This function has 3 outputs:
1. a [1 x nMax] matrix of each approximation of the square root for a value
2. a [1 x nMax] matrix of the error at each iteration of the approximation
3. a [1 x nMax] matrix containing the current step for plotting purposes


```matlab
function [sq_rt_apprx,error,length] = square_root_approx_true(initial_guess, value, nMax)
    % A 1 x nMax matrix to contain the approximation of each iteration
    sq_rt_apprx = [1:nMax];
    % A 1 x nMax matrix to contain the error of each iteration
    error = [1:nMax];
    % A 1 x nMax matrix that counts the iteration number for plotting
    length = [1:nMax];
    % This sets the first value of the array as the initial guess
    sq_rt_apprx(1,1) = initial_guess;

    %Since we have the first value there are only nMax-1 values left to
    %approximate
    for i=1 : (nMax-1)
        %the simple formula: x(n+1) = (1/2)*(x(n)+ a/(x(n)))
        %if a square root is unknown this formula will eventually converge to a
        %desired square root
        sq_rt_apprx(1,i+1) = (1/2) * (sq_rt_apprx(1,i) + value/(sq_rt_apprx(1,i)));
        %this is the actual error of the approximation for each iteration
        error(1,i) = abs(sq_rt_apprx(1,i)-sqrt(value));
    end
    %since the for loop contains nMax-1 total iterations, we need to find the
    %error for the final iteration nMax, which is done here.
    error(1,nMax) = abs(sq_rt_apprx(1,nMax) - sqrt(value));
end
```
