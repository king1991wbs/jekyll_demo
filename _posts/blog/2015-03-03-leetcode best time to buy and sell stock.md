---
layout : post
title : best time buy and sell stock
category : blog
description : leetcode
---

������ˢleetcode  best time to buy and sell stock���⣬������ύ������һ�ݽ����
<pre><code>
class Solution{
public:
	int maxProfit(vector<int> &prices){

		int profit = 0;
		int maxP = 0;
		for(int i = 0; i < prices.size() - 1; ++i){
			profit += prices[i+1] - prices[i];
			if(profit <= 0){
				profit = 0;
				continue;
			}
			maxP = max(maxP, profit);
		}

		return maxP;
	}
};
</code></pre>

���������ʾ����Ϊ�յ�ʱ��run time error...���Ǵ�gdb��ʼ���ԣ����־���prices����һ���յ�vector����Ҳ�����forѭ���ڲ���

�ǳ��벻ͨ�����ǵ�cppreference�ϲ�vector.size()�����Ľ��������õ����½��ͣ�
<pre><code>
size_type size() const;
</code></pre>
ԭ��size()�����ķ���ֵ������size_type�����ǲµ���������gcc��Ϊ�޷�����ֵ���͡���˱���͵�ͨ�ˡ������ύ�µĴ��룬��������ac�ɹ���