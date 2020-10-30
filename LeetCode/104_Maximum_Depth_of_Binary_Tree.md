# 104 Maximum Depth of Binary Tree

~~~ cpp
class Solution {
public:
	int max = 0;
	int maxDepth(TreeNode* root) {
		if (!root) {
			return 0;
		}
		else if (!root->left && !root->right) {
			return 1;
		}
		else {
			recur(root, 1);
		}
		return max;
	}

	void recur(TreeNode* node, int num) {
		if (num > max)
			max = num;
		
		if (node->left)
			recur(node->left, num + 1);
		if (node->right)
			recur(node->right, num + 1);
		
	}
};
~~~
`Runtime: 12 ms, faster than 68.82% ` <br>
`Memory Usage: 19.2 MB, less than 15.66%`

주어진 Binary tree 의 max depth 를 구하는 문제<br>
null 이 0 인걸 미리 체크했어야하는데 ㅜ 



