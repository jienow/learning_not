### 高阶函数
- 定义：haskell中的函数可以作为参数和返回值被传来传去，被称为高阶函数。
#### 柯里函数
- 所有的函数都只有一个参数
```
divideByTen :: (Floating a) => a -> a 
divideByTen = (/10)
```
- `(/10) 200，和200 / 10等价`
#### takeWhile函数
- 定义：接受一个条件和一个list，如果遍历list里面的每一个，如果为true，就返回，如果false，就停止遍历。返回的元素重新组成一个list
    ```
    takeWhile (/=' ') "elephants know how to party"，回传 "elephants"
    ```
    ```
    要求所有小于 10000 的奇数的平方的和
    ghci> sum (takeWhile (<10000) (filter odd (map (^2) [1..])))  
    166650
    ```
#### lambda
- 在haskell里面， `\` 代表了lambda的开始。
  ```
  两者的等价
  map (\x -> x+3) [1,6,3,2]
  map (+3) [1,6,3,2]
  ```
#### 