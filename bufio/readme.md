## bufio.ScanBytes ফাংশনটি বিস্তারিত ব্যাখ্যা এবং ব্যবহার
### ScanBytes কী?
bufio.ScanBytes হল bufio.Scanner-এর একটি split function, যা ইনপুট ডাটা স্ট্রিমকে প্রতি বাইট করে স্ক্যান করে এবং প্রতিটি বাইটকে আলাদা করে রিটার্ন করে।
bufio.Scanner সাধারণত লাইন, শব্দ বা বাইট অনুযায়ী ডাটা পার্স করতে ব্যবহৃত হয়, যেখানে ScanBytes প্রতিটি বাইটকে আলাদা টোকেন হিসেবে ধরে।

#### ScanBytes এর সিনট্যাক্স

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
#### উদাহরণ: প্রতিটি বাইট আলাদা করে প্রিন্ট করা!
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
উদাহরণ: ফাইল থেকে বাইট-বাই-বাইট পড়া!

```go
package main

import (
	"bufio"
	"fmt"
	"os"
)

func main() {
	// ফাইল ওপেন করা
	file, err := os.Open("test.txt")
	if err != nil {
		fmt.Println("Error opening file:", err)
		return
	}
	defer file.Close()

	// Scanner তৈরি
	scanner := bufio.NewScanner(file)
	scanner.Split(bufio.ScanBytes)

	// ফাইল থেকে প্রতিটি বাইট পড়া
	for scanner.Scan() {
		fmt.Printf("Byte: %q\n", scanner.Text())
	}

	// সম্ভাব্য এরর চেক করা
	if err := scanner.Err(); err != nil {
		fmt.Println("Error:", err)
	}
}
```
#### ব্যাখ্যা
```
os.Open("test.txt") দিয়ে ফাইল ওপেন করা হয়েছে।

scanner.Split(bufio.ScanBytes) দিয়ে Scanner-কে প্রতি বাইট পড়ার জন্য সেট করা হয়েছে।

scanner.Text() দিয়ে প্রতিটি বাইট প্রিন্ট করা হচ্ছে।
```
#### উদাহরণ: সিরিয়াল ডিভাইস থেকে সেন্সর ডাটা রিড করা!
```অনেক ক্ষেত্রে UART বা Serial Communication ব্যবহার করে সেন্সর বা IoT ডিভাইস থেকে ইনপুট নেওয়া হয়। সেখানে ScanBytes ব্যবহার করা যায়।
```

```go
package main

import (
	"bufio"
	"fmt"
	"log"
	"os"
)

func main() {
	// ধরে নিই, "/dev/ttyUSB0" হলো সিরিয়াল পোর্ট
	file, err := os.Open("/dev/ttyUSB0")
	if err != nil {
		log.Fatal(err)
	}
	defer file.Close()

	scanner := bufio.NewScanner(file)
	scanner.Split(bufio.ScanBytes)

	for scanner.Scan() {
		fmt.Printf("Received Byte: %q\n", scanner.Text())
	}

	if err := scanner.Err(); err != nil {
		fmt.Println("Error:", err)
	}
}
```
#### ব্যাখ্যা
```
এখানে আমরা "/dev/ttyUSB0" (সিরিয়াল পোর্ট) থেকে ইনপুট পড়ছি।

ScanBytes ব্যবহার করে প্রতিটি বাইট প্রসেস করা হচ্ছে।

এটি সেন্সর ডাটা, আরডুইনো ইনপুট বা GPS ডাটা প্রসেস করার জন্য ব্যবহার করা যেতে পারে।
```
