# 20 Valid Parentheses

~~~ cpp
class Solution {
public:
	bool isValid(string s) {
		stack<char> st;

		for (int i = 0; i < s.size(); i++) {
			if (s[i] == ')') {
				if (st.empty()) {
					return false;
				}
				else if(st.top()!='('){
					return false;
				}
				st.pop();
			}
			else if (s[i] == '}') {
				if (st.empty()) {
					return false;
				}
				else if (st.top() != '{') {
					return false;
				}
				st.pop();
			}
			else if (s[i] == ']') {
				if (st.empty()) {
					return false;
				}
				else if (st.top() != '[') {
					return false;
				}
				st.pop();
			}
			else {
				st.push(s[i]);
			}
		}
		return st.empty();
	}
};
~~~
`0~4ms / 6.2MB`

스택에 넣고 닫힌 괄호를 만나면 짝이 있는지 확인 후 pop 하는 방식 

아래는 코드 길이 짧은 버전 

~~~ cpp
class Solution {
public:
    bool isValid(string s) {
        std::stack<char> st;
        for(char i: s){
            if(i == '}' && !st.empty() && st.top()=='{')
                st.pop();
            else if(i == ')' && !st.empty() && st.top()=='(')
                st.pop();
            else if(i == ']' && !st.empty() && st.top()=='[')
                st.pop();
            else st.push(i);
        }
        return st.empty();
    }
};
~~~
`0~4ms / 6.2MB`
