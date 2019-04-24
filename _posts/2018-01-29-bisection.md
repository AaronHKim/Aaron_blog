---
title: "Bisection Method"
date: 2018-02-29
#tags: [post]
header:
  #image:
excerpt: "this is the excerpt"
mathjax: "true"
published: true
toc: true
toc_sticky: true


sidebar:
  - title: "Numerical Analysis"
  # image: http://placehold.it/350x250
  #  image_alt: "image"
  #  text: "Some text here."
  #- title: "Another Title"
  #  text: "More text here."
    nav: sidebar-numerical
  # asdf
---
#### INTRODUCTION
A recursive root finding algorithm that takes in two values and finds the
a midpoint between them. The next iterate always chooses two values with
different signs. If the root exists and sign of f(left) and f(right) are
different (one is a negative number and the other is a positive one) then
this method will always converge. (See Intermediate Value Theorem)

#### INITIAL PARAMETERS
* a - the left bound
* b - the right bound
* epsilon - the error tolerance
* f(x) - the function for which we are solving the real roots
* points - the number of points on the interval

```matlab
a= 0;
b=2.5;
epsilon = 1*10.^-5;
f = @(x) x.*x-1;
points = linspace(a,b,100);
```
#### Applying the function
setting vectors holding the left, right, and approximation for each
iteration. We are also taking a vector holding the error at each
iteration and a vector holding the number of iterations

```matlab
[left,right,approx,error,i]=bisection(a,b,f,epsilon);
```
#### PLOT PARAMETERS
```matlab
% plotting the middle approximation vs the iteration
figure(1)
% plotting the left, right, and middle approximation on the x axis over the
% function itself. Blue indicates an initial guess, the closer to red the
% greater the iteration number
plot(i,approx)
figure(2)
scatter(approx,zeros(size(approx,1),1),25,linspace(1,10,size(approx,1)),'filled')
hold on
scatter(left,zeros(size(left,1),1),100,linspace(1,10,size(left,1)),'d')
scatter(right,zeros(size(right,1),1),100,linspace(1,10,size(right,1)))
plot(points,f(points))
hold off
```
<img src="{{ site.baseurl }}/images/numerical_analysis/linear_methods/bisection_method/approximation_vs_iteration.png">

<img src="{{ site.baseurl }}/images/numerical_analysis/linear_methods/bisection_method/example_of_bijection.png">

#### BISECTION METHOD FUNCTION
#### This function has 4 inputs:
1. The left bound
2. The right bound
3. The function
4. The error tolerance

#### This function has 5 outputs:
1. The left bound for each iteration, [n+1 x 1]
2. The right bound for each iteration, [n+1 x 1]
3. The middle approximated value for each iteration, [n x 1]
4. The error from value at each iteration, [n x 1]
5. A vector holding the iteration number, [n x 1]

```matlab
function [left_bound,right_bound,approximation,error,subyindex] = bisection(a,b,f,epsilon)
    %INITIALIZING VARIABLES
    left_bound = a;
    right_bound = b;
    approximation = [];
    i=1;
    subyindex = [];
    % Intial error set at twice epsilon to make sure the loop runs
    % This is overwritten in the first while loop.
    error = epsilon + epsilon;


    while(error>epsilon)
        % finding the midpoint between the left and right values
        approximation(i,1)= (left_bound(i,1) +right_bound(i,1))/2;
        fprintf('an=%d,bn=%d,pn=%d,f(an)=%d,f(bn)=%d,f(pn)=%d\n',left_bound(i,1),right_bound(i,1),approximation(i,1),sign(f(left_bound(i,1))),sign(f(right_bound(i,1))),sign(f(approximation(i,1))))

        % makes sure the midpoint and next iterations always have opposite
        % signs
        if sign(f(left_bound(i,1)))~=sign(f(approximation(i,1)))
            left_bound(i+1,1)=left_bound(i,1);
            right_bound(i+1,1)=approximation(i,1);
        else
            right_bound(i+1,1)=right_bound(i,1);
            left_bound(i+1,1)=approximation(i,1);
        end

        % calculates the residual of the error
        error(i,1) = abs(f(approximation(i,1)));
        % creates an index of the current iteration
        subyindex = [subyindex; i];
        i=i+1;
    end
    % prints the approximated root to 4 decimal points.
    fprintf('the root is approximately %.4f\n', approximation(i-1,1));
end
```
