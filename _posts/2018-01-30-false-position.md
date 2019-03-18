---
title: "False Position"
date: 2018-01-30
tags: [test, post]
header:
  #image:
excerpt: "this is my test post"
mathjax: "true"
published: true
---
# False Position Method
### INTRODUCTION
We would like a recursive algorithm that does the following:
1. start with point slope formula: $$y-y(n)=m(x-x(n))$$
2. solve for fixed points by setting: $$y=0 , x=p(n)$$

    $$-y(n)=m(p(n)-x(n))$$

    $$p(n)=x(n)-y(n)* (1/m)$$
3. Here, we need an initial guess for the left and right bounds: a,b
    $$p(n) = b(n) - f(b(n)) * (b(n)-a(n)) / (f(b(n)) - f(b(n)))$$

4. We then update the formula using a similar procedure as in bisection method. We choose the point that does not have the same sign as p(n) to stay the same, and the boundary point that shares the same sign as p(n) gets set as p(n).

5. Eventually either one of the endpoints will become fixed, and only one side will constantly update.


### INITIAL PARAMETERS
* a - the left bound
* b - the right bound
* error - the error tolerance
* f(x) - the function for which we are solving the real roots

```matlab
a(1,1) = 2;
b(1,1)=3;
error = 1*10.^(-6);
f = @(x) x.*x - 5;
```
### Applying the function
setting vectors holding the left, right, and approximation for each
iteration. We are also taking a vector holding the error at each
iteration and a vector holding the number of iterations
% Setting 5 variables as the output for the function
% 1) p_n: [n x 1] vector holding approximated values
% 2) i_n: [n z 1] vector holding iteration number

```matlab
[p_n,i_n] = false_position(a,b,f,error);
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
<img src="{{ site.baseurl }}/images/numerical_analysis/linear_methods/false_position/plot.png">

#### BISECTION METHOD FUNCTION
This function has 4 inputs:
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
