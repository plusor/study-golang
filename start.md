# 特性
===

* 自动垃圾回收
* 更丰富的内置类型
* 函数多返回值

		func GetPhone()(brands, version string){
		    return "iPhone", "7"
		}
		// 等价于
		func GetPhone()(brands, version string){
			brands 	= "iPhone"
			version = "7"
		    return
		}
		
		    brands, version := GetPhone()      //调用赋值
		    _, version := GetPhone() //缺省调用

* 错误/异常处理

      defer conn.Close()

* 匿名函数和闭包
* 类型和接口
* 并发编程
* 反射
* 语言交互性