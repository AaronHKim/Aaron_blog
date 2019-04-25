---
title: "Conjugate Gradient"
date: 2018-02-08
#tags: [post]
header:
  #image:
excerpt: "Cojugate Gradient Code"
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
Conjugate Gradient solves this equation by solving the equation for
steepest decent of the conjugate A-conjugate search directions, assuming
they exist.


#### INITIAL PARAMETERS
* A: [n x n] initial matrix
* b: [n x 1] vector solution
* x: [n x 1] The fixed point vector being guessed
* epsilon: an error tolerance

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
a=[4,2,0;2,10,3;0,3,5];
b=[0;-9;9];
x=zeros(size(a,1),1);
epsilon = 1*10^-6;
```

#### Applying the function
This method stores the one output:
* final_matrix: stores the final reduced matrix after performing gaussian elimination

```matlab
[final_matrix]=gauss_elim(B);
```

#### Gauss Elimination FUNCTION

##### This function has 1 input:
1. Takes in an [n x m] Matrix


##### This function has 1 output:
1. Outputs a reduced [n x m] Matrix


```matlab
function [Matrix] = gauss_elim(Matrix)

    % locks in the first row and reduces rows below
    for pivotrow=1 : size(Matrix,1)-1
        % in each iteration the pivot row changes as we move down the
        % matrix. This allows the subsequent iterations to always only
        % treat the reduced rows above it as finished pivot rows

        for row = pivotrow+1:size(Matrix,1)
            % creates a normalization factor for a row
            m=Matrix(row,pivotrow)/Matrix(pivotrow,pivotrow);
            % sets the element beneath the diagonal to zero. Since we know
            % the objective is to zero out the elements in each column, we
            % can just set them to zero and avoid the calculation and just
            % calculate the data we need in the rows above.
            Matrix(row,pivotrow)=0;

            % updates the remaining elements in the row that have been row
            % reduced
            for col=pivotrow+1:size(Matrix,1)-1
                Matrix(row,col)=Matrix(row,col)-m.*Matrix(pivotrow,col);
            end

        end

    end

end

```
##### Function Output
```matlab
final_matrix =

    3.0000   -1.0000    1.0000    2.0000         0   10.0000
         0   -4.6667    2.6667    0.3333   -2.0000    1.0000
         0         0    4.2857    3.7857         0  -17.0000
         0         0         0   -2.1833   -3.0000    4.0000
         0         0         0         0    1.0000  -19.0000
```
