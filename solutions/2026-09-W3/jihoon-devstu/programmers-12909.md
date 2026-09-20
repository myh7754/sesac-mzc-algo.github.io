---
status: done
language: java
# 블로그 등 풀이 원문이 있으면 아래 주석을 해제해 입력합니다.
# url: https://example.com/solution
---

## 문제 설명
- 주어진 문자열 s의 괄호가 올바르게 설계 되었는지 파악하여 true or false 를 반환하시오.

## 접근
- '(' 를 만날 때 stack 에 1개를 push , 그리고 )를 만날 때 pop 을 하되 , 스택에 남아있는게 없거나 끝까지 돌았을 때 '(' 가 남아있다면 false 반환

## 풀이

```java
import java.util.*;

class Solution {
    boolean solution(String s) {
        boolean answer = true;

        Deque<Character> stack = new ArrayDeque<>();
        
        for(int i = 0 ; i< s.length(); i++){
            char c = s.charAt(i);
            if(c == '('){
                stack.push('(');
            }else if(c == ')'){
                if(stack.isEmpty()){
                    return answer = false;
                }
                
                stack.pop();
            }
        }
        
        answer = stack.isEmpty() ? true : false;
        return answer;
    }
}

```

## 개선된 풀이(AI)

```java

import java.util.*;

class Solution {
    boolean solution(String s) {
        boolean answer = true;
        int count = 0;

        for(int i = 0 ; i< s.length(); i++){
            char c = s.charAt(i);
            if(c == '('){
                count++;
            }else if(c == ')'){
                if(count == 0){
                    answer = false;
                    break;
                }

                count--;
            }
        }

        if(count != 0){
            answer = false;
        }

        return answer;
    }
}
```
시간복잡도 : O(n) , 공간복잡도 : O(n)

소요시간: 약 20분

## 막혔던 부분

최적화를 시키니 , 굳이 stack 에 '(' 를 집어넣는것이 아니라 int 값 하나로도 구할 수 있는 문제였다.
