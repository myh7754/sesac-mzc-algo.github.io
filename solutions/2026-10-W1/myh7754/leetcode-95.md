---
status: done
language: java
---

## 접근

1. `1 ~ n`을 값으로 갖는 모든 이진 **탐색** 트리를 만들어 반환하는 문제다. 개수만 세는 게 아니라 트리 자체를 전부 만들어야 한다.
2. BST의 성질을 쓴다. 루트를 `root`로 정하면 **왼쪽 서브트리에는 `root`보다 작은 값만, 오른쪽 서브트리에는 큰 값만** 들어간다. 즉 `root`를 고르는 순간 왼쪽/오른쪽이 완전히 독립된 같은 문제로 쪼개진다.
3. 그래서 "구간 `[start, end]`로 만들 수 있는 모든 트리의 목록"을 반환하는 재귀 함수 `go(start, end)`를 정의한다.
4. `start > end`면 노드를 하나도 못 넣는 경우다. 이때 빈 리스트를 반환하면 조합이 통째로 사라지므로, **`null` 하나를 담은 리스트**를 반환해 "빈 트리도 하나의 경우"로 센다.
5. `start ~ end`의 각 값을 루트로 삼아 `go(start, root-1)`와 `go(root+1, end)`를 구하고, 두 목록의 **모든 조합**을 짝지어 트리를 만든다.

## 풀이

```java
class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;

    TreeNode(int val) {
        this.val = val;
    }
}

class Solution {
    public List<TreeNode> generateTrees(int n) {
        return go(1,n);
    }

    public List<TreeNode> go(int start, int end) {
        List<TreeNode> result = new ArrayList<>();

        // 만들 수 있는 노드가 없을 경우 -> 마지막에 도달한 경우
        if (start > end) {
            result.add(null);
        }

        // start ~ end 중 하나를 루트로 선택
        for (int root = start; root <=end; root++) {
            // 왼쪽 이진트리
            List<TreeNode> leftTrees = go(start, root -1);

            // 오른쪽 이진트리
            List<TreeNode> rightTrees = go(root+1, end);

            for(TreeNode left : leftTrees) {
                for (TreeNode right : rightTrees) {
                    TreeNode node = new TreeNode(root);

                    node.left = left;
                    node.right = right;

                    result.add(node);
                }
            }
        }

        return result;
    }
}
```

트리 개수는 카탈란 수만큼 나온다.

## 막혔던 부분

특별히 막힌 부분 없이 한 번에 풀었습니다.
