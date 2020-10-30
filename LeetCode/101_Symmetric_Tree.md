# 101 Symmetric Tree

~~~ cpp
class Solution {
public:
	bool isSymmetric(TreeNode* root) {
		if (root) {
			if (!root->left && !root->right) {
				return true;
			}
			else if (!root->left || !root->right) {
				return false;
			}
			else {
				return ck(root->left, root->right);
			}
		}
		else {
			return true;
		}
	}

	bool ck(TreeNode* t1, TreeNode* t2) {
		if (!t1 && !t2) {
			return true;
		}
		else if (!t1 || !t2) {
			return false;
		}
		else if (t1->val != t2->val) {
			return false;
		}
		else {
			return ck(t1->left, t2->right) && ck(t1->right, t2->left);
		}
	}
};
~~~
`Runtime: 4 ms, faster than 90.20% of C++ online submissions for Symmetric Tree.` <br>
`Memory Usage: 16.6 MB, less than 73.30% of C++ online submissions for Symmetric Tree.`

주어진 트리가 symmetric tree 인지 확인하는 문제 
재귀로 돌려서 풀었다



