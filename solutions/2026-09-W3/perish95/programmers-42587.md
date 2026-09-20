---
status: done
language: cpp
# 블로그 등 풀이 원문이 있으면 아래 주석을 해제해 입력합니다.
# url: https://example.com/solution
---

## 문제 설명
- 프로세스의 중요도가 높은 프로세스 먼저 실행하기를 원함
- 프로세스의 중요도가 담긴 `priorities` 배열을 제시
- 몇 번째로 실행되는지 알고 싶은 프로세스의 인덱스 `location`

## 접근
- 당연히 `queue`를 사용. `<priorities의 인덱스, 프로세스의 중요도>`로 짝을 지어서 `queue`에 push
- 중요도가 높은 순서대로 원하므로 `priorities`를 내림차순으로 정렬
- 그 후에 while 루프 안에서 조건에 맞게 체크하면 끝

## 풀이

```C++
#include <string>
#include <vector>
#include <queue>
#include <algorithm>
#include <functional>
#include <iostream>

using namespace std;

int solution(vector<int> priorities, int location) {
    int answer = 0;
    int idx = 0;
    queue<pair<int, int>> q;  // <priorities의 인덱스, 프로세스의 중요도>

    for (int i = 0; i < priorities.size(); i++)
        q.push({ i, priorities[i] });

    sort(priorities.begin(), priorities.end(), greater<int>());

    while (!q.empty()) {
        auto [curIdx, curPrior] = q.front();
        q.pop();

        if (curPrior == priorities[idx]) {
            answer++;
            idx++;
            if (curIdx == location)
                return answer;
        } else {
            q.push({ curIdx, curPrior });
        }
    }
    return answer;
}


```

시간복잡도O(n²)
소요시간: 약 20분

## 막혔던 부분

없었음
