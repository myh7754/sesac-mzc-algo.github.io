---
status: done
language: java
# 블로그 등 풀이 원문이 있으면 아래 주석을 해제해 입력합니다.
# url: https://example.com/solution
---

## 문제 설명
- nums 의 배열을 순회하며 , target의 값보다 nums[i] 가 크거나 같아질 때 해당 index를 return
- target 이 nums의 가장 끝 숫자보다 크다면 해당 위치 점유하여 return

## 접근


## 풀이

``` java

class Solution {
    public int searchInsert(int[] nums, int target) {
        int answer = 0;
        for(int i = 0; i<nums.length ; i++){
            if(nums[i]>=target){
                answer = i;
                break;
            }

            if(nums[nums.length-1]<target){
                answer = nums.length;
            }
        }
            return answer;
    }
}
```

시간복잡도O(n) , 공간복잡도 O(1)
소요시간: 약 10분

## 막혔던 부분

해당 문제를 O(log n) 으로 풀려면 이분 탐색을 써야 하는데 , 해당 부분은 아직 공부가 안되어서 패스하였음.
