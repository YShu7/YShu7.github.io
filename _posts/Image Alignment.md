#### Image Alignment

1. Template matching 

   Slow, combinatory, global solution 

2. Multi-scale template matching 

   Faster, combinatory, locally optimal 

3. Localrefinementbased on some initial guess 

   Fastest, locally optimal 

Translation: $\begin{bmatrix}x+p_1 \\ y + p_2\end{bmatrix} = \begin{bmatrix}1 & 0 & p_1 \\ 0 & 1 & p_2\end{bmatrix} * \begin{bmatrix}x \\ y \\ 1\end{bmatrix}$

Affine:  $\begin{bmatrix}p_1x+p_2y+p_3 \\ p_4x+p_5y+p_6\end{bmatrix} = \begin{bmatrix}p_1 & p_2 & p_3 \\ p_4 & p_5 & p_6\end{bmatrix} * \begin{bmatrix}x \\ y \\ 1\end{bmatrix}$

#### Lucas-Kanade Alignment