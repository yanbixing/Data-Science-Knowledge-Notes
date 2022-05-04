# Linear Algebra


<!-- <div  align="center">
  <img src=https://upload.wikimedia.org/wikipedia/commons/b/bf/Matris.png style = "zoom:30%">
</div> -->


## Understanding on matrix multiplication

$$\vec{u}_{[m]} = W_{mn}\vec{v}_{[n]}$$

- Denote:
    $$\vec{u}_{[m]} := [u_1... u_m]^T$$
    $$\vec{v}_{[n]} := [v_1... v_n]^T$$

- A $m \times n$ weight $W_{mn}$:
    $$W_{mn} = \begin{bmatrix}w_{11}&...&w_{1n}\\...&w_{ij}&...\\w_{m1}&...&w_{mn}\end{bmatrix} = \begin{bmatrix}w_1^T\\...\\w_m^T\end{bmatrix}$$

- Then:
    $$\vec{v}_{[n]} := [w^T_1\vec{v}_{[n]}... w^T_m\vec{v}_{[n]}]^T$$
- I.e.
  - We map (also scale) a current $n$-$\dim$ vector $\vec{v}_{[n]}$ to $m$ different new directions $w^T_i$, and use this project values as the coordinate values of the new $m$-$\dim$ vector $\vec{u}_{[m]}$.