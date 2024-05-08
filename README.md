# SPAxb
This project shows how to develop and solve sparse Ax=b system. PETSc is utilised for the iterative solution.
Each row of matrix A has non-zero entires of the form (1,1,4,1,1) that is typical of a matrix obtained from descritized Laplacian operator on an orthogonal two dimensional mesh using conventional numerical techniques such as finite difference or finite volume method. For structured (IJK ordering) mesh, the discretization yields a banded matrix. For this project, the mesh cells are anti-clockwise ordered as given below.

  _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ 
 |           |           |           |           |     
 |    (15)   |    (14)   |    (13)   |    (12)   |
 |           |           |           |           |
 |_ _ _ _ _ _|_ _ _ _ _ _| _ _ _ _ _ | _ _ _ _ _ |
 |           |           |           |           |
 |    (8)    |    (7)    |    (6)    |    (11)   |
 |           |           |           |           |
 |_ _ _ _ _ _|_ _ _ _ _ _|_ _ _ _ _ _| _ _ _ _ _ | 
 |           |           |           |           |
 |    (3)    |    (2)    |    (5)    |    (10)   |
 |           |           |           |           |
 |_ _ _ _ _ _|_ _ _ _ _ _|_ _ _ _ _ _| _ _ _ _ _ |   
 |           |           |           |           |
 |    (0)    |    (1)    |    (4)    |    (9)    |
 |           |           |           |           |
 |_ _ _ _ _ _|_ _ _ _ _ _|_ _ _ _ _ _| _ _ _ _ _ | 

 
The resulting matrix size is 16x16. The above cell ordering results in a sparse matrix with increasing bandwidth with cell number. Each cell corresponds to a matrix entry typical to that of finite volume discretization.

A grid file is provided "grid.out" that shows the cell-cell connectivity. For interior cells (2,5,6,7) there are 4 neighbours, hence the number of non-zero entries are 5 including the parent cell number. For boundary cells (0,1,3,4,8,9,10,11,12,13,14,15) the number of neighbouring cells are less than 4, hence the non-zero entries in the corresponding rows are less than 5. In the grid file "grid.out", the corresponding entries of the boundary cells where neighbours are absent are labelled '-1'.

After reading the cell connectivity data from the grid file the matrix entries are first stored in the CO-Ordinate (COO) format with entires stored in column-index (COL), row-index (ROW) and matrix values (VAL) arrays. The COO format is converted to Compressed Sparse Row (CSR) format with ROW entiries converted to row-pointer (ROW_ptr) array of size (n+1), where n is the matrix size. 
