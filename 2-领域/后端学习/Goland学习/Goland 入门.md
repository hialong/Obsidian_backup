---
created: 2024-10-19
updated: 2024-10-19
Type: knowledge
Status: ⌛️ 等待
tags:
---
## 关于测试用例的编写

1. 要有go.mod 才能跑test，  
        1. 后缀名称为xxx_test.go的文件就是测试文件了  
        2. 测试函数命名必须以单词Test开始  
        3. 测试函数只接受一个参数t `*testing.T  `
        4. 类型为 ` *testing.T ` 的变量 t 是你在测试框架中的 hook（钩子）所以当你想让测试失败时可以执行 t.Fail() 之类的操作  

3. 公共函数以大写字母开始，私有函数以小写字母开头
测试用例可以写多个子用例，如下


```go
package main  
  
import (  
    "testing"  
)  
  
func TestHelloWorld(t *testing.T) {  
  
    convertHelpTest := func(t *testing.T, got, want string) {  
       t.Helper()  
       if got != want {  
          t.Errorf("got %q,want %q", got, want)  
       }  
    }  
    t.Run("say hello to people", func(t *testing.T) {  
       got := Hello("Tom", "")  
       want := "Hello Tom"  
       convertHelpTest(t, got, want)  
    })  
    t.Run("say hello to the world when an empty string is suppiled ", func(t *testing.T) {  
       got := Hello("", "")  
       want := "Hello World"  
       convertHelpTest(t, got, want)  
    })  
    t.Run("specify language", func(t *testing.T) {  
       got := Hello("Tom", "Spanish")  
       want := "Hola Tom"  
       convertHelpTest(t, got, want)  
    })  
    t.Run("specify language", func(t *testing.T) {  
       got := Hello("Tom", "French")  
       want := "Bonjour Tom"  
       convertHelpTest(t, got, want)  
    })  
  
}
```
## goDoc
 go 在 1.13 之前自带 godoc，自行下载要用这个命令  
``` shell
go env -w GOPROXY=https://goproxy.cn,direct   
go get golang.org/x/tools/cmd/godoc  
go install golang.org/x/tools/cmd/godoc  
```

使用方式是
```shell
godoc -http :8000
```

然后访问 http://localhost:8000/pkg/ 
## 命名返回值
. 命名返回值，即参数后再来一个括号例如下面的这个

```go
func greetToTheWorld(language string) (prefix string) {  
    switch language {  
    case spanish:  
       prefix = SpanishPrefix  
    case french:  
       prefix = FrenchPrefix  
    default:  
       prefix = EnPrefix  
    }  
    return  
}
```

这个prefix 就是零值，int 就是 0 string 就是“”（与 java 不同，java 是 null）

- 这将在你的函数中创建一个名为 `prefix` 的变量。
    
    - 它将被分配「零」值。这取决于类型，例如 `int` 是 0，对于字符串它是 `""`。
        
        - 你只需调用 `return` 而不是 `return prefix` 即可返回所设置的值。
        
    - 这将显示在 Go Doc 中，所以它使你的代码更加清晰。