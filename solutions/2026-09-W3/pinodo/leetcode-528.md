---
status: done
language: python
# 블로그 등 풀이 원문이 있으면 아래 주석을 해제해 입력합니다.
# url: https://example.com/solution
---

## 접근
각 원소의 가중치를 이전 원소의 가중치와 더해서 stack에 저장.
가중치를 배열의 길이로 간주하고, index 0(min)에서 모든 가중치를 더한 값(max) 사이에서 랜덤으로 값을 하나 뽑음.
뽑은 값을 stack에서 binary search로 탐색해서 stack 원소 안의 값 안에 포함되면 그 원소의 인덱스를 리턴함.

## 풀이

```python
class Solution:

    def __init__(self, w: list[int]):
        self.stack = []
        sums = 0

        for weigh in w:
            sums += weigh
            self.stack.append(sums)

        self.totalSum = sums


    def pickIndex(self) -> int:
        picked = random.randint(1, self.totalSum)
        lo, hi = 0, len(self.stack) - 1

        while (lo < hi):
            mid = (lo + hi) // 2
            if (picked > self.stack[mid]):
                lo = mid + 1
            else:
                hi = mid

        return lo


# Your Solution object will be instantiated and called as such:
# obj = Solution(w)
# param_1 = obj.pickIndex()
```

시간복잡도: O(N) - sum(), O(logN) - pickIndex()

## 막혔던 부분
1. 문제 자체가 이해가 안됨 -> 예제를 AI에 물어봄
2. 접근 방식이 생각이 안남 -> 어떤 방법으로 접근해야될지 AI에 물어봄
3. Binary Search 개념 공부 및 적용
