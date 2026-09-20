---
status: done
language: cpp
# 블로그 등 풀이 원문이 있으면 아래 주석을 해제해 입력합니다.
# url: https://example.com/solution
---

## 문제 설명
- 정수가 든 배열이 주어졌을 때, 거기서 부분의 합이 제일 큰 값을 찾아서 반환

## 접근
- 전 항들의 계산 값들을 저장해야할 필요성이 보였기에 dp로 생각하고 풀이 진행
- 각 항까지의 부분합 저장을 위한 `vector<int> dp` 선언
- 각 항에서는 `nums[i]`와  `dp[i-1] + nums[i]`의 값들을 비교해서 여태까지의 합을 버릴지 자신을 더해서 저장할 지 비교
- 최종적으로 ans에 최댓값들을 비교해서 저장하여 return

## 풀이

```C++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        vector<int> dp(nums.size(), 0);
        int ans = nums[0];

        dp[0] = nums[0];
        if(nums.size() == 1) return nums[0];

        for(int i=1;i<nums.size();i++){
            dp[i] = max(dp[i-1]+nums[i], nums[i]);
            ans = max(ans, dp[i]);
        }

        return ans;
    }
};

```

시간복잡도O(n)
소요시간: 약 15분

## 막혔던 부분

처음 생각은 각 인덱스에 0부터 인덱스까지의 합을 넣고 거기서 항들을 빼서 최댓값을 뽑아내려고 생각했었다.
그렇게 해보려니 규칙이 보이지 않아서 다른 이런저런 생각을 해서 시간을 조금 낭비했다.
