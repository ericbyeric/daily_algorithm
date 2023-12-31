# daily_algorithm

## Leetcode
### Day 1
#### [22. Generate Parentheses](https://github.com/ericbyeric/daily_algorithm/blob/main/daily_algo/src/main/java/leetcode/GenerateParentheses.java)

```java
class Solution {
    // backtracking
    // ~ recursion + termination check
    public List<String> generateParenthesis(int n) {
        List<String> ret = new ArrayList<>();
        process(n, 0, 0, "", ret);
        return ret;
    }

    public void process(int n, int numOfOpened, int numOfClosed, String str, List<String> ret ){
        // termination
        if(numOfOpened==n && numOfClosed==n){
            ret.add(str);
            return;
        }
        // recurse next
        if(numOfOpened < n){
            process(n, numOfOpened+1, numOfClosed, str+"(", ret);  // add open bracket
        }
        if(numOfOpened > numOfClosed){
            process(n, numOfOpened, numOfClosed+1, str+")", ret);  // add closed bracket
        }

    }
}
```

### Day 2
#### [530. Minimum Absolute Difference in BST](https://github.com/ericbyeric/daily_algorithm/blob/main/daily_algo/src/main/java/leetcode/day002/MinimumAbsolutDifferenceInBST.java)
- implementation video : [youtube](https://www.youtube.com/watch?v=utCGIXhbYBU&t=13s&ab_channel=%EA%B3%84%ED%94%BC%EC%B8%84%EB%A1%9C%EC%8A%A4cinnamonchurros)
```java
public class MinimumAbsolutDifferenceInBST {
    // 트리 순회 (재귀)
    // preorder : self left right
    // inorder : left self right
    // postorder : left right self
    // 이진검색트리 성질 -> inorder -> 오름차순 정렬이 됨!
    static boolean init;
    static int prev;
    static int min;
    public static int getMinimumDifference(TreeNode root){
        init = false;
        int min = Integer.MAX_VALUE;
        inorder(root);
        return min;
    }

    public static void inorder(TreeNode root){
        // 예외 처리
        if(root==null) return;

        inorder(root.left);
        //self처리
        if(!init){
            init = true;
        }else{
            min = Math.min(min,root.val - min);
        }
        prev = root.val;
        inorder(root.right);
    }
}
```


### Day 3 
#### [461. Hamming Distance](https://github.com/ericbyeric/daily_algorithm/blob/main/daily_algo/src/main/java/leetcode/day003/HammingDistance.java)
```java
/**
 * Hamming distance:
 *  - bit연산의 기본적
 *  - bit가 다른 자릿수의 개수를 구하는 문제
 *
 * 문제 접근:
 *  - 두 수에 XOR를 취해서 1이 몇번 등장하는지 각 자릿수 만큼 체크해서 해당 count를 반환
 */
public class HammingDistance {

    public int hammingDistance(int x, int y){
        /*
        비트 연산의 기본
        a = 3 -> 이진수 11
        b = 2 -> 이진수 10

        a & b = 10 ; 두 비트가 모두 1이면 1, 아니면 0
        a | b = 11 ; 두 비트중 하나라도 1이면 1, 아니면 0
        a ^ b = 01 ; 두 비트가 다르면 1, 아니면 0


        Shift 연산
        a >> i ; a의 모든 비트를 오른쪽으로 i만큼 밀고, 맨 왼쪽은 0으로 채움
        a << 1 ; a의 모든 비트를 왼쪽으로 i만큼 밀고, 맨 오른쪽은 0으로 채움

        a >> 1 = 01 ;
        a << 1 = 10 ;
         */
        int xor = x ^ y;
        int cnt = 0;
        // int는 4byte - 32bit
        for(int i = 0; i<32; i++){
            cnt +=  ( xor >> i ) & 1;
        }
        return cnt;
    }
}
```
#### [21. Merge Two Sorted Lists](https://github.com/ericbyeric/daily_algorithm/blob/main/daily_algo/src/main/java/leetcode/day003/MergeTwoSortedLists.java)
```java
public class MergeTwoSortedLists {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2){
        ListNode ret = null;
        ListNode cur = null;

        while(l1 != nul l || l2 != null){
            if(l2==null || (l1 !=null && l1.val < l2.val)){
                // pick from l1
                if(ret == null){
                    ret = l1;
                    cur = l1;
                    l1 = l1.next;
                }else{
                    // cur: 1, l1 = 2 -> 4
                    // 1 -> 2
                    // cur: 2
                    // l1 : 4
                    cur.next = l1;
                    cur = l1;
                    l1 = l1.next;
                }
            }else{
                // pick from l2
                if(ret == null){
                    ret = l2;
                    cur = l2;
                    l2 = l2.next;
                }else{
                    cur.next = l2;
                    cur = l2;
                    l2 = l2.next;
                }
            }
        }
        return ret;
    }
}
```

### Day 4
```java
/**
 * 짝 맞추기
 * 대표적은 Stack 문제
 *
 * 3 types of brackets (total 6 brackets)
 * 주어진 input에서 charByChar로 for문을 돌면서 쌍을 비교
 */
public class ValidParentheses {
    public boolean isValid(String s){
        Stack<Character> stk = new Stack<>();
        char[] arr = s.toCharArray();
        for(char c:arr){
            // open bracket check
            if(c == '(' || c=='{' || c=='['){
                stk.push(c);
            }else if(c==')'){
                // stack의 쌍 비교
                if(stk.size() == 0 || stk.pop() !='(')
                    return false;
            }else if(c=='}'){
                // stack의 쌍 비교
                if(stk.size() == 0 || stk.pop() !='{')
                    return false;
            }else if(c==']'){
                // stack의 쌍 비교
                if(stk.size() == 0 || stk.pop() !='[')
                    return false;
            }else{
                    return false;
            }
//            if(stk.size() != 0){
//                return false;
//            }
        }
//        return true;
        return stk.size() == 0;
    }
}
```

### Day 5
```java
/**
 * Dynamic Programming
 *
 * 1 2 3 5 8 : 피보나치 수열
 * f(n) = f(n-1)+ f(n-2)
 * f(5) = f(4)+f(3)
 * f(4) = f(3)+f(2)
 * 일반 재귀구현: 공간 복잡도 O(1), 시간 복잡도 O(2^n)
 * f(2) = 2
 * f(3) = 3
 * 다이나믹 프로그래밍: 공간 복잡도 O(n), 시간 복잡도 O(n)
 * 점화식 찾기
 */
public class ClimbingStairs {
    public int climbStairs(int n){
        // 예외처리
        if(n <= 0 ) return 0;
        if(n == 1) return 1;

        int[] d = new int[n+1];
        // 초기값
        d[1] = 1;
        d[2] = 2;
        for(int i = 3; i <= n; i++){
            d[i] = d[i-1] + d[i-2];  // 점화식
        }

        return d[n];

    }
}

```