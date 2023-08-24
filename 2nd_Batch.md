# Вторая пачка задач
# Learning

Algorithms and stuff

https://leetcode.com/problems/squares-of-a-sorted-array

```scala
import scala.math.abs
    
    def sortedSquares(nums: Array[Int]): Array[Int] = {
        def loop(x1: Int, x2: Int, acc: List[Int]): List[Int] = {
            if (x1 > x2) acc
            else {
                val left = abs(nums(x1))
                val right = abs(nums(x2))
                left > right match {
                    case true => loop(x1 + 1, x2, (left * left) +: acc)
                    case _ => loop(x1, x2 - 1, (right * right) +: acc)
                }
            }
        }
        loop(0, nums.length - 1, Nil).toArray
    }
```

https://leetcode.com/problems/reverse-linked-list/

```scala
def reverseList(head: ListNode): ListNode = {
        def loop(head: ListNode, result: ListNode = null): ListNode = {
            head match {
                case null => result
                case h => {
                    val current = h.next
                    h.next = result
                    loop(current, h)
                }
            }
        }
        loop(head)
    }
```


