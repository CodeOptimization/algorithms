
# Question:
> Given a matrix of m * n integers, each row and column is in non-decreasing order. To find a target key in the matrix.
For example: Matrix as below(basically it's a two dimensional array):

|Ma|-tr|-ix|:| 
|:-:|:-:|:-:|:-:|
|1|5|8|13|
|9|13|15|17|
|10|18|19|20|
|11|21|22|23|

the location of 13 is either [0, 3] or [1, 1].

# Analysis
Let's say the size of matrix is n;

1. The most trivial approach will be scan the matrix row by row and column by column, the complexity will be O(n);

2. Since it's ordered in row and column, you may link that to "Binary search" immediately. As a result of this, we can divide the matrix by the middle element as below

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

The code as below:
```java
     public int[] binarySearchInArray(int[][] matrix, int target){
	    return binarySearchInArray1(matrix, target, 0, matrix.length - 1, 0, matrix[0].length - 1);
	}
     
     private int[] binarySearchInArray1(int[][] matrix, int target, int m1, int m2, int n1, int n2) {
        if (m1 > m2 || n1 > n2)
            return null;

        int rowMid = (m1 + m2) / 2, colMid = (n1 + n2) / 2;
        if (matrix[rowMid][colMid] > target) {
            int[] upperLeft = binarySearchInArray1(matrix, target, m1, rowMid - 1, n1, colMid - 1);
            if (upperLeft != null)
                return upperLeft;
            int[] lowerLeft = binarySearchInArray1(matrix, target, rowMid + 1, m2, n1, colMid - 1);
            if (lowerLeft != null)
                return lowerLeft;
            int[] upperRight = binarySearchInArray1(matrix, target, m1, rowMid - 1, colMid + 1, n2);
            if (upperRight != null)
                return upperRight;
            int[] left = oneDRowSearchInMatrix(matrix, target, rowMid, n1, colMid - 1);
            if (left != null)
                return left;
            int[] up = oneDColSearchInMatrix(matrix, target, m1, rowMid - 1, colMid);
            if (up != null)
                return up;
        } else if (matrix[rowMid][colMid] < target) {
            int[] upperRight = binarySearchInArray1(matrix, target, m1, rowMid - 1, colMid + 1, n2);
            if (upperRight != null)
                return upperRight;
            int[] lowerLeft = binarySearchInArray1(matrix, target, rowMid + 1, m2, n1, colMid - 1);
            if (lowerLeft != null)
                return lowerLeft;
            int[] lowerRight = binarySearchInArray1(matrix, target, rowMid + 1, m2, colMid + 1, n2);
            if (lowerRight != null)
                return lowerRight;
            int[] right = oneDRowSearchInMatrix(matrix, target, rowMid, colMid + 1, n2);
            if (right != null)
                return right;
            int[] down = oneDColSearchInMatrix(matrix, target, rowMid + 1, m2, colMid);
            if (down != null)
                return down;
        } else {
            return new int[]{rowMid, colMid};
        }
        return null;
    }
    
    private int[] oneDRowSearchInMatrix(int[][] matrix, int target, int row, int n1, int n2) {
        int lo = n1, hi = n2;
        while (lo <= hi) {
            int mid = (lo + hi) / 2;
            if (matrix[row][mid] > target)
                hi = mid - 1;
            else if (matrix[row][mid] < target)
                lo = mid + 1;
            else
                return new int[]{row, mid};
        }
        return null;
    }

    private int[] oneDColSearchInMatrix(int[][] matrix, int target, int m1, int m2, int col) {
        int lo = m1, hi = m2;
        while (lo <= hi) {
            int mid = (lo + hi) / 2;
            if (matrix[mid][col] > target)
                hi = mid - 1;
            else if (matrix[mid][col] < target)
                lo = mid + 1;
            else
                return new int[]{mid, col};
        }
        return null;
    }
    
```
### Can we do better?
Obviously, the previous algorithms is too tedious， because we can just search in two parts: if(key < m[mid][mid]) then search in  {[_upperLeft, up(1D), upperRight_] and [_left(1D), lowerleft_]}; else if(key > m[mid][mid]) then search in{[_lowerLeft, down(1D), lowerRight_] and [_upperRight, right(1D)_]} and the last case will be key == m[mid][mid] you just return the indices; 
```java
    public int[] binarySearchInArray2(int[][] matrix, int key, int r1, int r2, int c1, int c2) {
        if (r1 > r2 || c1 > c2)
            return null;

        int rMid = (r1 + r2) / 2, cMid = (c1 + c2) / 2;
        if (matrix[rMid][cMid] > key) {
            //search in the top and left
            int[] upper = binarySearchInArray2(matrix, key, r1, rMid - 1, c1, c2);
            if (upper != null)
                return upper;
            int[] left = binarySearchInArray2(matrix, key, rMid, r2, c1, cMid - 1);
            if (left != null)
                return left;
        } else if (matrix[rMid][cMid] < key) {
            //search in the lower part and right
            int[] lower = binarySearchInArray2(matrix, key, rMid + 1, r2, c1, c2);
            if (lower != null)
                return lower;
            int[] right = binarySearchInArray2(matrix, key, r1, rMid, cMid + 1, c2);
            if (right != null)
                return right;
        } else {
            return new int[]{rMid, cMid};
        }
        return null;
    }

```
#### Time complexity: 
Given the input size as _n_, _T(n) = T(n/2) + T(n/4) + O(1)_; According to __Akra Bazzi Theorem__ we have:

_T(n) = Theta(n^log_2(Phi))_, which is _log_2(Phi)_ roughly equals 0.69424191363... and the proof is [here](https://github.com/CodeOptimization/algorithms/blob/master/Akra_Bazzi_Theorem.md).

### Can we do even better?

3. Linear time search O(m + n):

And the answer is Yes, we can do better even with out binary search. Searching from right top corner, when you encounter a element that is 
smaller than _key_, go down(next row), while a larger element, go left(column of left size) and last case is equal which means you find it. Either you will find the element and stop, or you will run out of rows and columns that means no such an element.

```java
public int[] linearSearch(int[][] matrix, int key){
	    if(matrix == null || matrix.length == 0 || matrix[0].length == 0)
	        return null;
	    int i = 0, j = matrix[0].length - 1;
	    while(i < matrix.length && j >= 0){
	        if(matrix[i][j] > key)
	            j--;
	        else if(matrix[i][j] < key)
	            i++;
	        else
	            return new int[]{i, j};
	    }
	    return null;
	} 

```

# Question
What if we add one more condition: 

_The first integer of each row is greater than the last integer of the previous row._

# Solution
You can just treat this problem as a one dimensional array. Code as below:

```java
    public boolean searchMatrix(int[][] matrix, int target) {
        if(matrix == null || matrix.length == 0 ||  matrix[0].length == 0)
            return false;
        int m = matrix.length, n = matrix[0].length;
        int lo = 0, hi = m * n - 1;
        while(lo <= hi){
            int mid = (lo + hi) / 2;
            int row = mid / n, col = mid % n;
            if(target < matrix[row][col])
                hi = mid - 1;
            else if(target > matrix[row][col])
                lo = mid + 1;
            else
                return true;
        }
        return false;
    }
```
What about the time complexity?  O(log(m) + log(n)), or in other words, O(log(mn)).


#### Reference

  * https://en.wikipedia.org/wiki/Akra%E2%80%93Bazzi_method
  


