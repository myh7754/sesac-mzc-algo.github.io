---
status: done
language: javascript
# 블로그 등 풀이 원문이 있으면 아래 주석을 해제해 입력합니다.
# url: https://example.com/solution
---

## 접근

주어진 트럭 길이만큼 반복, 다리에 존재하는 차의 x 값을 1씩 상승 + 차를 더 올릴 수 있으면 올리기
다리에 차가 남아있는 경우 이동 반복

## 풀이

```javascript
function solution(bridge_length, weight, truck_weights) {
    var answer = 0;
 
    const bridge = new Bridge(bridge_length, weight);
    while(truck_weights.length > 0){
        bridge.tick();
        if(bridge.affordable(truck_weights[0])){
            const cur = truck_weights.shift();
            bridge.add(cur);
        }
        answer++;
    }
    
    while(bridge.arr.length > 0){
        answer++;
        bridge.tick();
    }
    
    return answer;
}

class Bridge {
    len;
    limit;
    arr = [];
    total = 0;
    
    constructor(len, limit){
        this.len = len;
        this.limit = limit;
    }
    
    affordable(weight){
        return this.limit >= this.total + weight;
    }
    
    add(v){
        this.arr.push({
            weight :v,
            x: 1
        })
        this.total+=v;
    }
    
    length(){
        return this.arr;
    }
    
    tick(){
        this.arr.forEach(v => v.x++);
        this.arr = this.arr.filter(v => {
            if(v.x <= this.len){
                return true;
            }
            this.total-=v.weight;
            return false;
        });
    }
}
```

시간복잡도 O(N * L) -> 트럭 수 * 다리 길이 -> 모든 트럭이 다리 길이만큼 지나가야함
공간복잡도 O(N)

## 막혔던 부분

