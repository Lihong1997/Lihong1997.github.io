

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





```java
java.com .cn
```