# 01 Two Sum

~~~ cpp
class Solution {
public:
	vector<int> twoSum(vector<int>& nums, int target) {
		vector<int> ans(2);

		long long len = nums.size();

		for (int i = 0; i < len; i++) {
			for (int j = i + 1; j < len; j++) {
				if (nums[i] + nums[j] == target) {
					ans[0] = i;
					ans[1] = j;
					return ans;
				}
			}
		}

		return ans;
	}
};
~~~
`528 ms / 9.4MB`

단순히 모든 경우를 찾아 더하고 target과 비교했다. 

더 효율적인 방법을 찾던 중 배열의 뺀 값을 저장하고 그 값을 단순 비교하면 되지 않을 까 했으나 속도가 뛰어나게 차이나지 않아 다른사람 코드를 보던 중 map 을 써 for 문 한 번으로 해결한 방법을 발견하여 적용한 코드
<br>

~~~ cpp
class Solution {
public:
	vector<int> twoSum(vector<int>& nums, int target) {
		map<int, int> m;

		for (int i = 0; i < nums.size(); i++) {
			if (m.find(nums[i]) != m.end()) {
				return{ m[nums[i]],i };
			}
            m[target - nums[i]] = i;
		}
        return {};
	}
};
~~~
`20 ms / 9.9MB`

다양한 문제를 다양한 방법으로 풀어봐야 겠다.
