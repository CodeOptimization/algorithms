In this profile we are trying to solve the Lowest Common Ancestor(LCA) problem. We first define the class as below and later we will 
demostrate the algorithms on 
  * Binary Search Tree
  * Binary Tree  
  * Regular Trees

```java
class BinaryTreeNode {
    int val;
    BinaryTreeNode left;
    BinaryTreeNode right;
    BinaryTreeNode(int x) { 
        val = x; 
    }
}

class TreeNode{
    int val;
    List<TreeNode> children = new ArrayList<TreeNode>();
    TreeNode(int x) { 
        val = x; 
    }
    
    public String toString(){
        return "[TreeNode: " + val + "]";
    }
}
```

### Binary Search Tree
```java
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root == null)
            return null;
        if(p.val < root.val && q.val < root.val)
            return lowestCommonAncestor(root.left, p, q);
        else if(p.val > root.val && q.val > root.val)
            return lowestCommonAncestor(root.right, p, q);
        else
            return root;
    } 

```


### Binary Tree
```java
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root == null)
            return null;
        if(root == p || root == q)
            return root;
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        if(left != null && right != null)
            return root;
        else if(left != null)
            return left;
        else if(right != null)
            return right;
        else 
            return null;
    }

```

### Regular Trees

```java
    public static TreeNode getLCA(TreeNode root, TreeNode p, TreeNode q){
        if(root == null)
            return null;
        if(root == p || root == q)
            return root;
        List<TreeNode> list = new ArrayList<TreeNode>();
        for(TreeNode node: root.children)
            list.add(getLCA(node, p, q));
        int count = 0;
        for(TreeNode node: list){
            if(node != null)
                count++;
        }
        //2 is the number of nodes to looking for ancestors
        if(count >= 2){
            return root;
        }else{
            for(TreeNode node: list)
                if(node != null)
                    return node;
        }
        return null;
    }
    
    public static TreeNode getLCA(TreeNode root, TreeNode p, TreeNode q, TreeNode r){
        if(root == null)
            return null;
        if(root == p || root == q || root == r)
            return root;
        List<TreeNode> list = new ArrayList<TreeNode>();
        for(TreeNode node: root.children)
            list.add(getLCA(node, p, q, r));
        int count = 0;
        for(TreeNode node: list){
            if(node != null)
                count++;
        }
        //2 is the number of nodes to looking for ancestors
        if(count >= 3){
            return root;
        }else{
            for(TreeNode node: list)
                if(node != null)
                    return node;
        }
        return null;
    }
   ```
