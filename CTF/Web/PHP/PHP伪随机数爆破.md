`mt_rand`生成的伪随机数可通过`php_mt_seed`爆破种子
参见 [[[GWCTF 2019]枯燥的抽奖]]

接受四个一组的参数，分别是：
- `mt_rand`实际生成的随机数的最小可能值
- `mt_rand`实际生成的随机数的最大可能值
- 向`mt_rand`传递的第一个参数（理论下界）
- 向`mt_rand`传递的第二个参数（理论上界）

#Web #PHP #Vulnerabilities #伪随机数 #mt_rand 