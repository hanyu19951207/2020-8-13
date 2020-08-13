# 2020-8-13
19. 删除链表的倒数第N个节点
    给定一个链表，删除链表的倒数第 n 个节点，并且返回链表的头结点。
    示例：
    给定一个链表: 1->2->3->4->5, 和 n = 2.
    当删除了倒数第二个节点后，链表变为 1->2->3->5.
思路：
    首先我们将添加一个哑结点作为辅助，该结点位于列表头部。哑结点用来简化某些极端情况，例如列表中只含有一个结点，或需要删除列表的头部。
    我们可以使用两个指针而不是一个指针。第一个指针从列表的开头向前移动 n+1 步，而第二个指针将从列表的开头出发。
    现在，这两个指针被 n 个结点分开。我们通过同时移动两个指针向前来保持这个恒定的间隔，直到第一个指针到达最后一个结点。
    此时第二个指针将指向从最后一个结点数起的第 n 个结点。我们重新链接第二个指针所引用的结点的 next 指针指向该结点的下下个结点。
题解：
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode first = dummy;
        ListNode second = dummy;
        for(int i = 1;i <= n + 1;i++){
            first = first.next;
        }
        while(first != null){
            first = first.next;
            second = second.next;
        }
        second.next = second.next.next;
        return dummy.next;
    }
}

22. 括号生成
    数字 n 代表生成括号的对数，请你设计一个函数，用于能够生成所有可能的并且 有效的 括号组合。
    示例：
    输入：n = 3
    输出：[
           "((()))",
           "(()())",
           "(())()",
           "()(())",
           "()()()"
         ]
思路：
    我们可以只在序列仍然保持有效时才添加 '(' or ')'，而不是像 方法一 那样每次添加。我们可以通过跟踪到目前为止放置的左括号和右括号的数目来做到这一点，
    如果左括号数量不大于 nnn，我们可以放一个左括号。如果右括号数量小于左括号的数量，我们可以放一个右括号。
题解：
class Solution {
    public List<String> generateParenthesis(int n) {
        List<String> ans = new ArrayList();
        backtrack(ans, new StringBuilder(), 0, 0, n);
        return ans;
    }

    public void backtrack(List<String> ans, StringBuilder cur, int open ,int close, int max){
        if(cur.length() == max * 2){
            ans.add(cur.toString());
            return;
        }
        if(open < max){
            cur.append('(');
            backtrack(ans, cur, open + 1, close, max);
            //回溯上一步，此位置有可能放右括号
            cur.deleteCharAt(cur.length() - 1);
        }
        if(close < open){
            cur.append(')');
            backtrack(ans, cur, open, close + 1, max);
            //回溯上一步，此位置有可能放左括号
            cur.deleteCharAt(cur.length() - 1);
        }
    }
}

504. 七进制数
    给定一个整数，将其转化为7进制，并以字符串形式输出。
    示例 1:
    输入: 100
    输出: "202"
    示例 2:
    输入: -7
    输出: "-10"
思路：
    迭代除7取余，存到一个StringBuilder里，最后反转即可。
    对于负数，直接先取正处理，然后加上负号即可。
题解：
class Solution {
    public String convertToBase7(int num) {
        String ans = "";
        if (num < 0) {
            num = 0 - num;
            ans += "-";
        };
        StringBuilder sb = new StringBuilder();
        do {
            int mod = num % 7;
            sb.append((char)('0'+mod));
            num = num / 7;
        } while (num > 0);
        ans += sb.reverse().toString();
        return ans;
    }
}

506. 相对名次
        给出 N 名运动员的成绩，找出他们的相对名次并授予前三名对应的奖牌。前三名运动员将会被分别授予 “金牌”，“银牌” 和“ 铜牌”（"Gold Medal", "Silver Medal", "Bronze Medal"）。
        (注：分数越高的选手，排名越靠前。)
        示例 1:
        输入: [5, 4, 3, 2, 1]
        输出: ["Gold Medal", "Silver Medal", "Bronze Medal", "4", "5"]
        解释: 前三名运动员的成绩为前三高的，因此将会分别被授予 “金牌”，“银牌”和“铜牌” ("Gold Medal", "Silver Medal" and "Bronze Medal").
        余下的两名运动员，我们只需要通过他们的成绩计算将其相对名次即可。
思路：
        1.排序，并且用hashmap来记录成绩数组的名次
        2.遍历赋值
题解：
class Solution {
    public String[] findRelativeRanks(int[] nums) {
        int len = nums.length;
        int [] temp = nums.clone();
        Arrays.sort(temp);
        Map<Integer,Integer> map = new HashMap<>(nums.length);
        for(int i = 0;i < len;i++){
            map.put(temp[i],i);
        }
        String [] ans = new String[len];
        for(int i = 0;i < len;i++){
            int index = map.get(nums[i]);
            if(index == len - 1)
                ans[i] = "Gold Medal";
            else if(index == len - 2)
                ans[i] = "Silver Medal";
            else if(index == len - 3)
                ans[i] = "Bronze Medal";
            else {
                ans[i] = String.valueOf(len - index);
            }
        }
        return ans;
    }
}

509. 斐波那契数
    斐波那契数，通常用 F(n) 表示，形成的序列称为斐波那契数列。该数列由 0 和 1 开始，后面的每一项数字都是前面两项数字的和。也就是：
    F(0) = 0,   F(1) = 1
    F(N) = F(N - 1) + F(N - 2), 其中 N > 1.
    给定 N，计算 F(N)。
    示例 1：
    输入：2
    输出：1
    解释：F(2) = F(1) + F(0) = 1 + 0 = 1.
思路：
    检查整数 N，如果 N 小于等于 1，则返回 N。
    否则，通过递归关系：Fn=Fn−1+Fn−2F_{n} = F_{n-1} + F_{n-2}Fn​=Fn−1​+Fn−2​ 调用自身。
    直到所有计算返回结果得到答案。
题解：
class Solution {
    public int fib(int N) {
        if(N <= 1)
            return N;
        return fib(N - 1) + fib(N - 2);
    }
}
