---
status: done
language: cpp
# 블로그 등 풀이 원문이 있으면 아래 주석을 해제해 입력합니다.
# url: https://example.com/solution
---

## 문제 설명
- 각 기능은 진도가 100%일 때 큐에서 pop
- 각 기능의 개발 속도는 서로 다르므로, 뒤에 있는 기능이 앞 기능보다 먼저 개발 가능
- 앞 기능이 배포될 때 뒤의 완료된 기능들도 순차적으로 배포 가능

## 접근
- 당연히 `queue`를 사용. `<progresses의 인덱스, progresseses의 값>`을 짝을 지어서 `queue`에 push
- 1일 마다 계산을 위해 while문 안에서 for문을 통해 계산
- 최종적으로 하루에 배포된 값을 `temp`에 담아서 `answer`에 push_back

## 풀이

```C++
#include <string>
#include <vector>
#include <queue>

using namespace std;

vector<int> solution(vector<int> progresses, vector<int> speeds) {
    vector<int> answer;
    queue<pair<int,int>> q;
    int completed = 0;
    
    for(int i=0;i<progresses.size();i++){
        q.push({i, progresses[i]});    
    }
    
     while(!q.empty()){
         int temp = 0;
         int n = q.size();
         
         for(int i = 0; i < n;i++){
            auto[idx, progress] = q.front();
         
            q.pop();
            progress += speeds[idx];
         
            if(progress >= 100 && idx == completed){
                temp++;
                completed++;
            } else {
                q.push({idx, progress});
            }     
         }
         
         if(temp != 0)
             answer.push_back(temp);
         
     }
    
    return answer;
}

```

## 개선된 풀이(AI)

```C++
#include <string>
#include <vector>
#include <queue>

using namespace std;

vector<int> solution(vector<int> progresses, vector<int> speeds) {
    vector<int> answer;
    queue<int> q;

    // 각 기능이 완료되기까지 걸리는 일수
    for (int i = 0; i < progresses.size(); i++) {
        int days = (100 - progresses[i] + speeds[i] - 1) / speeds[i]; // 올림 나눗셈
        q.push(days);
    }

    while (!q.empty()) {
        int front = q.front();   // 배포 기준이 되는 일수
        q.pop();
        int count = 1;

        // front 이하 일수의 뒤 작업들은 같이 배포
        while (!q.empty() && q.front() <= front) {
            q.pop();
            count++;
        }
        answer.push_back(count);
    }

    return answer;
}

```
- sol1 : 시간복잡도O(nk) k: 완료일수, n: progresses
- sol2 : 시간복잡도O(n)
소요시간: 약 20분

## 막혔던 부분

하루의 계산을 위한 for문의 조건문을 q.size()로 하였더니
for문의 길이가 가변되어서 그 부분에서 시간을 좀 사용했다.
