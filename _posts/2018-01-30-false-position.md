---
title: "False Position"
date: 2018-01-30
tags: [post]
header:
  #image:
excerpt: "this is my test post"
mathjax: "true"
published: true
---

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
### PLOT PARAMETERS
plotting the estimated fixed point value vs the iteration
```matlab
figure(1)
plot(i_n,p_n)
title('approixmated fixed point vs iteration')
xlabel('iteration')
ylabel('approximated value')
legend('p(n)')
```
<img src="{{ site.baseurl }}/images/numerical_analysis/linear_methods/false_position/plot.png">

### False Position Approximation Method
This function has 4 inputs:

#### INPUTS (4):
1. The initial left bound
2. The initial right bound
3. The function we are finding the fixed point of
4. The error tolerance we are willing to accept

#### OUTPUTS (2):
1. The error estimate at each iteration
2. The iteration number

#### COMMENTS:
1. Asymptotic error constant (lambda) is calculated but not output.
2. The error is the approximate error, which can also be added as an
   output

```matlab
function [pn,i] = false_position(a,b,f,error)
    % printing the initial conditions
    fprintf('the initial left bound is: %d\n',a(1))
    fprintf('the initial right bound is: %d\n',b(1))
    fprintf('the desired error is: %d\n\n',error)

    % makes sure loop criterion is met
    stop= error+error;
    % initializing variables
    lambda = 0;
    i=1;

    % Iterative stopping criterion (approximated error) is greater than the
    % desired actual error. This is done so that if desired the stopping
    % criterion can be obtained from the function
    while (stop(i,1) > error)
      fprintf('current step: %d\n',i)
      % algorithm for recursively approximating fixed points
      pn(i,1)=b(i,1)-f(b(i,1))*((b(i,1)-a(i,1))/(f(b(i,1))-f(a(i,1))));
      fprintf('f(a(%d)) = %d, f(b(%d)) = %d\n',i,a(i,1),i,b(i,1));
      fprintf('p(%d) = %.9d\n',i,pn(i));

      % choosing criterion for bounds.
      if sign(f(a(i,1))) == sign(f(pn(i,1))) % choose left
        a(i+1,1)=pn(i,1);
        b(i+1,1)=b(i,1);
      else % choose right
        a(i+1,1)=a(i,1);
        b(i+1,1)=pn(i,1);
      end

      fprintf('a(%d)=%d b(%d)=%d\n',i+1,a(i+1,1),i+1,b(i+1,1))

      % Prevents loop from stopping before the second iteration
      if i<3
          stop(i+1,1) = error+error;
          fprintf('\n')
      end

      % Approximated error can only be calculated from n>2
      if i>2
        % Lambda is the asymptotic error constant
        lambda(i,1) = (pn(i,1)-pn(i-1,1))/(pn(i-1,1)-pn(i-2,1));
        fprintf('lambda is: %d\n',lambda(i,1))

        % Stop is the approximated error criterion
        stop(i+1,1) = abs((lambda(i,1))/(lambda(i,1)-1))*abs(pn(i,1)-pn(i-1,1));
        fprintf('stopping error approximation: %d\n\n',stop(i+1,1))

      end

      i=i+1;
    end

    % formatting the vectors to look appropriate if called
    stop(3) = [];
    stop(1:2) = 0;
    a(i)=[];
    b(i)=[];
    i = [1:i-1]';

end
```
