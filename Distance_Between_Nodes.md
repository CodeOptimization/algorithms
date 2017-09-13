# Find distance between two given keys of a Binary Tree

Find the distance between two keys in a binary tree, no parent pointers are given. Distance between two nodes is the minimum number of 
edges to be traversed to reach one node from other.

### Dist(n1, n2) = Dist(root, n1) + Dist(root, n2) - 2*Dist(root, lca) 

  * 'n1' and 'n2' are the two given keys
  * 'root' is root of given Binary Tree.
  * 'lca' is lowest common ancestor of n1 and n2
  * Dist(n1, n2) is the distance between n1 and n2.




#### Reference
  * http://www.geeksforgeeks.org/find-distance-two-given-nodes/
