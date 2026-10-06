---
status: done
language: java
---

## 접근

1. 처음에는 3중 반복문으로 모든 조합을 확인했다. 정답은 맞았지만 `n`이 최대 1000이라 `O(n^3)`은 시간 초과가 났다.
2. 먼저 **정렬**한다. 정렬하면 `i < j < k`일 때 `nums[k]`가 항상 최댓값이므로, 삼각형 조건 3개(`a+b>c`, `b+c>a`, `a+c>b`)를 전부 볼 필요 없이 **`nums[i] + nums[j] > nums[k]` 하나만** 확인하면 된다.
3. 가장 큰 수 `k`를 바깥 반복문으로 고정하고, 남은 구간 `[0, k-1]`에서 작은 두 수를 찾는 문제로 바꾼다.
4. 그 구간을 **투 포인터**로 좁힌다. `left = 0`, `right = k - 1`에서 시작해
   - 합이 `nums[k]`보다 크면 조건 성립 → `right--`
   - 합이 부족하면 왼쪽 값을 키우는 수밖에 없으므로 → `left++`
5. 핵심은 성립했을 때 **한꺼번에 세는 것**이다. 정렬돼 있으므로 `left`를 `right` 직전까지 올려도 합은 더 커질 뿐이라 전부 성립한다. 따라서 `count++`를 반복하지 않고 `count += right - left`로 범위를 통째로 더한다.
6. 이 한 줄 덕분에 "값이 전부 같은" 최악 입력에서도 반복 횟수가 늘어나지 않아 `O(n^2)`가 유지된다.

## 풀이

```java
import java.util.*;

class Solution {
    public int triangleNumber(int[] nums) {
        Arrays.sort(nums);                          // ① 정렬 → nums[k]가 항상 최댓값
        int n = nums.length, count = 0;

        // 가장 큰수를 기준으로 뒤에서 부터 하나씩 k로 잡음
        // k가 2 미만이면 남은 수가 2개가 안되므로 삼각형을 못만든다.
        for (int k = nums.length -1; k>=2; k--) {

            // 남은 구간 [0, k-1]을 양쪽에
            int left = 0;
            int right = k-1;
            while(left < right) {
                if(nums[left] + nums[right] > nums[k]) {
                    // 여기서 원래 for문이 들어가지만 left를 올려도 성립 하는건 같아 숫자만 계산해서 넘어갈 수 있음.
                    count += right -left;

                    right--;
                } else {
                    left++;
                }
            }
        }
        return count;
    }
}
```

시간 O(n^2), 공간 O(1) (정렬 제외)

## 막혔던 부분

1. 투 포인터 개념 자체. 두 포인터를 어느 방향으로 움직여야 하는지, 그렇게 움직여도 정답을 빠뜨리지 않는다는 걸 어떻게 보장하는지 판단하는 부분이 어려웠습니다.
