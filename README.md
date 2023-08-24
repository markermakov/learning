# Learning
Algorithms and stuff

https://leetcode.com/problems/move-zeroes/

Костыльная хвостовая рекурсия, в задаче прикол изменить исключительно поступающий на вход список, осилил не до конца, так как метод просто выводит в консоль обновленный список

```scala
@tailrec
    def moveZeroes(nums: Array[Int], n: Int = 0, flag: Boolean = false): Unit = {
      if (nums(n) != 0) println(nums.mkString("Array(", ", ", ")"))
      else moveZeroes(if (!flag) nums.sorted.drop(n + 1) :+ 0 else nums.drop(n + 1) :+ 0, flag = true)
    }
```

https://leetcode.com/problems/valid-palindrome/

```scala
def isPalindrome(s: String): Boolean = {
  val str = s.toLowerCase.toCharArray.filter(_.isLetterOrDigit)
      val nums: Int = if (str.length % 2 == 0) (str.length / 2) - 1 else str.length / 2
      var check: Boolean = true
      
      for (i <- 0 to nums) if (str(i) != str(str.length - i - 1)) check = false
      check
    }
```

https://leetcode.com/problems/string-compression/

```scala
    import scala.collection.mutable
    import scala.collection.mutable.ArrayBuffer

    def extract(s: String): Array[Char] = s.head +: s.tail.toCharArray

    def compress(chars: Array[Char]): Int = {
        def loop(str: String, n: Int = 0, k: Int = 1, acc: mutable.ArrayBuffer[(Char, Int)] = ArrayBuffer()): mutable.ArrayBuffer[(Char, Int)] = {
            if (n == str.length - 1) acc :+ (str(n), k)
            else if (str(n) != str(n + 1)) loop(str, n + 1, 1, acc :+ (str(n), k))
            else loop(str, n + 1, k + 1, acc)
        }

        val seq = loop(chars.mkString("")).flatMap{x: (Char, Int) => if (x._2 > 1) List(extract(x._1.toString + x._2.toString): _*) else List(x._1) }

        for (i <- 0 to seq.length - 1) chars(i) = seq(i)
        chars.take(seq.length)
        seq.length
    }
```

https://leetcode.com/problems/summary-ranges/

```scala
import scala.collection.mutable

    def summaryRanges(nums: Array[Int]): List[String] = {
        val rmap: mutable.HashMap[Int, (Int, Int)] = mutable.HashMap()

        if (nums.length == 1) return nums.map(_.toString).toList
        else if (nums.isEmpty) return List()

        var max: Int = 0
        var min: Int = nums.head

        for (i <- 0 to nums.length - 2) {
            if (nums(i + 1) - nums(i) == 1) {
                max = nums(i + 1)
            }
            else {
                if (i == 0) rmap += (min -> (min -> min))
                min = nums(i + 1)
                max = nums(i + 1)
                rmap += (min -> (min -> max))
            }
            rmap += (min -> (min -> max))
        }
        rmap.toSeq.sortBy(_._1).map(x => if (x._1 >= x._2._2) s"${x._1}" else s"${x._1}->${x._2._2}").toList
  }
```

https://leetcode.com/problems/two-sum/

Тут уже силы были на исходе, по факту немного халтура, но работает

```scala
import scala.collection.mutable.ArrayBuffer
    
    def twoSum(nums: Array[Int], target: Int): Array[Int] = {        
        for (i <- nums.indices) {
            if (nums.indexOf(target - nums(i)) != -1 && nums.indexOf(target - nums(i)) != i) return Array(nums.indexOf(target - nums(i)), i)
        }
    Array(-1)
    }
```
