## bufio.ScanBytes ফাংশনটি বিস্তারিত ব্যাখ্যা এবং ব্যবহার
### ScanBytes কী?
bufio.ScanBytes হল bufio.Scanner-এর একটি split function, যা ইনপুট ডাটা স্ট্রিমকে প্রতি বাইট করে স্ক্যান করে এবং প্রতিটি বাইটকে আলাদা করে রিটার্ন করে।

bufio.Scanner সাধারণত লাইন, শব্দ বা বাইট অনুযায়ী ডাটা পার্স করতে ব্যবহৃত হয়, যেখানে ScanBytes প্রতিটি বাইটকে আলাদা টোকেন হিসেবে ধরে।

####ScanBytes এর সিনট্যাক্স

```go
func ScanBytes(data []byte, atEOF bool) (advance int, token []byte, err error)
```
#### প্যারামিটার ব্যাখ্যা:
```data []byte → ইনপুট ডাটা স্ট্রিম (যে বাফার থেকে বাইট নেওয়া হবে)।

atEOF bool → যদি true হয়, তাহলে শেষ পর্যন্ত পৌঁছানোর পর কীভাবে আচরণ করবে তা নির্ধারণ করে।

advance int → কত বাইট প্রসেস করা হয়েছে তার সংখ্যা রিটার্ন করে।

token []byte → পরবর্তী টোকেন (একটি বাইট) রিটার্ন করে।

err error → কোনো এরর হলে তা রিটার্ন করবে।

ScanBytes ব্যবহার করা হয় যখন:
টেক্সট বা বাইনারি ডাটা বাইট-বাই-বাইট বিশ্লেষণ করতে হয়।

সিরিয়াল ডিভাইস থেকে ইনপুট নিতে হয় (যেমন সেন্সর ডাটা বা স্ট্রিমিং ডাটা)।

ফাইল বা নেটওয়ার্ক স্ট্রিম থেকে ইনপুট রিড করা হয় এবং প্রতিটি বাইট আলাদাভাবে প্রসেস করতে হয়।
```
### উদাহরণ: প্রতিটি বাইট আলাদা করে প্রিন্ট করা!
```go
package main

import (
	"bufio"
	"fmt"
	"strings"
)

func main() {
	// ইনপুট স্ট্রিং
	input := "Hello"

	// bufio.Scanner তৈরি
	scanner := bufio.NewScanner(strings.NewReader(input))

	// Scanner-কে বাইট-বাই-বাইট স্ক্যান করতে সেট করা
	scanner.Split(bufio.ScanBytes)

	// প্রতিটি বাইট প্রিন্ট করা
	for scanner.Scan() {
		fmt.Printf("Byte: %q\n", scanner.Text())
	}

	// সম্ভাব্য এরর চেক করা
	if err := scanner.Err(); err != nil {
		fmt.Println("Error:", err)
	}
}
```
