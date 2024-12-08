Let's define $T=\begin{bmatrix} T_0 & T_1 & \dots & Tn \end{bmatrix}^T$ as the vector of time intervals for the piece-wise polynomial $f(t)$.
$$
f(t) = \begin{cases} 
\begin{bmatrix} x_1(t) & y_1(t) & z_1(t) & \psi_1(t) \end{bmatrix}^T  \text{ for } t \in [0,  T_1] \\
\begin{bmatrix} x_2(t) & y_2(t) & z_2(t) & \psi_2(t) \end{bmatrix}^T  \text{ for } t \in [T_1,  T_2] \\
\vdots \\
\begin{bmatrix} x_N(t) & y_N(t) & z_N(t) & \psi_N(t) \end{bmatrix}^T  \text{ for } t \in [T_{N-1},  T_N] \\
\end{cases}
$$

Let's state the minimization problem of choosing the individual elements of T to get as fast as possible to the goal while satisfying the equality constraints (passing through waypoints) and satisfying the inequality constraints (not going over maximum velocity and acceleration).

$$
\begin{gather}
\min_{T} \quad T^TT\\\\


g_i(t) \leq 0, \quad i = 1, ..., m \\
h_j(T) = 0, \quad j = 1, ..., p
\end{gather}
$$
Where:

- $T$ is the vector of variables to be determined,
- $T^TT$ is the objective function to be minimized,
- $g_i(x)$ are inequality constraints (distance to the reference trajectory, velocity, acceleration)
- $h_j(T)$ are equality constraints (waypoints and initial and final position, velocity, acceleration).


$$
h_j(T) = \begin{bmatrix}
	f^{(j)}(T_0) - X_0^{(j)} \\
	f^{(j)}(T_1) - X_1^{(j)} \\
			\vdots \\	
	f^{(j)}(T_n) - X_n^{(j)} \\
\end{bmatrix}
$$
Where:

- $X=\begin{bmatrix} X_0 & X_1 & \dots & X_n \end{bmatrix}^T$ is the vector of constraints (0-th derivative) associated to T
- $X^{(k)}=\begin{bmatrix} X_0^{(k)} & X_1^{(k)} & \dots & X_n^{(k)} \end{bmatrix}^T$ is the vector of constraints (k-th derivative) associated to T

$$
g_i(t) = |f^{(i)}(t) - Y^{(i)}|
$$
> **NOTE**: I'm referring to the norm of the function

Where:

- $Y= \begin{bmatrix} x & y & z & \psi \end{bmatrix}^T$ is the vector of constraints (0-th derivative)
- $Y^{(k)} = \begin{bmatrix} x^{(k)} & y^{(k)} & z^{(k)} & \psi^{(k)} \end{bmatrix}^T$ is the vector of constraints (k-th derivative)

