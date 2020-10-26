# 53 Maximum Subarray

~~~ cpp
class Solution {
public:
	int maxSubArray(vector<int>& nums) {
		int max = nums.at(0);

		if (nums.size() <= 1) {
			return nums.at(0);
		}
		else {
			for (int i = 0; i < nums.size(); i++) {
				int tmp = 0;
				for (int j = i; j < nums.size(); j++) {
					tmp += nums.at(j);
					if (tmp > max)
						max = tmp;
				}
			}

		}

		return max;
	}
};
~~~
`Runtime: 1472 ms, faster than 5.41% of C++ online submissions for Maximum Subarray.` <br>
`Memory Usage: 13.6 MB, less than 14.69% of C++ online submissions for Maximum Subarray.`

주어진 array 에서 연속된 부분 배열의 인수를 더한 합 중 가장 큰 값을 return 하는 문제 

vecotr.at 보다 array를 쓰듯 nums[i] 로 바꿔주니 1472ms -> 832ms 로 시간이 줄었다.


~~~ cpp
class Solution {
public:
	int maxSubArray(vector<int>& nums) {
		int max = nums[0];

		int tmp = 0;
		for (int i = 0; i < nums.size(); i++) {
			tmp += nums[i];
			if (tmp > max) {
				max = tmp;
			}
			if (tmp < 0) {
				tmp = 0;
			}
		}

		return max;
	}
};
~~~
`Runtime: 8 ms, faster than 97.12% of C++ online submissions for Maximum Subarray.`<br>
`Memory Usage: 13.6 MB, less than 14.69% of C++ online submissions for Maximum Subarray.`

for 문 한 번에 끝낼 수 있는 방법.

