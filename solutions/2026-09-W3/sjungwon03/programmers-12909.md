---
status: done
language: javascript
# 블로그 등 풀이 원문이 있으면 아래 주석을 해제해 입력합니다.
# url: https://example.com/solution
---

## 접근

여는 괄호 `(`를 스택에 쌓고 `)` 닫는 괄호가 나오면 스택을 확인, 없으면 false 처리

## 풀이

```javascript
function solution(s){
    var answer = true;
    
    const stack = [];
    const splited = s.split("");
    for(const c of splited){
        if(c == "("){
            stack.push(c);
            continue;
        }
        if(stack.length == 0){
            answer = false;
            break;
        }    
        stack.pop();
    }
    
    if(stack.length > 0){
        answer = false;
    }

    return answer;
}
```

시간복잡도 O(n), 공간복잡도 O(n)

## 막혔던 부분

