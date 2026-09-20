---
status: done
language: java
---

## 접근

1. 각 시점의 가격이 **언제 떨어지는지**를 찾는 문제. 떨어지지 않으면 끝까지 버틴 시간을 센다.
2. 이중 반복문(O(n²))으로도 풀리지만, 이미 지나간 인덱스 중 "아직 떨어지는 순간을 못 만난 것"만 들고 있으면 한 번 훑기로 끝난다 → 스택.
3. 스택에는 **가격이 아니라 인덱스**를 넣는다. 답이 "몇 초 버텼는지"라서 `현재 인덱스 - 저장한 인덱스`로 거리를 계산해야 하기 때문.
4. `i`를 보면서 스택 맨 위 인덱스의 가격이 `prices[i]`보다 크면(= 가격이 떨어졌으면) 꺼내서 `answer[index] = i - index`로 확정하고, 떨어질 때까지 계속 꺼낸다.
5. 그 다음 `i`를 스택에 넣는다.
6. 루프가 끝나고 스택에 남은 인덱스는 **끝까지 가격이 안 떨어진 것**이므로 `answer[index] = prices.length - 1 - index`로 채운다.
7. 각 인덱스가 스택에 정확히 한 번 들어가고 한 번 나오므로 전체 O(n).

## 풀이

```java
class Solution {
    public int[] solution(int[] prices) {
        int[] answer = new int[prices.length];
        List<Integer> s = new ArrayList<>();
        for (int i = 0; i < prices.length; i++) {
            while (!s.isEmpty() && prices[s.get(s.size() - 1)] > prices[i]) {
                // 가장 최근에 넣은 인덱스
                int index = s.remove(s.size() - 1);
                // 현재 인덱스랑 스택의 마지막 index 만큼의 차로 차이를 구하기
                answer[index] = i - index;
            }
            // index 추가
            s.add(i);
        }

        // 마지막으로 떨어지지 않은 경우
        while (!s.isEmpty()) {
            int index = s.remove(s.size() - 1);
            answer[index] = prices.length - 1 - index;
        }
        return answer;
    }
}
```

시간 O(n), 공간 O(n)

## 막혔던 부분

1. **O(n²) → 스택 최적화로 넘어가는 과정.** 이중 반복문 풀이는 바로 나왔는데, 이걸 스택으로 바꾸는 게 어려웠다. "가격이 떨어질 때 과거 인덱스를 한꺼번에 정산한다"는 관점으로 바꾸고 나서야 while 루프의 모양이 잡혔다.
2. 처음에 스택에 가격을 넣었다가 거리 계산이 안 돼서 인덱스로 바꿨다.
