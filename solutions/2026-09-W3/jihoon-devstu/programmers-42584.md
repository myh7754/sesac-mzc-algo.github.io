---
status: done
language: java
# 블로그 등 풀이 원문이 있으면 아래 주석을 해제해 입력합니다.
# url: https://example.com/solution
---

## 문제 설명
- prices 의 배열을 돌며 현재 인덱스의 숫자가 직전 인덱스의 숫자보다 낮아진 경우 , 트리거 발생
- 예전 인덱스들 중 , 현재 인덱스보다 숫자가 높은 요소들을 전부 계산하셔 answer 배열에 인덱스를 계산하여 담기
- 숫자가 낮아지지 않은 요소들 계산하여 answer 담기

## 접근
- stack이 empty 인 경우엔 stack 현재 index를 push 
- stack이 empty 가 아니며 , 현재 stack의 마지막에 들어간 값 (직전인덱스 값)이 price[i] 보다 크다 , 즉 이번에 들어온 prices[i] 값이 더 작으면
    그 인덱스 값을 계산하여 answer 의 index값을 계산하여 담기.

## 풀이

```java
import java.util.*;

class Solution {
    public int[] solution(int[] prices) {
        Deque<Integer> stack = new ArrayDeque<>();
        int[] answer = new int[prices.length];
        for(int i = 0 ; i< prices.length; i++){
            while(!stack.isEmpty() && prices[stack.peek()] > prices[i]){
                int a = stack.pop();
                answer[a] = i - a;
            }
            
            stack.push(i);
        }
        
        while(!stack.isEmpty()){
            int a = stack.pop();
            answer[a] = (prices.length -1) - a ;
        }
        

        return answer;
    }
}

```

소요시간: 약 60분
시간복잡도 , 공간복잡도 모두 O(n)

## 막혔던 부분

접근 1의 trigger 가 되어야 하는 부분과 접근 방식은 알아내었지만 , 이걸 코드로 구현하는 부분과
Stack에 index를 담았을 때 , answer 에 담아야 하는 index를 계산하는부분에서 시간이 많이 들었습니다.