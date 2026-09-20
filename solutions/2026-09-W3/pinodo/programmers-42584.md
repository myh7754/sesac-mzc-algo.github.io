---
status: done
language: python
# 블로그 등 풀이 원문이 있으면 아래 주석을 해제해 입력합니다.
# url: https://example.com/solution
---

## 접근
O(n**2)로 가격 리스트를 순회하면서, 각 리스트당 현재 순번에서 끝까지 한번 더 순회하며 가격이 낮아지는 시점 확인.
시간복잡도 개선하려면 가격이 낮아진 시점에, 과거의 요소들을 체크하고 한번에 처리하는 무언가가 필요해보임.

## 풀이

```python
# Try_1: 시간복잡도: O(n**2)
def solution(prices):
    stack = []
    for i in range(len(prices)):
        count, currPrice = 0, prices[i]
        while (i + 1 < len(prices)):
            if (currPrice > prices[i]):
                break
            count += 1
            i += 1
        stack.append(count)
    return stack


# AI 답안 (시간복잡도: O(n))
def solution(prices):
    n = len(prices)
    answer = [n - 1 - i for i in range(n)]  # 끝까지 안 떨어진 경우의 기본값
    stack = []  # 아직 가격이 떨어지지 않은 시점의 인덱스
    for i, price in enumerate(prices):
        while stack and prices[stack[-1]] > price:
            j = stack.pop()
            answer[j] = i - j  # j 시점의 가격이 i초에 처음 떨어짐
        stack.append(i)
    return answer
# 설명
# answer는 처음에 "끝까지 안 떨어졌다"고 가정한 값(n-1-i)으로 채워둬요. 그다음 가격을 하나씩 확인해요. 스택 맨 위 시점의 가격이 지금 가격보다 비싸면, 그 시점의 가격은 지금 처음 떨어진 거예요. 그래서 그 인덱스를 꺼내서 i - j로 기간을 기록해요. 맨 위가 지금 가격보다 비싸지 않을 때까지 이걸 반복한 뒤 현재 인덱스를 쌓아요. 끝까지 스택에 남은 인덱스는 한 번도 안 떨어진 거라서, 처음 넣어둔 기본값이 그대로 정답이에요.

# 예를 들어 [1, 2, 3, 2, 3]에서 i=3일 때 가격 2가 들어오면, 스택 맨 위의 인덱스 2(가격 3)가 꺼내지면서 answer[2] = 3 - 2 = 1이 돼요. 최종 결과는 [4, 3, 1, 1, 0]이에요.

# 스택 맨 위만 확인하면 되는 이유가 있어요. 이 방식대로 쌓으면 스택 안의 가격은 아래에서 위로 갈수록 항상 같거나 커져요. 그래서 맨 위가 안 떨어졌으면 그 아래도 전부 안 떨어진 거예요.

# for 안에 while이 있어서 O(n²)처럼 보이지만 실제로는 O(n)이에요. 각 인덱스는 스택에 딱 한 번 들어가고 많아야 한 번 나와요. 그래서 꺼내는 작업(pop)은 전체를 통틀어 n번을 넘을 수 없어요. 공간복잡도는 스택과 결과 리스트 때문에 O(n)이에요.
```

시간복잡도: O(n**2)

## 막혔던 부분

