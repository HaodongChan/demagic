# demagic
A coding style __proposal__
> filling out the real code only after finishing the pseudo-code and keep the pseudo-code as comments

### examples
In many coding languages, we have `//` for commenting after an entire line. But how do you do if you want a specific expression in the line highlighted and explained?

In C/C++, you can use `/**/`, but it was so ugly.

I mean, if you prefer ___making your code more like a book___ to just coding it out and leave it more like a magical equipment, you'd like to express your mind in English rather than in code that is lawful and formatted but rarely readable.

Here is an example in GoLang.

```go
package main

import "fmt"

// usage: 
// go fibonacci(c, q)
// then use <-c to "call"(read) the yield value multiple times. 
// The array of "returned"(written) values are 0,1,1,2,3,5...
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
	q<-0// any int as signal to quit
}
```
See? You can first type `fmt.Println(?"the i+1 th fibonacci")` to use a placeholder and keep on coding so that you can leave the details behind and concentrate on the logic itself.

When you get all the things done, you come back to add the working detail.

As you are adding the detail, you comment the rest of line started with the placeholder(you may need to parenthesize it if it was not). The future tool can still translate the code into the non-detailed 'book'.

Besides convenience in expressing, readers can understand your 'book' also more conveniently, and get to know the original thoughts instead of the formatted ones.



* * *

### 中文阐释：（Explain in Chinese）

簡單說一下，就是經常我們寫代碼的時候需要寫pseudocode，或者描述一下期望的功能，然後繼續編碼；同樣地，我們讀代碼的時候，也經常需要把一大段代碼用它整體實現了什麼功能來理解，不考慮實現細節。有時候有些人在實現功能的時候為了代碼效率，不得不採用一些magic words的寫法，讓讀者讀得雲裡霧裡的，需要花很多時間來理解，所以我就希望，這些人在實現這樣magic words的時候、在實現一個功能塊的時候的時候，可以用到一種通用的、可以在任意位置（行頭、行間、行尾）注釋的注釋方法，並且實現標準。這就極大地方便了代碼閱讀——由於更精細（而不是更頻繁）的注釋，代碼閱讀效率得到提高；這還可以幫助有跳步思維習慣（這不好，因為顯然這些步驟都是不可或缺的）的程序員用自己喜歡的方式編程。



註釋倡議如下：

未實現時，用 ___?"what you want here"___ 來占位，

實現後，直接在 ___?"what you want here"___ 前打 __//__ 註釋掉，保留後面的格式，並在 __//__ 前添加同樣的格式，就完成了任意位置的註釋。
