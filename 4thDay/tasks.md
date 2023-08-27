# Learning

4th Batch

https://leetcode.com/problems/top-k-frequent-words/

```scala
object Solution {
    // null?
    // [] k = 0
    // по алгоритму подсчитываю в мапу кол-во встречающихся строк
    // потом преобразовываю ее с помощью мапов в искомый результат, вроде шустро работает
    // скорость O(n * n log n)

    import scala.collection.mutable
    import scala.collection.mutable.ArrayBuffer

    def topKFrequent(words: Array[String], k: Int): List[String] = {
        val map: mutable.Map[String, Int] = mutable.Map()

        for (w <- words) {
            map.get(w) match {
                case Some(i) => map(w) += 1
                case None => map += (w -> 1)
            }
        }

        map.groupBy(_._2)
            .map(_._2.toArray)
            .map{ x => if (x.length > 1) x.sortBy(_._1)(Ordering[String].reverse) else x }
            .toList
            .sortBy(x => x(0)._2)
            .map(_.flatMap(x => List(x._1)))
            .flatten
            .takeRight(k)
            .reverse
    }
}
```

https://leetcode.com/problems/intersection-of-two-arrays-ii/

```scala
object Solution {
    // nums.length >= 1
    // only digits
    // not null

    // 2 мапы сгрупированных по кол-ву повторений элементов через группировку
    // прогоняю через ключи одной из них чтобы найти пересечение

    // Скорость O(n)
    import scala.collection.mutable.ListBuffer
    import scala.math

    def intersect(nums1: Array[Int], nums2: Array[Int]): Array[Int] = {
        val map1 = nums1.groupBy(i => i)
        val map2 = nums2.groupBy(i => i)

        val result: ListBuffer[Array[Int]] = ListBuffer()

        for (k <- map1.keys) {
            map2.get(k) match {
                case Some(i) => {
                    val arr: Array[Int] = if (map2(k).length > map1(k).length) map1(k).take(map1(k).length) else map2(k).take(map2(k).length)
                    result += arr
                }
                case None => 
            }
        }

        result.flatten.toArray
    }
}
```

https://leetcode.com/problems/valid-palindrome-ii/

```scala
object Solution {
    // s.length >= 1
    // only lowercase english letters
    // not null

    // две рекурсии, одна сравнивает элементы в случае нахождения лишнего элемента
    // другая основная
    // скорость O(n)

    def validPalindrome(s: String): Boolean = {

        def isValid(i: Int, j: Int): Boolean = {
            if (i > j) true
            else if (s(i) != s(j)) false
            else isValid(i + 1, j - 1)
        }

        def aux(i: Int, j: Int): Boolean = {
            if (i > j) true
            else if (s(i) != s(j)) isValid(i + 1, j) || isValid(i, j - 1)
            else aux(i + 1, j - 1)
        }
    
        aux(0, s.length - 1)
    }
}
```
