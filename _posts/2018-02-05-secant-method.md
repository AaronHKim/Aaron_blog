---
title: "Secant Method"
date: 2018-02-05
#tags: [post]
header:
  #image:
excerpt: "secant method"
mathjax: "true"
published: true
toc: true
toc_sticky: true


sidebar:
  - title: "Title"
    image: http://placehold.it/350x250
    image_alt: "image"
    text: "Some text here."
    nav: sidebar-numerical
  - title: "Another Title"
    text: "More text here."
    nav: sidebar-numerical
---
#### INTRODUCTION
Secant Method is an iterative root finding algorithm that approximates
roots using a succession of secant lines, by finding the next iteration
based off the previous two iterations.
This method does not always converge, hence it requires an initial guess
that is somewhat close to the root. It also requires at least two initial
guesses, a function that we are attempting to find the root of, and
the error tolerance.


#### INITIAL PARAMETERS
* first_pt - the first of two initial guesses required for the secant method
* second_pt - the second of two initial guesses required for secant method.
* epsilon - the error tolerance
* f - the equation we are trying to solve the fixed points of
* p - the matrix intended to hold the fixed points

```matlab
p = 0;
f = @(x) x.^2-5;
epsilon = 1.*10.^-6;
first_pt=2.25;
second_pt=2.75;
```
#### Applying the function
This method stores the two outputs:
* p_n: an [1 x n] vector holding the approximated root of f(x)
* n:   [n x 1] vector holding the iteration number
* error: the approximate error for our method


```matlab
[p_n,error,n]=secant(first_pt,second_pt,p,f,epsilon);
```
#### PLOT PARAMETERS

Plotting the estimated root vs the iteration number
```matlab
figure(1)
plot(n,p_n)
title('Secant Method approximation')
xlabel('Iteration Number')
ylabel('Approximated Root Value')
legend('Approximated Root at n')
```
<img src="{{ site.baseurl }}/images/numerical_analysis/linear_methods/secant_method/approximation_vs_iteration.png">

#### SECANT METHOD FUNCTION

##### This function has 5 inputs:
1. The first initial guess
2. The second initial guess
3. The vector containing the fixed points per iteration
4. The function we are trying to determine the fixed points of.
5. The error tolerance (how much error we are willing to accept)

##### This function has 3 outputs:
1. Fixed Points for each iteration, Dimension: [1 x n]
2. Superlinear error approximation, Dimension: [1 x n-1]
3. Iteration number, Dimension: [1 x n]

```matlab
function [fixed_point,error,n] = secant(first_guess,second_guess,fixed_point,func,epsilon)
    %initializing variables for the while loop
    i = 0;
    error= epsilon + epsilon;
    fixed_point(1)=first_guess;
    fixed_point(2)=second_guess;

    while(error>epsilon)
        i=i+1;
        %set to i=2 or greater since, the algorithm requires p(n) and
        %p(n-1) to run.
        if i>1
            % named run because it is a difference in x values
            run = fixed_point(i)-fixed_point(i-1);
            % names rise because it is a difference in y values
            rise = func(fixed_point(i))-func(fixed_point(i-1));
            % fixed point algorithm
            fixed_point(i+1) = fixed_point(i)-func(fixed_point(i)).*((run)/(rise));

        end
        % e(n-1)? abs(p(n)-p(n-1) < epsilon
        % I reindexed the formula since in Matlab there is no 0th
        % index position
        error(i) = abs(fixed_point(i+1)-fixed_point(i));
        % prints the fixed poitn and error at each iteration
        fprintf('p: %.9d,error:%.9d\n',fixed_point(i),error(i));
    end
    % prints an additional iteration of the root, which is even more
    % accurate than the previous
    fprintf('p: %.9d,error:N/A\n',fixed_point(i+1));
    % sets the number of total p(n) values calculated
    n=[1:i+1];
end
```
