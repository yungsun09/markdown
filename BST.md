今天来学二叉搜索树啊,先上定义 

>二叉搜索树（又：二叉查找树，二叉排序树，Binary Search Tree，BST）是一种二叉树，其中每个结点最多有两个子结点且具有二叉搜索树性质：左子树上所有结点的值均小于它的根结点的值以及右子树上所有结点的值均大于它的根结点的值 

  

看了这个之后是不是觉得又晦涩又难懂还屁用没有？ 

  

但其实...还是有的... 

  

- 在我随意的往里放数，让你实时告诉我第n大（或小）的数是什么，怎么办? 

- 一个有序通讯录如果我想往里面塞条新联系人，让你实时更新怎么办? 

  

如果一个不会BST的人 

绝对是有序数组/有序链表上来就刚的，能不能告诉我有序数组的插入大O是多少?那有序链表的查找呢? 

  

所以，这个时候就体现出来BST的作用啦，BST的特点之一就是中序遍历出来就是有序的，且插入查找删除的速度都比较快O(logn) 

  

就冲这个，不学你好意思吗？你怎么睡得着的？ 

  

  

那咱们就开始学习建BST树 

  

## 创建节点 

  

建树以一步，建节点！（当然这里只是最基础形态的节点，定制化开发请联系我来做 QQ：1234xxx） 

  

在这里我们定义每一个节点都应该有两个子节点，分别是左节点和右节点（废话），且每个节点还得能存值，所以它长这个样子 

```javascript 

function createNode(val, left, right) { 

  const node = {}; 

  node.val = val === undefined ? 0 : val; 

  node.left = left === undefined ? null : left; 

  node.right = right === undefined ? null : right; 

  return node; 

} 

```

好！！！节点完事了，那树怎么建呢？我们来回顾一下二叉树的性质： 

  

请自己往上翻，翻到“**左子树上所有结点的值均小于它的根结点的值以及右子树上所有结点的值均大于它的根结点的值**”这个地方，大声读一遍再翻下来，谢谢 

  

我默认你们都是好孩子奥，都读完了奥，那我们接着往下看⬇️ 

  

首先我们需要一个根结点，随便来一个数“5”，那这个节点应该长什么样呢？ 

  

```javascript 

Node { 

  val: 5, 

  left: null, 

  right: null 

} 

```

  

由于咱们只有这一个节点，所以我叫他根节点不过分吧？ 

  

那么问题开始，如果我想往里边加一个数“2”，我应该加在哪里 

  

![image.png](https://s2.loli.net/2022/07/26/1EhMku4Sfalsq5R.png) 

  

我放左边不过分吧？为什么啊？2是不是比5小？**左子树上所有结点的值均小于它的根结点的值以及右子树上所有结点的值均大于它的根结点的值**对不对？ 

  

好那咱们继续，这回我放个9，应该在哪快想快想 

  

![image.png](https://s2.loli.net/2022/07/26/lfmKakr1AvDGW6V.png) 

  

没毛病吧？ 

  

问题来了哈，我想放个7，放在哪？咱们来一起琢磨一下 

  

7比5大吗？ 大，好看右节点 

7比9大吗？ 不，那看9的左节点 

没有左节点？ 那我就塞这了 

  

![](https://s2.loli.net/2022/07/26/jUNixwOH5PeLIBC.png) 

  

好，嗯句上方的逻辑，咱们其实就可以写出来一个BST的插入功能了 

  

## 插入 

  

```javascript 

// 插入节点 

const insertNode = (node, val) => { 

  if (val <= node.val) { 

    if (node.left === null) { 

      node.left = createNode(val); 

    } else { 

      insertNode(node.left, newNode); 

    } 

  } else { 

    if (node.right === null) { 

      node.right = createNode(val);; 

    } else { 

      insertNode(node.right, newNode); 

    } 

  } 

}; 

```

  

依此类推，如果我们想找到一个数应该怎么做呢 

  

## 搜索 

  

```javascript 

// 搜索节点 

const searchNode = (node, val) => { 

    if (node === null) { 

        return false 

    } 

    if (val < node.val) { 

        return searchNode(node.left, val) 

    }else if (val > node.val) { 

        return searchNode(node.right, val) 

    }else { 

        return true 

    } 

} 

```

## 获取极值 

```javascript 

// 获取最大值 

const findMin = node => { 

    if (node) { 

        while (node && node.left !== null) { 

            node = node.left 

        } 

        return node.val 

    } 

    return null 

} 

  

// 获取最大值 

const findMax = node => { 

    if (node) { 

        while (node && node.right !== null) { 

            node = node.right 

        } 

        return node.val 

    } 

    return null 

} 

```

  

注意：我的这种树生成的写法是允许重复的，但如果你后期打算进行任何类型的"树旋转"操作,重复的节点将会导致问题 