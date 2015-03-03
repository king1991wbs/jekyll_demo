---
layout : post
title : best
category : blog
description : leetcode
---

今天在刷leetcode  best time to buy and sell stock这题，起初我提交的这样一份结果：

<pre><code>
class Solution{
public:
	int maxProfit(vector&lt;int> &amp;prices){

		int profit = 0;
		int maxP = 0;
		for(int i = 0; i &lt; prices.size() - 1; ++i){
			profit += prices[i+1] - prices[i];
			if(profit &lt;= 0){
				profit = 0;
				continue;
			}
			maxP = max(maxP, profit);
		}

		return maxP;
	}
};
</code></pre>

结果总是提示输入为空的时候run time error...于是打开gdb开始调试，发现就算prices传入一个空的vector对象也会进入for循环内部。

非常想不通，于是到cppreference上查vector.size()函数的借口情况，得到如下解释：
<pre><code>
size_type size() const;
</code></pre>
原来size()函数的返回值类型是size_type，于是猜到了能是在gcc下为无符号数值类型。如此便解释得通了。于是提交新的代码，不出所料ac成功。

<pre><code>
class Solution{
public:
	int maxProfit(vector&lt;int> &amp;prices){

		int profit = 0;
		int maxP = 0;
		int eleSize = prices.size();//size()函数返回的值是无符号整形，因此直接用在for循环prices.szie()-1下溢
		for(int i = 0; i &lt; eleSize - 1; ++i){
			profit += prices[i+1] - prices[i];
			if(profit &lt;= 0){
				profit = 0;
				continue;
			}
			maxP = max(maxP, profit);
		}

		return maxP;
	}
};
</code></pre>
