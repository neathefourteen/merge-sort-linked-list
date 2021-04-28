# merge-sort-linked-list(https://leetcode.com/problems/sort-list/)
```golang
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func sortList(head *ListNode) *ListNode {
    if head == nil { return nil }
    
    size := 0
    tmp := head
    for tmp != nil {
        size++
        tmp = tmp.Next
    }
    
    return splitAndMerge(head, size)
}


func splitAndMerge(head *ListNode, size int) *ListNode {
    if size == 1 || head == nil { return head }
    
    var left, right *ListNode = head, nil
    leftSize := size / 2
    
    i := 1
    for {
        if i == leftSize {
            right = left.Next
            left.Next = nil
            break
        }
        left = left.Next
        i++
    }
    left = head
    
    left, right = splitAndMerge(left, leftSize), splitAndMerge(right, size - leftSize)
    
    return mergeSortedLists(left, right)
}

func mergeSortedLists(first, second *ListNode) *ListNode {
    if second == nil && first != nil { return first }
    if first == nil && second != nil { return second }
    
    var aggregator, tail *ListNode
    
    for {
        if first == nil && second != nil {
            tail = appendToTail(tail, second)
            if aggregator.Next == nil {
                aggregator.Next = tail
            }
            break
        }
        
        if second == nil && first != nil {
            tail = appendToTail(tail, first)
            if aggregator.Next == nil {
                aggregator.Next = tail
            }
            break
        }
        
        current := findLowerNode(first, second)
        
        if aggregator != nil && tail != nil {
            tail = appendToTail(tail, current)  
        } else if aggregator != nil && tail == nil {
            tail = current
            aggregator.Next = tail
        } else if aggregator == nil { 
            aggregator = current 
        }
        
        if current == first {
            first = first.Next
        } else if current == second {
            second = second.Next
        }
    }
    
    return aggregator
}

func findLowerNode(first, second *ListNode) *ListNode {
    if first.Val < second.Val {
        return first
    }
    
    return second
}

func appendToTail(tail, node *ListNode) *ListNode {
    if tail == nil {
        return node
    }
    tail.Next = node
    return node
}
```
