[TOC]

# Binary Tree

### 常用接口

```c++
std::max(1, 2);
std::max({1, 2, 3, 4});
std::pair<int, int> minmax = std::minmax({1, 2, 3, 4});
std::vector<int>::iterator result = std::max_element(v.begin(), v.end());
reverse(v.begin(), v.end()) // 倒序
sort(a.begin(),a.end(),less<int>()); // 降序排列
std::swap(a, b);
double pow(double base, double exponent) // 指数
double sqrt(double x) // 开根号
```





## Traversal





#### Morris traversal ---o(n) 时间o(1)空间遍历

+ 常规递归或非递归方式遍历二叉树，总是需要记录路径（与树高度有关），因此无法实现o(1)空间复杂度



#### 层次遍历

```c++
#define DO_IF(exp, do) {if ((exp)) { (do;) }}
void traversal(TreeNode* root) {
    queue<TreeNode*> qu;
    qu.push(root); // root非空

    while (!qu.empty()) {
        int size = qu.size(); // 获取当前层的元素个数
        while(size--) {
            auto node = qu.front();
            qu.pop();
            
            // do something to node
            
            DO_IF(node->left, qu.push(node->left));
            DO_IF(node->right, qu.push(node->right));
        }
    }
}

```

#### 二叉树两个节点的最近公共祖先

思路

+  搜索每个节点时，返回这节点为根节点的子树中是否有 两个目标节点，有则返回，无则返回NULL
+ 由于是从子节点向上搜索，因此采用后序遍历

改进题：

+ 二叉搜索树， 从根节点出发，根节点大小在两节点之间时候即为最近公共祖先，否则向左子节点或右子节点移动
+ 频繁查询， dfs前序遍历所有节点，每个节点都保存自己的父节点和深度。 类似两个链表求公共节点一样，深度大的节点点先走dp1-dp2步，再找到值相同的点。

```c++
TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
    if (root == NULL || root == p ||  root == q) {
        return root;
    }

    auto left = lowestCommonAncestor(root->left, p, q);
    auto right = lowestCommonAncestor(root->right, p, q);
    if (left != NULL && right != NULL) {
        return root;
    }

    return left != NULL ? left : right;
}
```



### 二叉搜索树

二叉搜索树是一种二叉树，且树中每个节点均满足下述属性：

- 任意节点的左子树中的值都 **严格小于** 此节点的值。
- 任意节点的右子树中的值都 **严格大于** 此节点的值。

**因此二叉搜索树使用中序搜索，将得到严格递增的数组**

```c++
    int last = 0;
    int treeSize = 0;
    bool checkBinarySearch(TreeNode* root) {
        if (!root) {
            return true;
        }
        
        if (!checkBinarySearch(root->left)) {
            return false;
        }
        if (root->val <= last) {
            return false;
        }
        last = root->val;
        treeSize++;
        if (!checkBinarySearch(root->right)) {
            return false;
        }
        return true;
    }
```

