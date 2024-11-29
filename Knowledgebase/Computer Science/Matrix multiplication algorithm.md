```
1 if columns [A]  rows [B]

2    then error "incompatible dimensions"

3    else for i 1 to rows [A]

4             do for j 1 to columns[B]

5                    do C[i, j]  0

6                       for k  1 to columns [A]

7                           do C[i ,j]  C[i ,j] + A [i, k] B[k, j]

8         return C

```
   
