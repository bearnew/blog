# js双精度浮点数
1. 数字存储
    * 第0位：符号位，0表示正数，1表示负数(s)
    * 第1位到第11位：储存指数部分（e）
    * 第12位到第63位：储存小数部分（即有效数字）f
2. js最大安全数是 Number.MAX_SAFE_INTEGER == Math.pow(2,53) - 1, 而不是Math.pow(2,52) - 1
    * 二进制表示有效数字总是1.xx…xx的形式，尾数部分f在规约形式下第一位默认为1（省略不写，xx..xx为尾数部分f，最长52位）。因此，JavaScript提供的有效数字最长为53个二进制位（64位浮点的后52位+被省略的1位）
3. 进制转换
```js
0.1 -> 0.0001100110011001...(无限循环)
0.2 -> 0.0011001100110011...(无限循环)
```
4. 为什么 x=0.1 能得到 0.1
```js
0.10000000000000000555.toPrecision(16)
// 返回 0.1000000000000000，去掉末尾的零后正好为 0.1

// 但来一个更高的精度：
0.1.toPrecision(21) = 0.100000000000000005551
```
