# 02 Add Two Numbers

~~~ cpp
class Solution {
public:
	ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
		ListNode* nodes[150];
		int idx = 0;
		int sum = 0;
		int rup = 0;
		while (l1||l2) {
			if (l1) {
				sum += l1->val;
				l1 = l1->next;
			}
			if (l2) {
				sum += l2->val;
				l2 = l2->next;
			}
			sum += rup;
			rup = sum / 10;
			nodes[idx++] = new ListNode(sum % 10);
			sum = 0;
		}
		if (rup > 0) {
			nodes[idx++] = new ListNode(rup);
		}

		for (int i = 0; i < idx-1; i++) {
			nodes[i]->next = nodes[i + 1];
		}
		return nodes[0];
	}
};
~~~
`Runtime: 28 ms, faster than 93.66%` <br>
`Memory Usage: 71.5 MB, less than 24.69%`<br>

역순으로 되어있는 node 값을 더하고 역순으로 출력하는 문제 <br>
첨에 integer에 저장하려고했더니 숫자 범위가 1~100 ㅜ <br>
그래서 뒤에서부터 한 자리씩 더하고 그때그때 저장했다 <br>
맨 마지막 올림자리 남은 것을 안 더해서 조금 해맸다.

