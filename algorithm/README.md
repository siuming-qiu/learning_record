## 1. 快速幂
```javascript
// 例如计算7的5次方，可以将5转换为2进制，即101，则可变化为7的4次方乘7的1次方
function qpow(a, n) {
    let res = 1
    while(n) {
        if(n & 1) {
            res *= a
        }
        a *= a
        n >>= 1
    }
    return res
}
```