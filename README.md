validator包
================
<img align="right" src="https://raw.githubusercontent.com/go-playground/validator/v9/logo.png">[![Join the chat at https://gitter.im/go-playground/validator](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/go-playground/validator?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)
![Project status](https://img.shields.io/badge/version-10.1.0-green.svg)
[![Build Status](https://travis-ci.org/go-playground/validator.svg?branch=master)](https://travis-ci.org/go-playground/validator)
[![Coverage Status](https://coveralls.io/repos/go-playground/validator/badge.svg?branch=master&service=github)](https://coveralls.io/github/go-playground/validator?branch=master)
[![Go Report Card](https://goreportcard.com/badge/github.com/go-playground/validator)](https://goreportcard.com/report/github.com/go-playground/validator)
[![GoDoc](https://godoc.org/github.com/go-playground/validator?status.svg)](https://godoc.org/github.com/go-playground/validator)
![License](https://img.shields.io/dub/l/vibe-d.svg)

validator包实现对值和结构的验证是基于tag标签的。

下面是它的一些特性：
- 跨字段和跨结构验证，通过使用tag标签或者自定义验证程序。
- 切片、数组、map，可以验证多维字段和所有层级。
- 可以深入map的key和value进行验证。
- 处理interface类型会先确定它的基础字段类型。
- 处理自定义类型，如sql驱动值，请查看 [这里](https://golang.org/src/database/sql/driver/types.go?s=1210:1293#L29)
- 可以给一组验证规则定义一个别名，以便于在结构体上验证。
- 提取自定义的字段名，例如提取出JSON名，然后将此JSON名用于返回给用户。
- 可以定制i18n,感知错误信息。
- 是著名框架[gin](https://github.com/gin-gonic/gin)默认的验证器。将gin使用的v8版本升级到v9版本，请看[这里](https://github.com/go-playground/validator/tree/master/_examples/gin-upgrading-overriding)

使用：
------------
	go get github.com/go-playground/validator/v10
	import "github.com/go-playground/validator/v10"

返回错误值
-------

验证函数返回类型错误<br>
返回类型错误，err总是不为nil:<br>
仅仅验证器InvalidValidationError用于错误验证的输入，nil或ValidationErrors作为错误类型错误。在代码中，仅仅需要检查返回的err是否为nil,以及检查err为InvalidValidationError的类型(一般不需要检查的这么仔细)。示例代码如下：

```go
err := validate.Struct(mystruct)
validationErrors := err.(validator.ValidationErrors)
 ```

用例和文档
------

请查看 https://godoc.org/github.com/go-playground/validator 有详细的描述和用例。

##### 用例:

- [基础](https://github.com/go-playground/validator/blob/master/_examples/simple/main.go)
- [自定义字段类型](https://github.com/go-playground/validator/blob/master/_examples/custom/main.go)
- [结构体类型](https://github.com/go-playground/validator/blob/master/_examples/struct-level/main.go)
- [翻译与自定义错误](https://github.com/go-playground/validator/blob/master/_examples/translations/main.go)
- [gin升级或覆盖validator](https://github.com/go-playground/validator/tree/v9/_examples/gin-upgrading-overriding)
- [清洗 - 将所有用例放置在一起](https://github.com/bluesuncorp/wash)

基准测试
------
###### 运行于Mac pro (15-inch, 2017) go 版本 go1.10.2 darwin/amd64
```go
goos: darwin
goarch: amd64
pkg: github.com/go-playground/validator
BenchmarkFieldSuccess-8                                         20000000                83.6 ns/op             0 B/op          0 allocs/op
BenchmarkFieldSuccessParallel-8                                 50000000                26.8 ns/op             0 B/op          0 allocs/op
BenchmarkFieldFailure-8                                          5000000               291 ns/op             208 B/op          4 allocs/op
BenchmarkFieldFailureParallel-8                                 20000000               107 ns/op             208 B/op          4 allocs/op
BenchmarkFieldArrayDiveSuccess-8                                 2000000               623 ns/op             201 B/op         11 allocs/op
BenchmarkFieldArrayDiveSuccessParallel-8                        10000000               237 ns/op             201 B/op         11 allocs/op
BenchmarkFieldArrayDiveFailure-8                                 2000000               859 ns/op             412 B/op         16 allocs/op
BenchmarkFieldArrayDiveFailureParallel-8                         5000000               335 ns/op             413 B/op         16 allocs/op
BenchmarkFieldMapDiveSuccess-8                                   1000000              1292 ns/op             432 B/op         18 allocs/op
BenchmarkFieldMapDiveSuccessParallel-8                           3000000               467 ns/op             432 B/op         18 allocs/op
BenchmarkFieldMapDiveFailure-8                                   1000000              1082 ns/op             512 B/op         16 allocs/op
BenchmarkFieldMapDiveFailureParallel-8                           5000000               425 ns/op             512 B/op         16 allocs/op
BenchmarkFieldMapDiveWithKeysSuccess-8                           1000000              1539 ns/op             480 B/op         21 allocs/op
BenchmarkFieldMapDiveWithKeysSuccessParallel-8                   3000000               613 ns/op             480 B/op         21 allocs/op
BenchmarkFieldMapDiveWithKeysFailure-8                           1000000              1413 ns/op             721 B/op         21 allocs/op
BenchmarkFieldMapDiveWithKeysFailureParallel-8                   3000000               575 ns/op             721 B/op         21 allocs/op
BenchmarkFieldCustomTypeSuccess-8                               10000000               216 ns/op              32 B/op          2 allocs/op
BenchmarkFieldCustomTypeSuccessParallel-8                       20000000                82.2 ns/op            32 B/op          2 allocs/op
BenchmarkFieldCustomTypeFailure-8                                5000000               274 ns/op             208 B/op          4 allocs/op
BenchmarkFieldCustomTypeFailureParallel-8                       20000000               116 ns/op             208 B/op          4 allocs/op
BenchmarkFieldOrTagSuccess-8                                     2000000               740 ns/op              16 B/op          1 allocs/op
BenchmarkFieldOrTagSuccessParallel-8                             3000000               474 ns/op              16 B/op          1 allocs/op
BenchmarkFieldOrTagFailure-8                                     3000000               471 ns/op             224 B/op          5 allocs/op
BenchmarkFieldOrTagFailureParallel-8                             3000000               414 ns/op             224 B/op          5 allocs/op
BenchmarkStructLevelValidationSuccess-8                         10000000               213 ns/op              32 B/op          2 allocs/op
BenchmarkStructLevelValidationSuccessParallel-8                 20000000                91.8 ns/op            32 B/op          2 allocs/op
BenchmarkStructLevelValidationFailure-8                          3000000               473 ns/op             304 B/op          8 allocs/op
BenchmarkStructLevelValidationFailureParallel-8                 10000000               234 ns/op             304 B/op          8 allocs/op
BenchmarkStructSimpleCustomTypeSuccess-8                         5000000               385 ns/op              32 B/op          2 allocs/op
BenchmarkStructSimpleCustomTypeSuccessParallel-8                10000000               161 ns/op              32 B/op          2 allocs/op
BenchmarkStructSimpleCustomTypeFailure-8                         2000000               640 ns/op             424 B/op          9 allocs/op
BenchmarkStructSimpleCustomTypeFailureParallel-8                 5000000               318 ns/op             440 B/op         10 allocs/op
BenchmarkStructFilteredSuccess-8                                 2000000               597 ns/op             288 B/op          9 allocs/op
BenchmarkStructFilteredSuccessParallel-8                        10000000               266 ns/op             288 B/op          9 allocs/op
BenchmarkStructFilteredFailure-8                                 3000000               454 ns/op             256 B/op          7 allocs/op
BenchmarkStructFilteredFailureParallel-8                        10000000               214 ns/op             256 B/op          7 allocs/op
BenchmarkStructPartialSuccess-8                                  3000000               502 ns/op             256 B/op          6 allocs/op
BenchmarkStructPartialSuccessParallel-8                         10000000               225 ns/op             256 B/op          6 allocs/op
BenchmarkStructPartialFailure-8                                  2000000               702 ns/op             480 B/op         11 allocs/op
BenchmarkStructPartialFailureParallel-8                          5000000               329 ns/op             480 B/op         11 allocs/op
BenchmarkStructExceptSuccess-8                                   2000000               793 ns/op             496 B/op         12 allocs/op
BenchmarkStructExceptSuccessParallel-8                          10000000               193 ns/op             240 B/op          5 allocs/op
BenchmarkStructExceptFailure-8                                   2000000               639 ns/op             464 B/op         10 allocs/op
BenchmarkStructExceptFailureParallel-8                           5000000               300 ns/op             464 B/op         10 allocs/op
BenchmarkStructSimpleCrossFieldSuccess-8                         3000000               417 ns/op              72 B/op          3 allocs/op
BenchmarkStructSimpleCrossFieldSuccessParallel-8                10000000               163 ns/op              72 B/op          3 allocs/op
BenchmarkStructSimpleCrossFieldFailure-8                         2000000               645 ns/op             304 B/op          8 allocs/op
BenchmarkStructSimpleCrossFieldFailureParallel-8                 5000000               285 ns/op             304 B/op          8 allocs/op
BenchmarkStructSimpleCrossStructCrossFieldSuccess-8              3000000               588 ns/op              80 B/op          4 allocs/op
BenchmarkStructSimpleCrossStructCrossFieldSuccessParallel-8     10000000               221 ns/op              80 B/op          4 allocs/op
BenchmarkStructSimpleCrossStructCrossFieldFailure-8              2000000               868 ns/op             320 B/op          9 allocs/op
BenchmarkStructSimpleCrossStructCrossFieldFailureParallel-8      5000000               337 ns/op             320 B/op          9 allocs/op
BenchmarkStructSimpleSuccess-8                                   5000000               260 ns/op               0 B/op          0 allocs/op
BenchmarkStructSimpleSuccessParallel-8                          20000000                90.6 ns/op             0 B/op          0 allocs/op
BenchmarkStructSimpleFailure-8                                   2000000               619 ns/op             424 B/op          9 allocs/op
BenchmarkStructSimpleFailureParallel-8                           5000000               296 ns/op             424 B/op          9 allocs/op
BenchmarkStructComplexSuccess-8                                  1000000              1454 ns/op             128 B/op          8 allocs/op
BenchmarkStructComplexSuccessParallel-8                          3000000               579 ns/op             128 B/op          8 allocs/op
BenchmarkStructComplexFailure-8                                   300000              4140 ns/op            3041 B/op         53 allocs/op
BenchmarkStructComplexFailureParallel-8                          1000000              2127 ns/op            3041 B/op         53 allocs/op
BenchmarkOneof-8                                                10000000               140 ns/op               0 B/op          0 allocs/op
BenchmarkOneofParallel-8                                        20000000                70.1 ns/op             0 B/op          0 allocs/op
```

补充软件
----------------------
这里有一系列软件，用于在验证之前或之后，补充validator包。

* [form](https://github.com/go-playground/form) - 解码 url.Values 成 go 语言的值 以及 编码 go 值 成为url.Values。支持二位数组和所有map。
* [mold](https://github.com/go-playground/mold) - 一个通用库，帮助修改和设置结构体或其他对象的数据。

俗译：liuhongrui/护卫神
