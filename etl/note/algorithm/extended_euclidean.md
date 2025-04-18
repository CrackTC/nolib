# 关于扩展欧几里得

当 $b = 0$ 时

$a$ 和 $b$ 最大公因数为 $a$

$a \times 1 + b \times 0 = gcd(a, b)$

若 $b \ne 0$

假设已经通过扩展欧几里得获得

$b \times y + a \% b \times x = gcd(a, b)$

设 $a = qb + r$

$b \times y + r \times x = gcd(a, b)$

即 $rx = gcd(a, b) - by$

要维护 $a \times x + b \times y' = gcd(a, b)$ 这一循环不变量

要求 $qbx + rx + by' = gcd(a, b)$

$qbx + gcd(a, b) - by + by' = gcd(a, b)$

$qbx - by + by' = 0$

$by' = by - qbx$

$y' = y - qx$

$y' = y - (a / b)x$ （这里是整数除法）
