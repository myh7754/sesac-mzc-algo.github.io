---
status: done
language: java
---

## 접근

1. 1초 단위로 시간을 진행시키는 시뮬레이션. 다리 위 트럭을 큐로 관리한다.
2. 흔한 풀이는 다리 길이만큼 큐를 0으로 채워두고 매초 밀어내는 방식인데, 다리 길이가 길면 의미 없는 0을 계속 돌리게 된다. 대신 큐에 `{트럭 무게, 나갈 시간}`을 넣어서 **나갈 시점만 기억**한다.
3. 매 초마다
   - `time++`
   - 큐 맨 앞 트럭의 나갈 시간이 현재 시간과 같으면 빼고 `currentWeight`에서 차감
   - 남은 트럭이 있고 `currentWeight + 다음 트럭 무게 <= weight`면 진입시키고, 나갈 시간을 `time + bridge_length`로 기록
4. 트럭은 1초에 최대 1대만 진입하므로 나갈 시간이 서로 겹치지 않는다. 그래서 매 틱에 `poll()`을 한 번만 확인해도 충분하다.
5. 대기 트럭도 없고 다리 위도 비면 종료. 이때의 `time`이 마지막 트럭이 완전히 건넌 시각이다.

## 풀이

```java
class Solution {
    public int solution(int bridge_length, int weight, int[] truck_weights) {

        Queue<int[]> bridge = new LinkedList<>();

        int time = 0;
        int currentWeight = 0;
        int index = 0;

        while (index < truck_weights.length || !bridge.isEmpty()) {

            time++;

            // 나갈 시간이 된 트럭 제거
            if (!bridge.isEmpty() && bridge.peek()[1] == time) {
                currentWeight -= bridge.poll()[0];
            }

            // 다음 트럭 진입
            if (index < truck_weights.length
                    && currentWeight + truck_weights[index] <= weight) {

                int truck = truck_weights[index];

                // 현재 시간 + bridge_length에 빠져나감
                bridge.offer(new int[]{truck, time + bridge_length});

                currentWeight += truck;
                index++;
            }
        }

        return time;
    }
}
```

시간 O(트럭 수 + 총 소요 시간), 공간 O(다리 위 트럭 수)

## 막혔던 부분

1. **큐에 나갈 시간을 저장하는 발상.** 처음엔 다리 길이만큼 큐를 0으로 채우고 매초 한 칸씩 미는 방식을 생각했는데, `{무게, 나갈 시간}` 쌍으로 들고 있으면 밀어낼 필요 없이 맨 앞만 확인하면 된다는 걸 알고 나니 코드가 훨씬 짧아졌다.
