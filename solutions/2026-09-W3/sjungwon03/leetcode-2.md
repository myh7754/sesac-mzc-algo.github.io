---
status: done
language: javascript
# 블로그 등 풀이 원문이 있으면 아래 주석을 해제해 입력합니다.
# url: https://example.com/solution
---

## 접근

연결리스트로 표현된 뒤집힌 숫자 연산하기, 더 긴 연결리스트 이어서 처리하기
반복하면서 숫자 올림처리

## 풀이

```javascript
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} l1
 * @param {ListNode} l2
 * @return {ListNode}
 */
var addTwoNumbers = function(l1, l2) {
    const head = new ListNode();
    let cur = head;
    let cur1 = l1;
    let cur2 = l2;
    let lead = 0;
    while(cur1 != null){
        const val = (cur1?.val ?? 0) + (cur2?.val ?? 0) + lead;
        cur.next = new ListNode(val % 10);
        lead = Math.floor(val / 10);
        cur = cur.next;

        cur1 = cur1?.next;
        cur2 = cur2?.next;
    }
    while(cur2 != null){
        const val = (cur1?.val ?? 0) + (cur2?.val ?? 0) + lead;
        cur.next = new ListNode(val % 10);
        lead = Math.floor(val / 10);
        cur = cur.next;

        cur1 = cur1?.next;
        cur2 = cur2?.next
    }
    if(lead != 0){
        cur.next = new ListNode(lead);
    }
    return head.next;
};
```

시간복잡도 O(n), 공간복잡도 O(n)

## 막혔던 부분

