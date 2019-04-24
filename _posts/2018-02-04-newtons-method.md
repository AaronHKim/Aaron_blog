---
title: "Newton's Method(linear)"
date: 2018-02-29
#tags: [post]
header:
  #image:
excerpt: "linear newtons method"
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
Newton's Method is a Fixed Point Iteration Method that uses a slightly different function for g(x) rather than the standard method. This method uses g(x) = x - f(x)/f'(x). This requires an initial guess that is sufficiently close to the root, an error tolerance (As a stopping criterion, and a function which we are attempting to find the roots or fixed points of. (the two equations can be used interchangeably by including or excluding a few factors. The method will remain the same.)

#### INITIAL PARAMETERS
* starting_point - the initial guess
* error - the error tolerance
* g(x) - the fixed point equation we are trying to solve
* points - the number of points on the interval

```matlab
starting_point = 2.5;
error = 1*10.^(-6);
fprintf('starting point: %d\n',starting_point);
fprintf('the desired error is: %d\n\n',error);
g = @(x) x-(x.*x-5)./(2.*x);
```
#### Applying the function
This method stores the two outputs:
* p_n: an [n x 1] vector holding the approximated root of f(x)
* n:   [n x 1] vector holding the iteration number


```matlab
[p_n,n]=newtons_mtd(g,error,starting_point);
```
#### PLOT PARAMETERS

Plotting the estimated root vs the iteration number
```matlab
figure(1)
plot(n,p_n);
title('Approximated root vs Iteration')
xlabel('Iteration Number')
ylabel('Approximated x Value')
legend('p(n)')
```
<img src="{{ site.baseurl }}/images/numerical_analysis/linear_methods/newtons_method_linear/approximation_vs_iteration.png">

#### NEWTONS METHOD FUNCTION

##### This function has 3 inputs:
1. The function we are trying to find the fixed point of
2. The error tolerance (desired accuracy)
3. The initial guess from which to begin iterating

##### This function has 2 outputs:
1. The error estimate at each iteration
2. The iteration number

```matlab
function[p,n] = newtons_mtd(func,error,initial_point)
    % Begins at 1 since we already have an initial point
    index = 1;
    p(1)= initial_point;
    % set to error + 1 to satisfy loop condition.
    stop = error+1;

    while (stop > error)

      index=index+1;
      fprintf('current step: %d\n',index)

      % Updating Approximated Root
      p(index)=func(p(index-1));
      fprintf('p(%d) = %.9d, p(%d) = %.9d\n',index-1,p(index-1),index,p(index));

      % Conditions under which the Estimated Error can be calculated.
      % If desired the Estimated Error vector can be exported
      if index>=2
        ErrorEst(index-1)=abs(p(index)-p(index-1));
        % Updates while loop criterion
        stop = ErrorEst(index-1);
      end

      fprintf('stopping error approximation(%d): %.9d\n\n',index-1,stop);
    end
    % [n x 1] vector holding index value created for plotting purposes.
    n=[1:index];
end
```
