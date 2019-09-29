# demagic
A coding style __proposal__
> filling out the real code only after finishing the pseudo-code and keep the pseudo-code as comments

### examples
In many coding languages, we have `//` for commenting after an entire line. But how do you do if you want a specific expression in the line highlighted and explained?

In C/C++, you can use `/**/`, but it was so ugly.

I mean, if you prefer ___making your code like a book___ to just coding it out and leave it like a magical equipment you'd like to express your mind in natural English rather than that is lawful in the coding language but is rarely readable.

Here is an example in GoLang.

```go
package main

import "fmt"

// usage: 
// go fibonacci(c, q)
// 然後多次<-c來多次"調用"（取值），"返回"（賦值）序列爲0,1,1,2,3,5...
// The above line in English : 
// then use <-c to "call"(read) the yield value multiple times. 
// The array of "return"(written) values are 0,1,1,2,3,5...
func fibonacci(c, q chan int) {
	var x, y int = 0, 1
	var tmp int
	for {
		select {
		case c<-x:
			tmp = y
			y = x + y
			x = tmp
		case <-q:
			break
		}
	}
}

func main() {
	c := make(chan int)
	q := make(chan int)
	go fibonacci(c,q)
	for i := 0; i < 10; i++ {
		fmt.Println(<-c)//?"the i+1 th fibonacci")
	}
	q<-0//?"any int as signal to quit"
}
```
So you can first type `?"the i+1 th fibonacci"` as an placeholder and keep on coding so that you can leave the details behind and concentrate on the logic itself.
Also, readers can understand your 'book' faster, and get to know the original instead of formatted thoughts.
