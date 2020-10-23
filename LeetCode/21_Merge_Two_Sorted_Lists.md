# 21 Merge Two Sorted Lists

~~~ cpp
class Solution {
public:
	ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
		ListNode* ans = new ListNode(0);
		
		ListNode* tmp = ans;

		while (l1 && l2) {
			if (l1->val > l2->val) {
				tmp->next = l2;
				l2 = l2->next;
				tmp = tmp->next;
			}
			else if (l1->val < l2->val) {
				tmp->next = l1;
				l1 = l1->next;
				tmp = tmp->next;
			}
			else {
				tmp->next = l1;
				l1 = l1->next;
				tmp = tmp->next;

				tmp->next = l2;
				l2 = l2->next;
				tmp = tmp->next;
			}

		}
        
        if (!l1) {
			tmp->next = l2;
		}
		else if (!l2) {
			tmp->next = l1;
		}

		return ans->next;
	}
};
~~~

`Runtime: 12 ms, faster than 51.44% of C++ online submissions for Merge Two Sorted Lists.
Memory Usage: 15.3 MB, less than 90.96% of C++ online submissions for Merge Two Sorted Lists.`

sorted 된 두 개의 리스트를 merge 하는 문제
sorted 안된 경우도 있는 줄 알고 이상하게 헤매다 품 ㅜ 문제를 잘 읽자...
