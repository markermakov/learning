# Learning

https://leetcode.com/problems/valid-parentheses/description/

```scala
object Solution {
    // can not be null
    // only brackets
    // 1 bracket => false

    // решение подсмотрел, сходу не смог решить, судя по алгоритму, создается мапа с открытой и закрытой скобкой
    // если чара нет в ключах мапы, значит это открывающая скобка, закидываем ее в стек,
    // если есть, то если стек пуст или закрывающая скобка этого вида не находится на верхушке стека -> возвращаем false

    // сложность по скорости O(n) по памяти O(n)
    def isValid(s: String): Boolean = {
        import scala.collection.mutable

        val pairs = Map(')' -> '(', '}' -> '{', ']' -> '[')

        val stack = new mutable.Stack[Char](s.length)

        s.foreach {
        case c if pairs.contains(c) =>
            if (stack.isEmpty || pairs(c) != stack.pop()) {
                return false
            }
        case c => stack.push(c)
        }

        stack.isEmpty
    }
}
```

https://leetcode.com/problems/group-anagrams/description/

```scala
object Solution {
    // can not be null
    // min length 1
    // only english letters

    // ["a"] => [["a"]]
    // по алгоритму создам мапу (стринг, список(стринг)), берем каждое слово и сортируем чары в нем
    // если отсортированной стринги нет в мапе => добавляем с пустым списком как ключ
    // проверяем отсортированное слово в мапе и добавляем исходное слово в случае успеха
    
    // по скорости O(n * n log n) вроде
    // по памяти затрудняюсь
    
    def groupAnagrams(arr: Array[String]): List[List[String]] = {
        val anaMap = scala.collection.mutable.HashMap.empty[String, scala.collection.mutable.ArrayBuffer[String]]

        for (word <- arr) {
            val chars: Array[Char] = word.toArray
            val sortedWord: String = chars.sorted.mkString

            if (anaMap.getOrElse(sortedWord, 0) == 0) {
                anaMap += (sortedWord -> scala.collection.mutable.ArrayBuffer.empty[String])
            }

            anaMap.get(sortedWord).get += word
        }
        anaMap.values.toList.map(_.toList)
    }
}
```

https://leetcode.com/problems/maximum-product-of-three-numbers/description/

```scala
object Solution {
    // can not be null

    // nums.length == 3 => _ * _

    // min length 3

    // not sorted

    //[-100, -20, -3, -1, 2, 5]
    //[-8,-7,-2,10,20]
    //[-1, -2, -3, -4]

    // тут просто сортируем список и берем максимальное значение из умножаемых групп с начала и с конца

    // по скорости О(n log n)
    // по памяти тоже самое

    def maximumProduct(nums: Array[Int]): Int = {
        val sorted = nums.sorted
        math.max(sorted.take(2).product * sorted.last, sorted.takeRight(3).product)
    }
}
```

https://leetcode.com/problems/maximum-average-subarray-i/description/

```scala
object Solution {
    // no need to sort? 
    // null ?
    // [1] ; k = 1 -> 1.0000
    // [1] ; k > 1 ? -> ???
    // if k > nums.length -> error
    //
    //[1, 2, 3] k = 2 

    // рекурсия, двигается окном в k по массиву, вычитая первый элемент окна и прибавляя (индекс последнего + 1)
    // затем сравниваем две получившиеся суммы и выбираем одну

    // по скорости O(n)
    // память O(n)

    import scala.math.max

    def findMaxAverage(nums: Array[Int], k: Int): Double = {
        def loop(i: Int, sum: Int, best: Int): Double = {
            if (i == nums.length) best / k.toDouble
            else {
                val newSum = sum - nums(i - k) + nums(i)
                loop(i + 1, newSum, math.max(best, newSum))
            }
        }
        
        val sum = nums.slice(0, k).sum
        
        loop(k, sum, sum)
    }
}
```
https://leetcode.com/problems/subarray-sum-equals-k/description/

```scala
object Solution {
    // min length == 1
    // [1] == 1

    // делаем мапу, проходимся по всем цифрам, если разницы искомой суммы и аккамулятора в мапе нет, то
    // если в ключах есть аккамулятор, то прибавляем 1, если нет -> добавляем новую пару

    // скорость/память O(n)

    def subarraySum(nums: Array[Int], k: Int): Int = {
        val map = scala.collection.mutable.Map(0 -> 1)
        var curr = 0
        var res = 0
        for (num <- nums) {
            curr += num            
            res += map.get(curr - k).getOrElse(0)
            if (map.contains(curr)) 
                map(curr) += 1 
            else 
                map += (curr -> 1)
        }
        res
    }
}
```
