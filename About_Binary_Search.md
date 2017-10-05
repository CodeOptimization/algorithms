## Basic Binary Search

```java
    public int binarySearch1(int[] nums, int target){
        int lo = 0, hi = nums.length - 1, mid = 0;
        while(lo <= hi){
     //'<= is' trying to include the nums[lo] == nums[hi] == target case; but you need to update index: 
     //increasing or decreasing within each while-loop.
            mid = (lo + hi) / 2;
            if(nums[mid] < target)
                lo = mid + 1;
            else if(nums[mid] > target)
                hi = mid - 1;
            else
                return mid;
        }
        return -1;
    }
    
    public int binarySearch2(int[] nums, int target){
        int lo = 0, hi = nums.length - 1, mid = 0;
        while(true){
            if(lo > hi)
                return -1;
            mid = (lo + hi) / 2;
            if(nums[mid] < target)
                lo = mid + 1;
            else if(nums[mid] > target)
                hi = mid - 1;
            else
                return mid;
        }
    }
```

## Return Closest
You need to return the first occurence of the element, otherwise -1.

```java
  
```


## With Duplicates


```java
    public int binarySearchFirstOccurence1(int[] nums, int target){
        int res = -1, lo = 0, hi = nums.length - 1;
        while(lo <= hi){
            int mid = (lo + hi) / 2;
//             System.out.println("lo = " + lo + ", mid = " + mid + ", hi = " + hi);
            if(nums[mid] < target){
                lo = mid + 1;
            }else if(nums[mid] == target){
                res = mid;
                hi = mid - 1;
            }else{
                hi = mid - 1;
            }
        }
        return res;
    }
    
    public int binarySearchFirstOccurence2(int[] nums, int target){
        int lo = -1, hi = nums.length;
        while(lo + 1 != hi){
            int m = (lo + hi) / 2;
            if(nums[m] < target)
                lo = m;
            else
                hi = m;
        }
        if(hi >= nums.length || nums[hi] != target)        
            return -1;
        else
            return hi;
    }
    
    public int binarySearchLastOccurence1(int[] nums, int target){
        int res = -1, lo = 0, hi = nums.length - 1;
        while(lo <= hi){
            int mid = (lo + hi) / 2;
//             System.out.println("lo = " + lo + ", mid = " + mid + ", hi = " + hi);
            if(nums[mid] < target){
                lo = mid + 1;
            }else if(nums[mid] == target){
                res = mid;
                lo = mid + 1;
            }else{
                hi = mid - 1;
            }
        }
        return res;
    }

```
