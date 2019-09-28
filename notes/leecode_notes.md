

[TOC]

# 力扣题解

## 栈

### 题目1047：

给出由小写字母组成的字符串 S，重复项删除操作会选择两个相邻且相同的字母，并删除它们。在 S 上反复执行重复项删除操作，直到无法继续删除。在完成所有重复项删除操作后返回最终的字符串。答案保证唯一。

示例：

输入："abbaca"  
输出："ca"  
解释：  
例如，在 "abbaca" 中，我们可以删除 "bb" 由于两字母相邻且相同，这是此时唯一可以执行删除操作的重复项。之后我们得到字符串 "aaca"，其中又只有 "aa" 可以执行重复项删除操作，所以最后的字符串为 "ca"。

题解：栈空或栈顶元素不与下一个字符串元素相同入栈；栈顶元素与下一个字符串元素相同，则栈顶出栈，且跳过这一字符串元素；重复上述操作，直至最后一个元素，最后再用一个栈逆序输出

代码：

```c++
#include<iostream>
#include<stack>
using namespace std;
string removeDuplicates(string S) {
	stack<char>st;
 	for(int i=0;i<S.size();i++){
  		if(st.empty()||st.top()!=S[i]){
           st.push(S[i]);
           continue;
    	}else {
    		st.pop();
    	}
  }
  stack<char>stReverse;
  while(!st.empty()){
  	stReverse.push(st.top());
   	st.pop();
  }
  string res;
  while(!stReverse.empty()){
  	res+=stReverse.top();
	stReverse.pop();
  }
 return res;
} 
int main(){
	string s;
	cin>>s;
	cout<<removeDuplicates(s)<<endl;
	return 0;
}
```

### 题目496:

给定两个没有重复元素的数组 nums1 和 nums2 ，其中 nums1 是 nums2 的子集。找到 nums1 中每个元素在 nums2 中的下一个比其大的值。

nums1 中数字 x 的下一个更大元素是指 x 在 nums2 中对应位置的右边的第一个比 x 大的元素。如果不存在，对应位置输出 - 1。

**示例：**

输入: nums1 = [4,1,2], nums2 = [1,3,4,2].
输出: [-1,3,-1]
解释:
    对于num1中的数字4，你无法在第二个数组中找到下一个更大的数字，因此输出 -1。
    对于num1中的数字1，第二个数组中数字1右边的下一个较大数字是 3。
    对于num1中的数字2，第二个数组中没有下一个更大的数字，因此输出 -1。

**题解：**



1. 维护一个递减栈
2. 依次遍历nums2数组中的元素，
   1. 若栈空则入栈
   2. 栈不空，比较当前元素与栈顶元素的大小，栈顶大于此元素，则入栈，反之出栈；直到栈圩空或者栈顶元素小于此元素才入栈。出栈的元素存入map中，key为出栈元素，value为此元素
   3. 若nums2遍历完成，栈不为空，则将剩余元素value=-1；

3. 遍历nums1，以num1为key遍历map，将得到的value依次存入数组，

4. 所得即答案

   

   **代码：**



```c++
#include<iostream>
#include<stack>
#include<map>
#include<vector>
using namespace std;
vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
        //维护一个单调递减的栈
        vector<int>res;
        if(nums1.size()==0)return res;
        stack<int>st;
        map<int,int>mmap;
        st.push(nums2[0]);
        for(int i=1;i<nums2.size();i++){
            if(st.top()>nums2[i])st.push(nums2[i]);
            else{
                while(!st.empty()){
                    if(st.top()<nums2[i]){
                        mmap[st.top()]=nums2[i];
                        cout<<st.top()<<" "<<nums2[i]<<endl;
                        st.pop();
                    }else break;
                }
                st.push(nums2[i]);
            }
        }
        while(!st.empty()){
            mmap[st.top()]=-1;
            cout<<st.top()<<" "<<"-1"<<endl;
            st.pop();
        }
        for(int i=0;i<nums1.size();i++){
            res.push_back(mmap[nums1[i]]);
        }
        return res;
    }
    int main(){
    	vector<int> nums1,nums2,ans;
    	int a[10]={
	    	4,1,2
	    };
	    int b[10]={
    		1,3,4,2
    	};
    	for(int i=0;i<3;i++){
	    	nums1.push_back(a[i]);
	    }
	    for(int i=0;i<4;i++){
    		nums2.push_back(b[i]);
    	}
    	ans=nextGreaterElement(nums1,nums2);
    	for(int i=0;i<ans.size();i++){
	    	cout<<ans[i]<<" "<<endl; 
	    }
    	return 0;
    }
```

### 题目225：



*题目：*使用队列实现栈的下列操作：

1. push (x) -- 元素 x 入栈
2. pop () -- 移除栈顶元素
3. top () -- 获取栈顶元素
4. empty () -- 返回栈是否为空

*分析：*

1. 利用单队列，加一个中间变量得操作
2. 入栈如入队
3. 出栈时，将队列依次出队再入队，进行n-1次，直至将最后一个元素移至队头
4. 最后出队，即为出栈结束

*代码：*



```c++
class MyStack {
public:
    /** Initialize your data structure here. */
    queue<int> q1;
    int temp;
    MyStack() {
        
    }
    
    /** Push element x onto stack. */
    void push(int x) {
        q1.push(x);
    }
    
    /** Removes the element on top of the stack and returns that element. */
    int pop() {
        for(int i=1;i<q1.size();i++){
            q1.push(q1.front());
            q1.pop();
        }
        temp=q1.front();
        q1.pop();
        return temp;
    }
    
    /** Get the top element. */
    int top() {
        for(int i=1;i<q1.size();i++){
            q1.push(q1.front());
            q1.pop();
        }
        temp=q1.front();
        q1.push(q1.front());
        q1.pop();
        return temp;
    }
    
    /** Returns whether the stack is empty. */
    bool empty() {
        return q1.empty();
    }
};

/**
 * Your MyStack object will be instantiated and called as such:
 * MyStack* obj = new MyStack();
 * obj->push(x);
 * int param_2 = obj->pop();
 * int param_3 = obj->top();
 * bool param_4 = obj->empty();
 */
```

### 题目232：



**使用栈实现队列的下列操作**：

1. push (x) -- 将一个元素放入队列的尾部。
2. pop () -- 从队列首部移除元素。
3. peek () -- 返回队列首部的元素。
4. empty () -- 返回队列是否为空。



***分析**：*

1. 利用双栈，一个栈做入队操作，一个栈做出队操作
2. 如果出队栈为空，将入队栈元素依次压入出队栈，
3. 如果入队栈满，则也将元素压入出队栈



***代码**：*



```c++
class MyQueue {
public:
    /** Initialize your data structure here. */
    stack<int> st1,st2;
    MyQueue() {
        
    }
    
    /** Push element x to the back of queue. */
    void push(int x) {
        st1.push(x);
    }
    
    /** Removes the element from in front of queue and returns that element. */
    int pop() {
        if(!st2.empty()){
            int res=st2.top();
            st2.pop();
            return res;
        }
        while(!st1.empty()){
            st2.push(st1.top());
            st1.pop();
        }
        int res=st2.top();
        st2.pop();
        return res;
    }
    
    /** Get the front element. */
    int peek() {
        if(!st2.empty()){
            return st2.top();
        }
        while(!st1.empty()){
            st2.push(st1.top());
            st1.pop();
        }
        return st2.top();;
    }
    
    /** Returns whether the queue is empty. */
    bool empty() {
        if(st1.empty()&&st2.empty())return true;
        else return false;
    }
};

/**
 * Your MyQueue object will be instantiated and called as such:
 * MyQueue* obj = new MyQueue();
 * obj->push(x);
 * int param_2 = obj->pop();
 * int param_3 = obj->peek();
 * bool param_4 = obj->empty();
 */
```



### 题目844：

**题目描述：**

给定 `S` 和 `T` 两个字符串，当它们分别被输入到空白的文本编辑器后，判断二者是否相等，并返回结果。 `#` 代表退格字符。

**示例：**

输入：S = "ab#c", T = "ad#c"
输出：true
解释：S 和 T 都会变成 “ac”。

**分析：**

1. 声明两个栈，遍历字符串，遇到字母入栈，遇到‘#’出栈(ps:出栈需要判断是否为空）；

2. 遍历完成之后，两栈为空，则返回true
3. 若不为空，则依次比较栈顶，若有不相等则返回false，若相等则出栈
4. 直至栈为空，则返回true

**代码：**



```c++
class Solution {
public:
    bool backspaceCompare(string S, string T) {
        stack<char>st1,st2;
        for(int i=0;i<S.size();i++){
            if(S[i]!='#')st1.push(S[i]);
            else if(st1.size()>0)st1.pop();
        }
        for(int i=0;i<T.size();i++){
            if(T[i]!='#')st2.push(T[i]);
            else if(st2.size()>0)st2.pop();
        }
        if(st1.empty()&&st2.empty())return true;
        else if(st1.size()!=st2.size())return false;
        else{
            while(st1.size()!=0){
                if(st1.top()!=st2.top())return false;
                st1.pop();
                st2.pop();
            }
            return true;
        }
    }
};
```

### 题目155：

**题目描述：**

设计一个支持 push，pop，top 操作，并能在常数时间内检索到最小元素的栈。

* push (x) -- 将元素 x 推入栈中。
* pop () -- 删除栈顶的元素。
* top () -- 获取栈顶元素。
* getMin () -- 检索栈中的最小元素。

**示例：**

MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin();   --> 返回 -3.
minStack.pop();
minStack.top();      --> 返回 0.
minStack.getMin();   --> 返回 -2.

**分析：**

1. O(n)的时间复杂度：直接定义一个数组栈，每次getmin(),遍历整个数组找到min
2. O(1)的时间复杂度：必定要用空间换时间
   * 每次一个元素准备入栈，则压入两个元素。一个为入栈元素，一个为当前状态小的最小值
   * 每次出栈出两个元素
   * 获取此时栈的最小值，只用st.top()即可
   * ps：也可以用双栈，一个数据栈，一个最小值栈，每次数据栈入栈，最小值也压入一个当前状态下的最小值；出栈则二者都出；获取当前状态的最小值，则获取最小值栈的栈顶元素

**代码：**



```c++
class MinStack {
public:
    /** initialize your data structure here. */
    stack<int>st;
    MinStack() {
        
    }
    
    void push(int x) {
        int temp;
        if(st.empty()){
            st.push(x);
            st.push(x);
        }else{
            temp=st.top();
            st.push(x);
            if(temp<x)st.push(temp);
            else st.push(x);
        }
    }
    
    void pop() {
        st.pop();
        st.pop();
    }
    
    int top() {
        int temp=st.top();
        st.pop();
        int res=st.top();
        st.push(temp);
        return res;
    }
    
    int getMin() {
        return st.top();
    }
};

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack* obj = new MinStack();
 * obj->push(x);
 * obj->pop();
 * int param_3 = obj->top();
 * int param_4 = obj->getMin();
 */
```

### 94_二叉树的中序遍历：

**题目：**

给定一个二叉树，返回它的*中序* 遍历。

**示例：**

```
输入: [1,null,2,3]
   1
    \
     2
    /
   3

输出: [1,3,2]
```

**分析：**

1. 中序遍历：左根右，先访问最左边的节点，然后返回上一层或右边，所以利用栈存储祖先节点
2. 当前指向的节点p不为空，或栈不为空继续循环
3. 当前指向的节点p不为空，则节点p入栈。p指向p左节点，继续循环
4. 内层循环结束，访问栈顶元素，p指向栈顶元素的右孩子，栈顶元素出栈。

**代码：**

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        stack<TreeNode*>st;
        vector<int>res;
        TreeNode* p=root;
        while(p!=NULL||!st.empty()){
            while(p!=NULL){
                st.push(p);
                p=p->left;
            }
            p=st.top();
            res.push_back(p->val);
            p=p->right;
            st.pop();
        }
        return res;
    }
};
```

### 144_二叉树的前序遍历

**题目：**

给定一个二叉树，返回它的 *前序* 遍历。

**示例：**

```
输入: [1,null,2,3]  
   1
    \
     2
    /
   3 

输出: [1,2,3]
```

**分析：**

前序遍历：中左右，1. 访问当前节点，2. 前序遍历其左孩子，3. 前序遍历右孩子，利用栈存储已经访问过的祖先节点

**代码：**



```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        //先访问再入栈
        vector<int>res;
        stack<TreeNode*>st;
        TreeNode* p=root;
        while(p!=NULL||!st.empty()){
            while(p!=NULL){
                res.push_back(p->val);
                st.push(p);
                p=p->left;
            }
            p=st.top()->right;
            st.pop();
        }
        return res;
    }
};
```

### 145_二叉树的后序遍历

**题目：**

给定一个二叉树，返回它的 *后序* 遍历。

**示例：**

```
输入: [1,null,2,3]  
   1
    \
     2
    /
   3 

输出: [3,2,1]
```

**分析：**

1. 二叉树的后序遍历：左右中，①对左子树进行后序遍历，②对右子树进行后序遍历，③

遍历当前节点。

2. 先序遍历为中左右，把先序序列逆置则为右左中，后序为左右中，则只要将先序遍历左右顺序交换，就是逆后序遍历序列

**代码：**

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        //对前序遍历进行改良，利用双栈
        stack<TreeNode*> st1;
        stack<int> st2;
        vector<int> res;
        TreeNode* p=root;
        while(p!=NULL||!st1.empty()){
            while(p!=NULL){
                st1.push(p);
                st2.push(p->val);
                p=p->right;
            }
            p=st1.top()->left;
            st1.pop();
        }
        while(!st2.empty()){//逆置st2得到后序序列
            res.push_back(st2.top());
            st2.pop();
        }
        return res;
    }
};
```

### 730.每日温度

**题目：**

根据每日 气温 列表，请重新生成一个列表，对应位置的输入是你需要再等待多久温度才会升高超过该日的天数。如果之后都不会升高，请在该位置用 0 来代替。

**例如：**

给定一个列表 temperatures = [73, 74, 75, 71, 69, 72, 76, 73]，你的输出应该是 [1, 1, 4, 2, 1, 1, 0, 0]。

**分析：**

动态规划+递减栈（ps：栈可存值，也可以存下标）

1. 因为是和未来几天进行比较，所以选择从后往前进行比较
2. 维护一个递减栈，栈空，则入栈，且res所对应的值为0
3. 栈不空，栈顶所对应的元素大于当前元素，res的值为栈顶减去当前元素的下标，并且当前元素下标入栈
4. 栈不空，栈顶所对应的元素、小于当前元素，则出栈。重复1，2操作
5. 最后返回res逆置

**代码：**



```c++
class Solution {
public:
     vector<int> dailyTemperatures(vector<int>& T) {
        //单调栈+动态规划
        //向下单增
        stack<int> st;//st可存值或者下标
        vector<int> res;
        res.push_back(0);
        st.push(T.size() - 1);
        for (int i = 0; i < T.size() - 1; i++) {
            int tag = T.size()-i - 2;
            while (T[tag] >= T[st.top()]) {
                st.pop();
                if (st.empty())break;
            }
            if (st.empty()) {
                st.push(tag);
                res.push_back(0);
            }
            else {
                res.push_back(st.top() - tag);
                st.push(tag);
            }
        }
        reverse(res.begin(), res.end());
        return res;
    }
};
```

### 173.二叉搜索树迭代器

**题目：**

实现一个二叉搜索树迭代器。你将使用二叉搜索树的根节点初始化迭代器。

调用 `next()` 将返回二叉搜索树中的下一个最小的数。

**示例：**

![][1]

BSTIterator iterator = new BSTIterator(root);
iterator.next();    // 返回 3
iterator.next();    // 返回 7
iterator.hasNext(); // 返回 true
iterator.next();    // 返回 9
iterator.hasNext(); // 返回 true
iterator.next();    // 返回 15
iterator.hasNext(); // 返回 true
iterator.next();    // 返回 20
iterator.hasNext(); // 返回 false

**分析：**

1. 二叉搜索树的中序遍历可以得到一个单增序列
2. 将中序遍历非递归方法进行拆解
3. 初始化时候，栈顶存放向最左边的元素
4. 每次next（），pop栈顶，然后将栈顶右子树用3操作，此时栈顶存放pop节点的右子树最左边的节点
5. 每次hasNext（）就是判断是否栈空

**代码：**



```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class BSTIterator {
public:
    stack<TreeNode*> st;
    TreeNode* p;
    BSTIterator(TreeNode* root) {
        p=root;
        while(p){
            st.push(p);
            p=p->left;
        }
    }
    
    /** @return the next smallest number */
    int next() {
        int res;
        if(!st.empty()){
            res=st.top()->val;
        }
        p=st.top()->right;
        st.pop();
        while(p){
            st.push(p);
            p=p->left;
        }
        return res;
    }
    
    /** @return whether we have a next smallest number */
    bool hasNext() {
        return !st.empty();
    }
};

/**
 * Your BSTIterator object will be instantiated and called as such:
 * BSTIterator* obj = new BSTIterator(root);
 * int param_1 = obj->next();
 * bool param_2 = obj->hasNext();
 */
```





[1]:bst-tree.png