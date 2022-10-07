---
layout: post
title:	a tutorial on neural ordinary differential equations
date:   2020-04-10 16:00:00
description: neural odes for the common people
---

The paper [Neural Ordinary Differential Equations](https://papers.nips.cc/paper/7892-neural-ordinary-differential-equations.pdf) won the Best Paper award at [NeurIPS 2019](https://nips.cc/Conferences/2019), but it was for its ideas and not ease of reading. As is often the case, it is difficult to put in 9 pages what probably requires twice as many. 

The main goal of the paper is to leverage the connection already drawn between ordinary differential equations (ODEs) and [Residual networks](http://openaccess.thecvf.com/content_cvpr_2016/papers/He_Deep_Residual_Learning_CVPR_2016_paper.pdf) (ResNets) to create ODE-powered neural networks. Likewise, the main goal of this blog post is to make sure we can all understand this connection more easily.

To begin, we need to talk about the importance of ResNets and how they helped the deep learning revolution. For a long time, neural networks used to be shallow, having only one (at most two) hidden layers. The reason was the vanishing gradient problem: as the gradient propagates back to previous layers, it gets smaller and smaller (depending on the activation function). But besides this practical reason, there was a theoretical one too. The universal approximation theorem [1] holds for neural networks even when there is one hidden layer, as long as it is not linear.

However, adding layers is what launched the deep learning revolution. Empirically, we had early empirical heuristics like pre-training and drop-out (both of which are not used much anymore) and ReLU (which still is). More recent theoretical work has shown that having many small ReLu layers with the number of neurons not bigger than size of the input + 4, also makes that network a universal approximator [2].

ResNets [3] are one more practical implementation that helps with the vanishing gradient problem, which at each layer simply add the input to the output as follows:

<p align="center"><img src="/assets/img/neuralodes/resnet.png" height="200" /></p>

In math form 
$$x_{n+1} = F(x_n) + x_n$$
				\item If $H(x)$ is the target, it is easier to approximate the residual $H(x) - x$ by $F(x)$
				\item Identity mapping means $F(x)=0$, easier than $F(x)=x$
				\item But why does this solve the gradient problem?



Vanilla neural network gradients:

Let the input be $x = x_0$, with intermediate layer outputs $x_1 = F_1(x_0)$, $x_2 = F_2(x_1)$ and output $y = F_3(x_2)$ and $F_n(x)=w_n x$ for simplicity

Vanilla net with two hidden layers: $y = F_3(x_2) = F_3(F_2(x_1)) = F_3(F_2(F_1(x_0)))$

Taking the gradient:
			$$ \frac{d y}{d w_1} = \frac{d F_3(x_2)}{d x_2} \frac{d x_2}{d w_1} =  \frac{d F_3(x_2)}{d x_2} \frac{d F_2(x_1)}{d x_1}  \frac{d x_1}{d w_1} = w_3 w_2 x_0
			$$


Residual network gradients
	Now let $x_1 = F_1(x_0) + x_0$, $x_2 = F_2(x_1) + x_1$ and $y = F_3(x_2) + x_2$
	ResNet: 
			$$ \begin{align*}
				y &= F_3(x_2) + x_2  \\
				  &= F_3( F_2(x_1) + x_1) +  F_2(x_1) + x_1 \\
				  &=F_3( F_2(F_1(x_0) + x_0) + F_1(x_0) + x_0) +  F_2(F_1(x_0) + x_0) + F_1(x_0) + x_0
			\end{align*} $$

Taking the gradient:
	$$ \frac{d y}{d w_1} =  w_3 w_2 x_0 + w_3 x_0 + w_2 x_0 + x_0 $$

Ordinary Differential Equations (ODEs)

Let $h(t)$ be a function and $h' = \frac{dh}{dt}$, i.e. $h^{(n)}(t) = \frac{d^n h}{d t^n}$.
General definition: $f \Big( t, h(t), h'(t), .., h^{(n-1)}(t) \Big) = h^{(n)}(t)$
Simple first-order example: $f(t, h(t)) = 5h(t) - 3\implies 5h - 3 = h'$ 
What is the solution? ($h \propto e^{5t}$)
Can we solve it numerically?

Euler's method:

Let's solve a first-order ODE $h' = f(h, t)$ with a given initial condition $h(0) = c$
Taylor approximation: $h(t + \alpha) = h(t) + \alpha h'(t) + ... $
Plugging in: $ h(t + \alpha) \approx h(t) + \alpha f(t, h(t)) $
Euler's method:  $\boxed{h_{n+1} = h_n + \alpha f(h_n, t_n)}$
Example: $h'(t) = 5h(t) - 3$ with $h(0)=1$ and $\alpha=1$
Table of solutions:

$$ 
\begin{array}{c|c|c|c}
	n & t & h_n(t) & h'_n(t)  \\ \hline
	0 & 0 & 1 & 2 \\ \hline 
	1 & 1 &  3 & 12 \\ \hline
	2 & 2 & 15 & 72
\end{array}
$$	


Neural ODEs

Comparing the two:
ResNets vs. Euler's method:

$$
\begin{array}{c|c}
	\text{ResNets} & \text{Euler's method} \\ \hline
	x_{n+1} = x_n + F_{n+1}(x_n) & h_{n+1} = h_n + \alpha f(h_n, t_n)
\end{array}			
$$

Can we model "depth" $\to \infty$?
Let the gradient be a neural network:
				$$ \frac{d z(t)}{d t} = f(z(t), t, \theta) $$

<p align="center"><img src="/assets/img/neuralodes/compare.png" height="300" /></p>

Training:
The adjoint method[4].
	$$ L(z(T)) = L \Big( z(0) + \int_{0}^T f(z(t), t, \theta) dt \Big) = L \Big( \text{ODE}(z(0), f, 0, T, \theta) \Big) $$

Don't naively backpropagate through the ODE solver
Solve a time-reversed ODE to get the gradient 
Define and solve a new ODE $a(t) = \partial L / \partial z(t)$:
$$ \frac{d a(t)}{d t} = - \frac{\partial f(z(t), t, \theta)}{\partial z} a(t),  \quad a(T) = \frac{\partial L}{\partial z(T)}  $$
The gradient with respect $\theta$ and the input are then:
$$ \frac{d L}{d \theta} = \int_{0}^{T} a(t)^T \frac{\partial f(z(t), t, \theta)}{\partial \theta} dt $$

Proof of the adjoint sensitivity method

Let's rewrite $L$ and define a new function $\mathcal{L}$:
$$ L(z) = \int_0^T l(z, t, \theta) dt + L(z_0), \quad l(z, t, \theta) = \frac{d L(z_t)}{dt} = \frac{\partial L^T(z_t)}{\partial z} f(z_t, t, \theta) $$

$$
\mathcal{L}(z, \frac{\partial z}{\partial t}, \theta):=\int_{0}^{T} l(z, t, \theta) d t+\int_{0}^{T} \lambda^{T} \Big( \frac{\partial z}{\partial t } - f(z, t, \theta) \Big) d t
$$

$$
\frac{d \mathcal{L}}{d \theta}=\int_{0}^{T} \nabla_{z} l^{T} \frac{\partial z}{\partial \theta}+\nabla_{\theta} l d t+\int_{0}^{T} \lambda^{T} \frac{\partial }{\partial \theta} \frac{\partial z}{\partial t} -\lambda^{T} \frac{\partial f}{\partial z} \frac{\partial z}{\partial \theta}-\lambda^{T} \frac{\partial f}{\partial \theta} d t
$$

$$
\text{Per partes:}\quad  \int_{0}^{T} \lambda^{T} \frac{\partial z}{\partial \theta} d t=\lambda_{T}^{T} \frac{\partial z_{T}}{\partial \theta}-\lambda_{0}^{T} \frac{\partial z_{0}}{\partial \theta}-\int_{0}^{T} \frac {\partial \lambda^{T}}{\partial t} \frac{\partial z_{t}}{\partial \theta} d t
$$

$$
\frac{d \mathcal{L}}{d \theta}=\int_{0}^{T} \nabla_{\theta} l d t+ \left(\nabla_{z} l-\frac {\partial \lambda^{T}}{\partial t}-\lambda^{T} \frac{\partial f}{\partial z}\right) \frac{\partial z}{\partial \theta} d t-\lambda^{T} \frac{\partial f}{\partial \theta} d t+\lambda_{T}^{T} \frac{\partial z_{T}}{\partial \theta}
$$

Define a new differential equation:
$$ \frac{\partial \lambda^T}{\partial t}  =\nabla_{z} l - \lambda^{T} \frac{\partial f}{\partial z}, \quad  \lambda_{T}^{T} =0 $$
\pause
The gradient then simplifies to:
$$
\nabla_{\theta} \mathcal{L}=\int_{0}^{T}\left(\nabla_{z} L^{T}-\lambda^{T}\right) \frac{\partial f}{\partial \theta} d t
$$
If we let $a(t) = \nabla_z L - \lambda(t)$, then we obtain the adjoint equation from before.

Putting it all together:

Algorithm:

Given: data $(x, y)$, NN dynamics $f_\theta(z(t), t)$, loss $L$
Forward pass: with initial condition $z(0)=x$, integrate:
$$ z(T) = z(0) + \int_0^T f_\theta(z(t), t) dt $$
Backward pass: with initial condition $a(T) = \partial L(z(T)) / \partial z$, integrate:
$$ a(0) = a(T) + \int_T^0 a(t) \frac{\partial f_\theta(z(t), t)}{\partial z} dt$$
Integrate:
$$ \frac{\partial L}{\partial \theta} = \int_0^T a(t) \frac{\partial f_\theta(z(t), t)}{\partial \theta} dt $$
Return: gradients w.r.t. input: $a(0)= \partial L(z(0)) / \partial z$, params: $\partial L / \partial \theta$.

Applications

Neural ODE instead of ResNet blocks


RK-net is backpropagating the gradient through a Runge-Kutta solver
		  Adjoint method memory cost independent of "depth"

Error control

Instead of \# of layers, specify accuracy of ODE (error tolerance)
Depth "$\approx$" \# of function evaluations
Constant memory cost


<p align="center"><img src="/assets/img/neuralodes/tradeoff.png" height="300" /></p>

Continuous change of variables

Let $z_1 = f(z_0)$ be a bijective transformation and $J_f$ be the Jacobian of $f$ (for scalar $z$, $J_f = \frac{d f}{d z}$). Then we have:
				\begin{align*}
					p(z_1) &= p(z_0) \Big| \det J_{f^{-1}} \Big| = p(z_0) \Big| \frac{1}{\det J_{f}} \Big| \\ 
					\log p(z_1) &= \log p(z_0) - \log \Big| \det J_{f} \Big|					
				\end{align*}		
		  Now let $\frac{d z}{d t} = f(z(t), t)$. Then the change in log probability is an ODE (under some regularity conditions):
			$$ \frac{\partial \log p(z(t))}{\partial t} = -\text{tr} \Big( \frac{d f}{d z(t)} \Big) $$		

Sketch of proof of the instantaneous version
Full proof in appendix A

Let $z(t + \epsilon) = T_\epsilon(z(t))$, $f$ Lipschitz and $z(t)$ bounded. 
		  We will need the following  results from Taylor expanding $T$:
					$$ T \approx z(t) + \epsilon \frac{dz}{dt} = z(t) + \epsilon f(z(t), t) $$ \pause
					$$  \frac{\partial T}{\partial z} = I + \epsilon \frac{\partial f(z(t), t)}{\partial z} + O(\epsilon^2) $$ \pause
					$$ \implies \lim_{\epsilon \to 0^+}  \frac{\partial T}{\partial z} = I, \quad  \lim_{\epsilon \to 0^+} \det \frac{\partial T}{\partial z}  = 1 $$ \pause
					$$ \lim_{\epsilon \to 0^+} \frac{\partial}{\partial \epsilon} \frac{\partial T}{\partial z}  = \frac{\partial f(z(t), t)}{\partial z} $$
		  
$$\boxed{\lim_{\epsilon \to 0^+} \det \frac{\partial T}{\partial z}  = 1, \quad lim_{\epsilon \to 0^+} \frac{\partial}{\partial \epsilon} \frac{\partial T}{\partial z}  = \frac{\partial f(z(t), t)}{\partial z}, \quad \frac{d}{d t} |A| = |A| tr \Big( A^{-1} \frac{d A}{d t} \Big)} 
	$$ \pause
 $$ \frac{\partial \log p(z(t))}{\partial t} = \lim_{\epsilon \to 0^+} \frac{1}{\epsilon} \Bigg[ \log p(z(t)) - \log \Big| \det \frac{\partial T}{\partial z} \Big| - \log p(z(t)) \Bigg] = $$ \pause
 $$ = -\lim_{\epsilon \to 0^+} \frac{1}{\epsilon} \Bigg[\log \Big| \det \frac{\partial T}{\partial z} \Big| \Bigg] =  -\lim_{\epsilon \to 0^+}  \frac{\partial }{\partial \epsilon} \log \Big| \det \frac{\partial T}{\partial z} \Big| = $$ \pause
$$ = -\lim_{\epsilon \to 0^+} \frac{\partial }{\partial \epsilon} \Bigg| \det \frac{\partial T}{\partial z}  \Bigg|  \cdot  \lim_{\epsilon \to 0^+}  \Bigg( \det \frac{\partial T}{\partial z}  \Bigg)^{-1}  = - \lim_{\epsilon \to 0^+} \frac{\partial }{\partial \epsilon} \Bigg| \det \frac{\partial T}{\partial z}  \Bigg|  = $$ \pause
$$ = - \lim_{\epsilon \to 0^+}  \Bigg |  \det \frac{\partial T}{\partial z}  \Bigg| \text{tr}\Bigg( \lim_{\epsilon \to 0^+}  \frac{\partial T^{-1}}{\partial z} \cdot   \lim_{\epsilon \to 0^+}  \frac{\partial }{\partial \epsilon} \frac{\partial T}{\partial z} \Bigg) = - \text{tr}\Big(  \frac{\partial f(z(t), t)}{\partial z} \Big)
$$


Normalizing flows[5]

Let $f$ be invertible, and $z' = f(z)$. Then:
$$
\begin{aligned} 
q\left(\mathbf{z}^{\prime}\right) &=q(\mathbf{z})\left|\operatorname{det} \frac{\partial f^{-1}}{\partial \mathbf{z}^{\prime}}\right|=q(\mathbf{z})\left|\operatorname{det} \frac{\partial f}{\partial \mathbf{z}}\right|^{-1} \\
\mathbf{z}_{K} &=f_{K} \circ \ldots \circ f_{2} \circ f_{1}\left(\mathbf{z}_{0}\right) \\ 
\ln q_{K}\left(\mathbf{z}_{K}\right) &=\ln q_{0}\left(\mathbf{z}_{0}\right)-\sum_{k=1}^{K} \ln \operatorname{det}\left|\frac{\partial f_{k}}{\partial \mathbf{z}_{k}}\right| 
\end{aligned}
$$

Example -- planar flow:
$$
	\begin{align*}
	z(t + 1) &= z(t) + u h(w^T z(t) + b) \\ 
	\Big| \det \frac{d z(t+1)}{d z} \Big| &=  \Big| \det(I + u \frac{\partial h}{\partial z}^T) \Big| = \Big| 1 + u^T \frac{\partial h}{\partial z} \Big| \\
	\log p(z(t + 1)) &= \log p(z(t)) - \log \Big| 1 + u^T \frac{\partial h}{\partial z} \Big| 
	\end{align*}
$$

Continuous normalizing flows
$$
	\begin{align*}
		\frac{d z(t)}{d t} &= uh(w^T z(t) + b) \\
		 \frac{\partial \log p(z(t))}{\partial t} &= - \text{tr}\Big( \frac{d f}{d z(t)} \Big) = - u^T \frac{\partial h}{\partial z(t)} \\
		\log p(z_T) &= \log p(z_0) - \int_0^T \text{tr} \Big( \frac{\partial h}{\partial z} \Big) dt 
	\end{align*}
$$

<p align="center"><img src="/assets/img/neuralodes/cnf.png" height="300" /></p>


Limitations

ODE becomes more demanding to compute over time
Can't control duration of training (NFE increase cannot be predicted)
Dimensionality of input/start and output/end must be the same
Toy data experiments
Continuity of vector fields limits possible flows

Augmented Neural ODEs


Motivation

NODE can fail for trivial functions

Let $g$ be any function such that $g(-1) = 1$ and $g(1) = -1$. Then NODE cannot represent it. 

<p align="center"><img src="/assets/img/neuralodes/intersect.png" height="300" /></p>

Limitations of NODE

Why is this the case? 
The trajectories intersect. The initial value problem means existing solutions are unique (under regularity conditions)
Interestingly, ResNets and the Euler method can solve it. Why?
Discretization errors allow for jumps.

NODE general failure class of functions

  Let's stack a linear layer on the output

Let $0 < r_1 < r_2 < r_3$, and $g(x)$ such that:
				$$
\left\{\begin{array}{ll}{g(\mathbf{x})=-1} & {\text { if }\|\mathbf{x}\| \leq r_{1}} \\ {g(\mathbf{x})=1} & {\text { if } r_{2} \leq\|\mathbf{x}\| \leq r_{3}}\end{array}\right.
				$$
				  Then for any dimension $d$, NODE cannot represent $g(x)$. Why?
				  NODE is a homeomorphism (preserves topology)


<p align="center"><img src="/assets/img/neuralodes/arch.png" height="300" /></p>
<p align="center"><img src="/assets/img/neuralodes/annulus.png" height="300" /></p>


NODE in practice

<p align="center"><img src="/assets/img/neuralodes/harder.png" height="300" /></p>



Augmented neural ODEs[6]

Should we discard NODEs? No
Can we avoid intersections and achieve linear separability? Yes
How? Augment the space with extra dimensions
Concatenate $a \in \mathbb{R}^p$ to $h \in \mathbb{R}^d$ with initial condition $a(0) = 0$ and solve the $\mathbb{R}^{d + p}$ ODE:

$$ \frac{\mathrm{d}}{\mathrm{d} t}\left[\begin{array}{l}{\mathbf{h}(t)} \\ {\mathbf{a}(t)}\end{array}\right]=\mathbf{f}\left(\left[\begin{array}{l}{\mathbf{h}(t)} \\ {\mathbf{a}(t)}\end{array}\right], t\right), \quad\left[\begin{array}{l}{\mathbf{h}(0)} \\ {\mathbf{a}(0)}\end{array}\right]=\left[\begin{array}{l}{\mathbf{x}} \\ {\mathbf{0}}\end{array}\right]
$$

Flow complexity and NFE over time

 Extra dimensions allow for simpler flows, which means a smaller number of function evaluations (NFEs):

<p align="center"><img src="/assets/img/neuralodes/flowtime.png" height="300" /></p>


In conclusion..
Neural ODE provide a new way to adaptively train "deep", "equal width" ResNets
Training allows setting error tolerances
 Can be made powerful by augmenting the dimensionality
 However, even ANODE cannot model uncertainty


---
[1] Kurt Hornik, Maxwell Stinchcombe, and Halbert White. <em>Multilayer feedforward networks are universal approximators</em>. In: Neural networks 2.5 (1989), pp. 359–366.
[2] Zhou Lu et al. <em>The expressive power of neural networks: A view from the width</em>. In: Advances in neural information processing systems. 2017, pp. 6231–6239.
[3] 3Kaiming He et al. <em>Deep residual learning for image recognition</em>. In: Proceedings of the IEEE conference on computer vision and pattern recognition. 2016, pp. 770–778.
[4] Tian Qi Chen et al. <em>Neural ordinary differential equations</em>. In: Advances in neural information processing systems. 2018, pp. 6571–6583.
[5]Danilo Jimenez Rezende and Shakir Mohamed. <em>Variational inference wit h normalizing flows</em>. In: arXiv preprint arXiv:1505.05770 (2015)
[6] Emilien Dupont, Arnaud Doucet, and Yee Whye Teh. <em>Augmented neural odes</em>. In: arXiv preprint arXiv:1904.01681 (2019).