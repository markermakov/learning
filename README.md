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
    import scala.collection.mutable.ArrayBuffer

    def isPalindrome(s: String): Boolean = {
        val alphabet = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ"
        val numbers = "0123456789"
        val arr: ArrayBuffer[(Char, Char)] = ArrayBuffer()

        for (i <- alphabet.indices) arr += Tuple2(alphabet(i), if (i >= 26) alphabet(i - 26) else alphabet(i))

        val charMap = (arr ++ numbers.map(x => (x, x))).toMap

        var newString: ArrayBuffer[Char] = ArrayBuffer()
        for (i <- 0 to s.length - 1) {
            charMap.get(s(i)) match {
                case Some(_) => newString += charMap(s(i))
                case None =>
            }
        }
        
	    var check: Boolean = true
	    val nums: Int = if (newString.length % 2 == 0) (newString.length / 2) - 1 else newString.length / 2

	    for (i <- 0 to nums) {
            if (newString(i) != newString(newString.length - i - 1)) check = false
	    }
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
    import scala.collection.mutable.ArrayBuffer

    def summaryRanges(nums: Array[Int]): List[String] = {
        
        var arr: ArrayBuffer[(Int, Int)] = ArrayBuffer()
        if (nums.length == 1) return nums.map(_.toString).toList
        else if (nums.isEmpty) return List()

        var max: Int = nums.head
        var min: Int = nums.head

         for (i <- 0 to nums.length - 2) {
            if (nums(i + 1) - nums(i) == 1) {
                max = nums(i + 1)
                if (i == nums.length - 2) arr += (min -> max)
            } else {
            arr += (min -> max)
            min = nums(i + 1)
            max = nums(i + 1)
            if (i == nums.length - 2) arr += (max -> max)
        }
    }
        arr.map(x => if (x._1 == x._2) s"${x._1}" else s"${x._1}->${x._2}").toList
  }
```

https://leetcode.com/problems/two-sum/

```scala
    import scala.collection.mutable.HashMap
    
    def twoSum(nums: Array[Int], target: Int): Array[Int] = {
	val nmap: HashMap[Int, Int] = HashMap()

        for (i <- nums.indices) {
		nmap.get(target - nums(i)) match {
		case Some(n) => return Array(i, n)
		case None => nmap += (nums(i) -> i)
		}
	Array(-1) // в случае провала
    }
```
