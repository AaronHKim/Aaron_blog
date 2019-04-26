---
title: "Successive Over Relaxation"
date: 2018-02-09
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
Successive Over Relaxation is distinct from Jacobi and Gauss Seidel
because it makes use of a constant weighting factor, where convergence
may be faster  than both depending on the weighting factor chosen.

#### INITIAL PARAMETERS
* A: [n x n] initial matrix
* b: [n x 1] vector
* x: [n x 1] The fixed point vector being guessed
* k: the iteration number
* n: the dimension of the matrix and vector.
* w: weighting factor. This is a constant that converges best if 1 < w < 2.

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
w=3/2;
a = [4,2,0;2,10,3;0,3,5];
b=[0;-9;9];
epsilon = 1*10^-6;
```

#### Applying the function
This method stores the two outputs:
* x_k [n x k+1] matrix holding the approximated solutions to the system of linear equations
* x: the approximated vector fixed points at each iteration

```matlab
[x_k]=SOR(a,b,w,epsilon);
```

#### successive over relaxation FUNCTION

##### This function has 4 inputs:
1. The initial matrix A [n x n]
2. The solution vector b [n x 1]
3. The weight factor (scalar)
4. The error tolerance (scalar)

##### This function has 1 output:
1. A [ n x (k+1)], matrix holding the k+1 fixed point approximations

```matlab
function [x] = SOR(a,b,w,epsilon)
    % INITIALIZING VARIABLES
    %----------------------------------------------
    % Initialize x to satisfy the while loop
    x = zeros(size(a,1),1);
    % Initialize the size of n since we need to go thorough each row
    n=size(a,1);
    % These are for cosmetic purposes
    sum1 = 0;
    sum2 = 0;
    % setting the intiial iteration
    k=1;

    % The norm is used to determine whether or not the matrix is within a
    % the desired error tolerance (You can think of it as a sort of
    % distance formula)
    while norm(a*x(:,k)-b)>epsilon
        % goes through each row
        for i=1:n

            % The first summation
            for j = 1:i-1
                sum1=sum1+a(i,j).*x(j,k+1);
            end

            % The second summation
            for l=i+1:n
                sum2=sum2+a(i,l).*x(l,k);
            end

            % The iterative approximation formula
            x(i,k+1)=(1-w).*x(i,k)+(w/a(i,i))*(b(i,1)-sum1-sum2);

            % resets the sums
            sum1=0;
            sum2=0;
        end

        %increases the index of k+1
        k=k+1;
    end

end
```
##### Function Output
```matlab
>> successiveoverrelaxation
  Columns 1 through 15

           0           0   1.0125000   1.5491250   0.6418238   1.0711814   1.0252382   0.9918509   0.9872134   1.0115144   0.9965633   0.9995795   1.0004215   1.0002458   0.9996554
           0  -1.3500000  -2.7405000  -1.8885150  -1.8561244  -2.0811052  -2.0059600  -1.9775186  -2.0068281  -2.0030940  -1.9971482  -2.0002816  -2.0006087  -1.9997044  -1.9999572
           0   3.9150000   3.2089500   2.7951884   2.9729178   3.0865357   2.9620960   2.9987187   3.0067861   2.9993916   2.9977376   3.0013847   2.9998555   2.9998062   3.0000584

  Columns 16 through 26

   1.0001402   0.9999972   0.9999845   0.9999971   1.0000095   0.9999949   1.0000007   1.0000005   0.9999999   0.9999998   1.0000001
  -2.0000896  -1.9999775  -1.9999859  -2.0000107  -1.9999995  -1.9999975  -2.0000010  -2.0000002  -1.9999996  -2.0000000  -2.0000000
   3.0000515   2.9999540   3.0000103   3.0000045   2.9999974   2.9999990   3.0000014   2.9999995   3.0000000   3.0000000   3.0000000


```
