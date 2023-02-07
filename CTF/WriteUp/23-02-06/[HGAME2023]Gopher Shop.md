![[Pasted image 20230207090409.png|500]]

```
/
```

![[Pasted image 20230207090935.png|400]]

注册并登录

![[Pasted image 20230207093629.png|1000]]

```go
func BuyProduct(context *gin.Context) {
	// ...
	
	//校验是否买的起
	if err != nil || number < 1 || user.Balance < uint(number) * price{
		context.JSON(400, gin.H{"error": "invalid request"})
		return
	}
	
	user.Days -= 1
	user.Inventory -= uint(number)
	user.Balance -= uint(number) * price
	
	// ...
}
```

由于交易过程中并未对库存数做任何校验，因此可通过整数溢出来购买大量的苹果

购买$1844674407370955162$个苹果，`uint(number) * price`的算术结果为$18446744073709551620$，`uint`在64位系统中最大值为$18446744073709551615$，因而发生上溢，结果为$18446744073709551620\ mod\ 2^{64}=4\le10$

![[Pasted image 20230207100009.png|700]]

由于卖出的逻辑中也存在整数溢出，因此卖出足够买Flag的量即可

![[Pasted image 20230207100244.png|700]]

#Web #go #overflow #代码审计