Python

# *星号的使用

- ## 参数中出现

  *args  代表参数是元组

  ​	fn(*args)      fn(1,2,3)

  **args 代表参数是字典

  ​	fn(**args)  fn(name='lafeier',age=18)

- ## 函数调用的时候出现，表示解压参数

​        解压元组

  ​	a = (1,2,3)   

  ​	 fn(*a) = fn(1,2,3)

​        解压字典

  ​	a = {name:"lafeier",age:18} 

  ​	 fn(**a) = fn(name="lafeier",age=18)