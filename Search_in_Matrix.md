
# Question:
> Given a matrix of m * n integers, each row and column is in non-decreasing order. To find a target key in the matrix.
For example: In {{ 1,  5,  8, 13}, { 9, 13, 15, 17}, {10, 18, 19, 20}, {11, 21, 22, 23}}, the location of 13 is either [0, 3] or [1, 1].

# Analysis
The most trival approach will be scan the matrix row by row and column by column, the complexity will be O(mn);

Since it's ordered in row and column, you may link that to "Binary search" immediately, (though this would be not so efficient even compared as trival solutioin, but it's a great exercise to binary search).
Similarly, we can devide the matrix by the middle element as below

|Matrix| | | 
|:-:|:-:|:-:|
| __upperLeft__ |   __up(1D)__    | __upperRight__ |  
| __left(1D)__  | m[mid][mid] | right(1D)  |
| __lowerLeft__ |   down(1D)  | lowerRight |

|Matrix| | |
|:-:|:-:|:-:|
| upperLeft |   up(1D)    | __upperRight__ |  
| left(1D)  | m[mid][mid] | __right(1D)__  |
| __lowerLeft__ |   __down(1D)__  | __lowerRight__ |
     


