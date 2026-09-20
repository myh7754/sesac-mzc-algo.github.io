---
status: done
language: python
# 블로그 등 풀이 원문이 있으면 아래 주석을 해제해 입력합니다.
# url: https://example.com/solution
---

## 접근
주어진 string에는 숫자, '[', ']', substring 총 네 종류가 포함됨.
문자열을을 순회하면서, bracket이 나올 경우, bracket 앞에 붙은 배수(숫자)와 bracket안에 있는 문자열을 곱셈함.
다중 연산이 포함될 수도 있음.
예를 들어 3[a]이면 'aaa', 3[a2[b]]이면 'abbabbabb' 반환

문자열을 순회하며, 총 4가지 경우를 분기함.
1. 숫자
  숫자가 나올 경우, 배수가 두자릿 수일 경우를 대비해서, 기존의 배수가 있다면 10을 곱한 후 다음 숫자와 더해 새로운 배수를 만듦.
  기존의 배수가 없다면 배수를 currVal에 저장.
2. '['
  bracket이 시작될 경우, bracket 앞 배수와, 현재까지의 substring을 tuple(currVal, currStr)로 스택에 저장.
3. ']'
  bracket이 종료될 경우, stack에 저장된 배수(prevVal)와 이전 문자열(prevStr)을 현재 문자열(currStr)에 적용시킴.
4. substring
  일반 character일 경우, currStr += 현재 substring

Runtime: 100% / Memory: 79.71%

## 풀이

```python
class Solution:
  def decodeString(self, s: str) -> str:
        currVal = 0
        currStr = ''
        stack = []

        for elem in s:
            if (elem.isdigit()):
                currVal = currVal * 10 + int(elem)
            elif (elem == '['):
                stack.append((currVal, currStr))
                currVal = 0
                currStr = ''
            elif (elem == ']'):
                prevVal, prevStr = stack.pop()
                currStr = prevStr + currStr * prevVal
            else:
                currStr += elem
        return currStr

  # Try_2
  # def decodeString(self, s: str) -> str:
  #   res, tmpStr = "", ""
  #   stack = []
  #   prevValue, currValue = 0, 0

  #   for elem in s:
  #     if (elem.isdigit()): # elem이 숫자면
  #       if (prevValue != 0): # 연속되는 숫자 문자열이면
  #         currValue = prevValue * 10 + int(elem)
  #       else:
  #         prevValue = int(elem)
  #         currValue = prevValue
  #     elif (elem == '['):
  #       stack.append('[')
  #     elif (elem == ']'):
  #       stack.pop()
  #       if (len(stack) == 0): # 모든 스택이 비워졌으면
  #         res += tmpStr * currValue
  #         currValue = 0
  #       else: # 중첩 []문자열이면
  #         tmpStr += tmpStr * currValue
  #     else: # elem이 문자열이면
  #       tmpStr += elem
  #   return res

  # Try_1
  # def decodeString(self, s: str) -> str:
  #     res = "", tmpStr = ""
  #     stack = []
  #     tmpVar = 1, idx = 0

  #     while True:
  #         if (idx == len(s) - 1):
  #             return res
  #         if (int(s[idx]).isdigit()): # s안의 원소가 숫자면
  #             tmpVar = int(s[idx])
  #             idx += 2
  #             stack.append('[')

  #             while (len(stack) > 0):
  #                 if (s[idx] == '['): # 다중 리스트
  #                     stack.append('[')
  #                     idx += 1
  #                 elif (s[idx] == [']']):
  #                     stack.pop()
  #                     idx += 1
  #                     res += (tmpStr) * tmpVar
  #                 elif (int(s[idx]).isdigit()):
  #                     # 재귀??
  #                 else:
  #                     tmpStr += s[idx]
  #                     idx += 1
  #         else:
  #             res += s[idx]
  #             idx += 1
      
  #     return res

```

시간복잡도: O(n)

## 막혔던 부분
stack에 '[', ']'만 저장해서 stack이 비었을 때 답을 도출하는 방식을 택했는데, 배수가 여러개인 문자열에서 배수를 따로 저장해야돼서 문제가 안풀림. tuple을 이용해서 pop되는 값을 빼내서 사용할 수 있다는 것을 알게돼서 적용시킴.
