[剑指 Offer 24. 反转链表](https://leetcode-cn.com/problems/fan-zhuan-lian-biao-lcof/)
```
剑指 Offer 24. 反转链表
定义一个函数，输入一个链表的头节点，反转该链表并输出反转后链表的头节点。

示例:
输入: 1->2->3->4->5->NULL
输出: 5->4->3->2->1->NULL
```

```
遍历反转解题思路：

遍历反转 1 2 3 4 5 nil
从head为1开始进行遍历：
1、定义一个prev变量保存当前节点前一个节点信息，prev=nil
2、定义curr变量表示当前节点开始进行遍历，curr=1
3、定义next保存当前节点的下一个节点信息，next=2
4、将当前节点的下一个节点信息置为prev变量，curr.Next=nil
5、将当前节点curr赋给prev变量，在下一轮遍历时使用，prev=1，下次遍历curr=2时会赋给curr.Next
6、将第3步保存的next变量赋给curr变量，进行下一轮遍历，curr=2
```

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */

type ListNode struct {
	Val  int
	Next *ListNode
}

// 第一种方法：直接遍历进行链表反转，简单易懂
func reverseList(head *ListNode) *ListNode {
	var prev *ListNode
	curr := head
	for curr != nil {
		next := curr.Next // 保存当前节点的下一个节点
		curr.Next = prev  // 将当前节点的下一个节点置为前一个节点
		prev = curr       // 将前一个节点置为当前节点，以便下一次遍历进行赋值
		curr = next       // 将当前节点赋为next，进行下次遍历
	}
	return prev
}

// 第二种方法，使用递归，比较难理解
func reverseList2(head *ListNode) *ListNode {
	// 递归只考虑一个节点的问题就行，别想那么多
	// 比如 1 2 3
	// 此时head = node(1)
	// head.next = node(2)
	// 怎么反转呢？
	// head.next.next = head // 等于是node(2).next = node(1)
	// head.next = nil 即可

	// 结束条件 一个节点或者没节点直接返回
	if head == nil || head.Next == nil {
		return head
	}
	// 1 2
	newHead := reverseList2(head.Next)
	// 2.Next = 1
	head.Next.Next = head
	// 1.Next = nil
	head.Next = nil
	// 返回最后的头节点
	return newHead
}
```