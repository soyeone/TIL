# 121 Best Time to Buy and Sell Stock

~~~ cpp
class Solution {
public:
	int maxProfit(vector<int>& prices) {
		int max = 0;
		for (int i = 0; i < prices.size() - 1; i++) {
			for (int j = i + 1; j < prices.size(); j++) {
				if (prices[i] < prices[j]) {
					if (prices[j] - prices[i] > max)
						max = prices[j] - prices[i];
				}
			}
		}
		return max;
	}
};
~~~
그냥 단순히 n(O^2)로 풀었더니 time exceeded ㅜ 

~~~ cpp
class Solution {
public:
	int maxProfit(vector<int>& prices) {
		int max = 0;
		int min = 2147483647;
		for (int i = 0; i < prices.size(); i++) {
			if (prices[i] < min)
				min = prices[i];
			else if (prices[i] - min > max)
				max = prices[i] - min;
		}

		return max;
	}
};
~~~

`Runtime: 8 ms, faster than 95.85%` <br>
`Memory Usage: 13.3 MB, less than 6.13%`

옛날에 ssafy에서 비슷한거 했었는데 고새 까먹어서.. 오래고민했따





