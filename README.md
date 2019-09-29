# demagic
A coding style __proposal__
> filling out the real code only after finishing the pseudo-code and keep the pseudo-code as comments

### examples
In many coding languages, we have `//` for commenting after an entire line. But how do you do if you want a specific expression in the line highlighted and explained?

In C/C++, you can use `/**/`, but it was so ugly.

I mean, if you prefer ___making your code more like a book___ to just coding it out and leave it more like a magical equipment, you'd like to express your mind in English plus some logical symbols rather than in code that is lawful and formatted but rarely readable.

Here is an example in GoLang.

```go
// 斐波那契數組，打印前十項，然後退出
// 方案一，把十項斐波那契數在一個函數中全部計算完畢，輸出到一個數組中，然後打印出來
// 方案二，在一個函數中，計算一個斐波那契數，然後輸出出來，再計算下一個斐波那契
// 方案三，在一個函數（協程）中做計算，在另一個函數（協程）中做輸出，兩個協程通過通道傳遞計算結果或（如果需要的話）結束消息
//		具體方案1， main函數做計算，計算完所有的十項斐波那契數後通知先前go了的輸出協程做輸出。這在基本流程上和方案一無異。
//		具體方案2，main函數做輸出，go一個計算方程，先計算完，再回到main輸出。同上，與方案一基本流程無異
//		具體方案3，main函數輸出，go協程計算，設置通道大小爲1，每塞入第二個數的時候因爲通道滿了，所以計算協程阻塞，轉到main函數從通道中拿出計算結果輸出，到輸出下一個的時候，因爲通道空了，所以main協程阻塞，轉到計算協程塞入和計算。直到main協程輸出了10次，用另一個預先設置好的通道通知計算協程退出，而計算協程相當於在每一次阻塞的時候都從通知退出的通道和等待塞入的通道二者之間選擇一個先疏通的通道運行，即用select選擇<-quit或c<-x。
//		具體方案4，main函數輸出，go協程計算，設置通道大小爲1，每塞入第二個數的時候因爲通道滿了，所以計算協程阻塞，轉到main函數從通道中拿出計算結果輸出，到輸出下一個的時候，因爲通道空了，所以main協程阻塞，轉到計算協程塞入和計算。直到main協程輸出了10次，然後在main協程阻塞之前，打破循環，結束。
//		具體方案5，main函數輸出，go協程計算，設置通道大小爲1，每塞入第二個數的時候因爲通道滿了，所以計算協程阻塞，轉到main函數從通道中拿出計算結果輸出，到輸出下一個的時候，因爲通道空了，所以main協程阻塞，轉到計算協程塞入和計算。直到計算協程計算了10個斐波那契數後，將通道關閉，那麼接受到關閉信號的main協程（main函數）則打破循環，結束。
// 		具體方案6，main函數計算，go協程輸出，。。。
// The above lines are my efforts to tell all the ways to code a Finbonacci in GoLang.
// I present the original one so that some of you may get to know where the inspiration comes from.
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
	q<-0//?"any int as signal to quit" -- this was added after everything else is done to add up the comments. It's redundant.
}
```
See? You can first type `fmt.Println(?"the i+1 th fibonacci")` to use a placeholder and keep on coding so that you can leave the details behind and concentrate on the logic itself.

When you get all the things done, you come back to add the working detail.

As you are adding the detail, you comment the rest of line started with the placeholder(you may need to parenthesize it if it was not). The future tool can still translate the code into the non-detailed 'book'.

Besides convenience in expressing, readers can understand your 'book' also more conveniently, and get to know the original thoughts instead of the formatted ones.
