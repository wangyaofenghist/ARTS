leetcode:Minimum-depth-of-binary-tree

## 题目描述

Given a binary tree, find its minimum depth.The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.

给出一个二叉树，从中找到最短路径上的节点数

分析：

1.一个二叉树 如何 找到最短路径

一般涉及到二叉树，很多算法都考虑递归，首先思考一下如何遍历一棵二叉树，前序、中序、后续都是常见的技巧，如果从这个开始改进，又改如何做

## 考察点

树、递归、层级遍历

##代码

1.递归算法

```
int minTree(TreeNode *root ,int num){
        if(root == NULL){
            return num;
        }
        //当左右子树都存在的时候 应该 取min 如果只有一个取 max，因为题目的最短路径上的节点数，路径一定得是一个完整路径，而不是一段
        if(root->left && root->right){
            return min(minTree(root->left,num+1),minTree(root->right,num+1));
        }else{
            return max(minTree(root->left,num+1),minTree(root->right,num+1));
        }
        
    }
```

2.递归算法

```
/**
    *root 节点，hasbrother 是否 有 兄弟节点
    *递归思想，深度便利，每次取出
    */
    int minDSF(TreeNode * root,bool hasbrother){
        if(root == NULL){
            //因为是取最小生成树 所以 有兄弟节点时候 返回 INT_MAX 会被舍弃，知道叶子节点
            return hasbrother ?INT_MAX : 0;
        }
        return 1+min(runDSF(root->left,root->right!=NULL),runDSF(root->right,root->left!=NULL));
        
    }
```

3.广度优先，层级遍历

```
//第二种思路 广度优先，找到第一个 叶子节点
    int minBSF(TreeNode * root){
        if(root==NULL) return 0;
        queue <TreeNode* > qu;
        TreeNode* last,*now;
        last = now = root;
        qu.push(root);
        int level =1,size;
        while(qu.size()){
            //最早被压入队列的元素
            now = qu.front();
            qu.pop();
            size = qu.size();
            if(now->left) qu.push(now->left);
            if(now->right) qu.push(now->right);
            //说明没有新的数据入队 已经走到了叶子节点 可以退出了 此种情况找到的肯定为最小深度
            if(qu.size()-size==0) break;
            //last == now 说明走完了一层
            if(last==now){
                level ++;
                //qu.back() 最后被压入队列的元素
                if(qu.size()) last = qu.back();
            }
        }
        return level;
    }
```



