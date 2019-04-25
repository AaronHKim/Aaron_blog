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
each variable algebraically. Gauss-Jordan performs a similar task.

#### INITIAL PARAMETERS
* B - the matrix we would like to reduce

$$\begin{bmatrix}
 3 & -1 &  1 &  2 &  0 & 10\\
-2 & -4 &  2 & -1 & -2 &  1\\
-4 &  3 &  2 &  1 &  0 & -17\\
 1 & -2 &  4 &  1 & -3 &  4\\
-3 &  4 & -2 &  2 &  1 & -19\\
\end{bmatrix}$$
```matlab
B = [3 -1 1 2 0 10; -2 -4 2 -1 -2 1; -4 3 2 1 0 -17; 1 -2 4 1 -3 4; -3 4 -2 2 1 -19];
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
