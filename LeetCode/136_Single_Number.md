# 136 Single Number

~~~ cpp
class Solution {
public:
	int singleNumber(vector<int>& nums) {
		if (nums.size() == 1) {
			return nums[0];
		}

		sort(nums.begin(), nums.end());

		for (int i = 0; i < nums.size() - 1; i++) {
			if (nums[i] != nums[i + 1]) {
				return nums[i];
			}
			i++;
		}
		return nums[nums.size() - 1];
	}
};
~~~
`Runtime: 64 ms, faster than 12.38%` <br>
`Memory Usage: 17.2 MB, less than 9.48%`

주어진 arr 에 중복되지 않는 단 한개의 integer 값을 찾는 문제 <br>
정렬해서 혼자있으면 찾는 걸로 했음 <br>

~~~ cpp
class Solution {
public:
	int singleNumber(vector<int>& nums) {
        ios_base::sync_with_stdio(false);
        cin.tie(NULL);
		int ans = 0;
		for (int i = 0; i < nums.size(); i++) {
			ans ^= nums[i];
		}
		return ans;
	}
};
~~~

XOR 사용한 방법 <br>
비트연산자, 논리게이트 이런거 잘 안써버릇 하는데 잘 알아둬야겠다.. <br>

~~~ cpp
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);
~~~
cpp 에서 표준 stream의 동기화를 끊어 독립적인 버퍼를 갖게 하고 strem을 unite 시켜 실행속도를 향상시키는 커맨드 <br>
stream이 tie된 상태에서는 다른 stream에서 입출력 요청이 오기전에 stream 을 flush 시킴.

~~~ cpp
std::cout << "Enter name:" ;
std::cin >> name
~~~
위에서 stream이 tie 된 상태에서는 output이 flush 되고난 후 user input을 받는데 <br>
untie 된 상태에서는 "Enter name"이 출력되지 않고 user input을 요구할 것임 <br>
unite 상태에서 cin으로 입력을 받기 전 뭔가를 띄우고 싶다면 매번 수동적으로 cout을 flush 시켜줘야 함.

