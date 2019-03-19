---
title: "Fixed Point Iteration"
date: 2018-02-01
tags: [test, post]
header:
  #image:
excerpt: "this is my test post"
mathjax: "true"
published: true
---
# False Position Method
### INTRODUCTION
This method solves for a fixed point meaning a point on a function at
which the following is true: $f(x) = x$. If we set a function equal to this
value, say call it g(x), then we say $g(x) = f(x) - x$, and that $g(x) = 0$,
where $f(x) = x$, i.e. a fixed point. We require a starting guess, a
function, and the error tolerance


### INITIAL PARAMETERS
* starting_point - the initial guess
* error - the error tolerance
* f(x) - the function for which we are solving the real roots

```matlab
starting_point = 1.5;
error = 1*10.^(-6);
g = @(x) x/2 + 1/x;
```
### Applying the function
p_n:  [n x 1] vector holding approximated fixed point
n:    [n x 1] vector holding the total iterations

```matlab
[p_n,n]=fixed_Point_Iteration(g,error,starting_point);
```
### PLOT PARAMETERS
Plotting the estimated fixed point vs the iteration number
```matlab
plot(n,p_n)
title('Approximated Fixed Point vs Iteration')
xlabel('Approximated x Value of The Fixed Point')
ylabel('iteration number')
legend('p(n)')
```
<img src="{{ site.baseurl }}/images/numerical_analysis/linear_methods/false_position/plot.png">

### Fixed Point Iteration Function
#### INPUTS (3):
1. The function we are trying to find the fixed point of
2. The error tolerance (desired accuracy)
3. The initial guess from which to begin iterating

#### OUTPUTS (2):
1. The error estimate at each iteration
2. The iteration number

#### COMMENTS:
1. Asymptotic error constant (lambda) is calculated but not output.
2. The error is the approximate error, which can also be added as an
   output

```matlab
function [p,index] = fixed_Point_Iteration(func,error,initial_point)
    % Initializes the index
    index = 1;
    % Initilizes the vector holding the fixed points
    p(index)= initial_point;
    % The stopping criterion set to allow loop below to iterate
    stop = error+1;

    % Iterative method continues until desired error met
    while (stop > error)

      index=index+1;
      fprintf('current step: %d\n',index)

      % Recursive approximation formula to approximate the fixed point
      p(index)=func(p(index-1));
      fprintf('p(%d) = %.9d, p(%d) = %.9d\n',index-1,p(index-1),index,p(index));

      % The error approximation.
      % The estimated error is returnable as a vector
      if index>=2
        ErrorEst(index-1)=abs(p(index)-p(index-1));
        % set scalar value at iteration to stop function rather than trying
        % Used to circumvent the stopping criterion above without
        % incorrectly setting the error estimate as error+1
        stop = ErrorEst(index-1);
      end

      fprintf('stopping error approximation(%d): %.9d\n\n',index-1,stop);

    end
    index = [1:index]';
end
```
