## steepest descent

#### 1.
compute $\vec{x}^{(1)},\vec{x}^{(2)}$ using steepest descent $\{\vec{x}^{(k)}\}$ for $f(x_1,x_2)=2x_1^4+x_2^2-4x_1x_2+5x_2$ with initial point $\vec{x}^{(0)}=(0,0)$

$$\begin{align}
\nabla{f(\vec{x},\vec{y})} &= (8x_1^2-4x_2,2x_2-4x_1+5) \\
\nabla{f(0,0)} &=(0-0,0-0+5) = (0,5) \\
\phi_0(t) &= f(\vec{x}^{(0)}-t\nabla{f(\vec{x}^{(0)})}) \\
&= f\begin{pmatrix} \begin{bmatrix}0\\0\end{bmatrix} - t\begin{bmatrix}0\\5\end{bmatrix} \end{pmatrix} = f\begin{pmatrix}\begin{bmatrix}0\\5\end{bmatrix}\end{pmatrix}\\
\phi_0'(t) &= -\nabla{f(\vec{x}^{(0)}-t\nabla{f(\vec{x}^{(0)})})}\\
&= -(8(-t)^{2}-4(-5t),2(-5t)-4(-t+5))\dot(0,5)\\
&= 50(1-2t)\\
t&=\frac{1}{2}\\
\vec{x}^{(1)} &= \begin{bmatrix} 0\\0 \end{bmatrix}-\frac{1}{2}\begin{bmatrix} 0\\5\end{bmatrix} \\
&= \begin{bmatrix}0\\-2.5\end{bmatrix}\\
\\
\nabla{f(0,-2.5)} &=(0-4*-2.5,2*-2.5-0+5) = (10,0) \\
\phi_0(t) &= f(\vec{x}^{(1)}-t\nabla{f(\vec{x}^{(1)})}) \\
&= f\begin{pmatrix} \begin{bmatrix}0\\-2.5\end{bmatrix} - t\begin{bmatrix}10\\0\end{bmatrix} \end{pmatrix} = f\begin{pmatrix}\begin{bmatrix}-10t\\-5/2\end{bmatrix}\end{pmatrix}\\
\phi_0'(t) &= -\nabla{f(\vec{x}^{(1)}-t\nabla{f(\vec{x}^{(1)})})}\\
&=  (10 - 8000t^3,
          40t)\\
&= 100 - 80000t^3\\
t&=\frac{800^{\frac{2}{3}}}{800}\\
\vec{x}^{(2)} &= \begin{bmatrix} 0\\-2.5 \end{bmatrix}-\frac{800^{\frac{2}{3}}}{800}\begin{bmatrix} 10\\0\end{bmatrix} \\
&= \begin{bmatrix}  -1.077217345015942 \\
  -2.500000000000000\end{bmatrix}
\end{align}$$



#### 2.
So we know from the book that the steepest descent method is bounded by $f^{(k+1)}<f^{(k)}$, so if we set our error tolerance to that, then we know that our method should stop. The rest of the method is pretty much as prescribed by the book. We get the following output for our error tolerance being above $1\times10^{-6}$


 |k        |$x_1$       |$x_2$|
|:---:|:---:|:---:|
|     1|     0.0000000|     0.0000000|
|     2|     0.0000000|    -2.5000000|
|     3|    -1.0772173|    -2.5000000|
|     4|    -1.0772173|    -4.6544347|
|     5|    -1.3251925|    -4.6544347|
|     6|    -1.3251925|    -5.1503851|
|     7|    -1.3706816|    -5.1503851|
|     8|    -1.3706816|    -5.2413631|
|     9|    -1.3787052|    -5.2413631|
|    10|    -1.3787052|    -5.2574104|
|    11|    -1.3801108|    -5.2574104|
|    12|    -1.3801108|    -5.2602216|
|    13|    -1.3803568|    -5.2602216|
|    14|    -1.3803568|    -5.2607135|

Our rate of convergence is actually slower for the given error tolerance than is Newton's Method, since Newton's method converged to the same error tolerance in 9 iterations.

<figure>
  <img src="{{ site.baseurl}} /images/operations_reasearch/steepest_descent/Operations_research_homework_8_plot.png" alt="x1 vs x2 steepest descent plot"/>
  <figcaption><br>a plot showing convergence.</figcaption>
</figure>




```matlab
    %% Method of steepest descent
    % Introduction:
    % - - -
    % $f(x)$
    clear
    format long
    x=[0;0];

    f=@(x) 2*x(1)*x(1)*x(1)*x(1)+x(2)*x(2)-4*x(1)*x(2)+5*x(2);
    epsilon = 1*10^-6;

    [k,x_k] = conjugate_gradient(f,x,epsilon)
    fileID = fopen('approximation1.txt','w');
    fprintf(fileID,'%6s %13s %13s\n','k','x<sub>1</sub>','x<sub>2</sub>');
    fprintf(fileID,'%6.0f %13.7f %13.7f\n',[k;x_k]);
    fclose(fileID);

    plot(x_k(1,:),x_k(2,:),'LineWidth',1,'color','r');
    xlabel('x1')
    ylabel('x2')
    title('plot of the path that steepest descent takes')
    legend('path that steepest descent takes')

    function [k,x]=conjugate_gradient(f,x,epsilon)
        k=1;
        weight= 1;
        bound = f(x(:,k));
        norm_bound(1,k) = norm(bound(:,k),Inf)+2*epsilon;



        while norm_bound(1,k)>epsilon
            syms a b t c d
            y= [a;b];
            [gradf]=gradient(f(y));
            gradfxy=[subs(gradf,{a,b},{x(1,k),x(2,k)})];
            phi = y-t*gradfxy;
            phixy= subs(phi,{a,b},{x(1,k),x(2,k)});
            dphixy = subs(gradf,{a,b},{phixy(1),phixy(2)});
            dphi = dot(dphixy,gradfxy);
            dphi = subs(dphi, {a,b,conj(t)},{2,3,t});
            weight=solve(dphi,t, 'Real',true);
            x(:,k+1)=x(:,k)-weight*gradfxy;
            bound(:,k+1) = f(x(:,k+1))-f(x(:,k));
            k=k+1;
            norm_bound(1,k) = norm(bound(:,k),Inf);
        end
        k=[1:k];
    end
  ```
