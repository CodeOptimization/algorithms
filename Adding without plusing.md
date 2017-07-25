# Question
How to add two numbers without using "+"?

# Analysing
## Algorithm
__Firstly__,  we need to know how the numbers are represented in the computer. We use 2's complement to represent numbers in computer system.

__Secondly__, let's check how the add operation is performed:

    1. 0 + 0 ->  0
    2. 0 + 1 ->  1
    3. 1 + 0 ->  1
    4. 1 + 1 -> 11
Comparing to ^ operation:

    1. 0 ^ 0 ->  0
    2. 0 ^ 1 ->  1
    3. 1 ^ 0 ->  1
    4. 1 ^ 1 ->  1
Then we can start trying to use '^' to represent the add operation. The lower digits in '+' and '^' operation are the same, except when 
the two operands are both equals to 1.

__Thirdly__, we can use '&' to find out whether two digits are both ones(the 4th case above).

## Example 1

    7 + 18 = 25;
    -> 0000 0111 + 0001 0010 = 0001 1001  
              
            Xor                Carry
              0000 0111          0000 0111
            ^ 0001 0010        & 0001 0010
            ------------       -----------
            = 0001 0101        = 0000 0010
                               << 1 (because we are calculating carry, we need to move one left digit)
                               -----------
                               = 0000 0100
            
              0000 0100          0000 0100
            ^ 0001 0101        & 0001 0101
            -----------        -----------
            = 0001 0001         = 0000 0100
                               << 1
                               -----------
                               0000 1000
              
              0000 1000          0000 1000
            ^ 0001 0001        & 0001 0001
            -----------        -----------
            = 0001 1001        = 0000 0000
           (No more both 1 in same digit, no more carrys. Then we have the final answer: 0001 1001)
        
## Example 2

    7 - 18 = 7 + (-18) = -11;
    7 -> 0000 0111 
    18's sign-and-magnitude:  0001 0010 
        ->  ones' complement: 1110 1101
        ->  two's complement: 1110 1110
    -18 -> 1110 1110
    7 - 18 = 0000 0111 + 1110 1110  = 1111 0101 
        ->  ones' complement: 0000 1010 + 1 
        ->  two's complement: 0000 1011 -> -11 
    
           Xor                Carry
             0000 0111          0000 0111
           ^ 1110 1110        & 1110 1110
            -----------       -----------
           = 1110 1001        = 0000 0110
                              << 1
                              -----------
                              = 0000 1100
                                
             1110 1001          1110 1001
           ^ 0000 1100        & 0000 1100
           -----------        -----------
           = 1110 0101        = 0000 1000
                              << 1
                              -----------
                              = 0001 0000
                                
             1110 0101          1110 0101
           ^ 0001 0000        & 0001 0000
           -----------        -----------
           = 1111 0101        = 0000 0000
           (No carry. The End.)
    
    
# Coding

```java
//java code
public int addByBits(int a, int b){
    int carry = a & b;
    int xor = a ^ b;
    while(carry != 0){
        int tempXor = xor;
        int tempCar = carry << 1;
        carry = tempXor & tempCar;
        xor = tempXor ^ tempCar; 
    }
    return xor;
}

```

