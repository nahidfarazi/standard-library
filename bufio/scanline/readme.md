### bufio.ScanLines ফাংশনটি বিস্তারিত ব্যাখ্যা এবং বাস্তব উদাহরণ
bufio.ScanLines হল Go এর bufio প্যাকেজের একটি ফাংশন, যা bufio.Scanner এর সাথে ব্যবহৃত হয়। এটি একটি split function, যা লাইনের ভিত্তিতে ইনপুটকে স্ক্যান করে।

### ScanLines কীভাবে কাজ করে?
ScanLines ফাংশনটি একটি লাইনকে আলাদা করে এবং নতুন লাইন (\n) এর উপর ভিত্তি করে টোকেন তৈরি করে। এটি একটি Scanner অবজেক্টের সাথে ব্যবহার করা হয়, যা ইনপুট স্ট্রিম (যেমন ফাইল বা ইউজার ইনপুট) থেকে লাইন-বাই-লাইন ডাটা পড়তে সাহায্য করে।

#### ScanLines ফাংশনের সংজ্ঞা:
```go
func ScanLines(data []byte, atEOF bool) (advance int, token []byte, err error)
```
#### প্যারামিটার:
```
data []byte – ইনপুট ডাটা যা স্ক্যান করা হবে।

atEOF bool – ফাইল বা ইনপুটের শেষ (EOF) পৌঁছেছে কিনা তা নির্ধারণ করে।
```
#### রিটার্ন ভ্যালু:
```
advance int – কতটুকু ডাটা প্রসেস করা হয়েছে।

token []byte – স্ক্যান করা টোকেন (অর্থাৎ লাইন)।

err error – যদি কোনো ত্রুটি ঘটে, তাহলে error রিটার্ন করে।
```

#### উদাহরণ ফাইল থেকে লাইন-বাই-লাইন পড়া:
##### নীচের উদাহরণে আমরা ScanLines ব্যবহার করে একটি ফাইলের প্রতিটি লাইন পড়ব এবং প্রিন্ট করব।

```go
package main

import (
	"bufio"
	"fmt"
	"os"
)

func main() {
	// ফাইল ওপেন করা
	file, err := os.Open("sample.txt")
	if err != nil {
		fmt.Println("ফাইল খুলতে সমস্যা:", err)
		return
	}
	defer file.Close()

	// নতুন Scanner তৈরি করা
	scanner := bufio.NewScanner(file)
	scanner.Split(bufio.ScanLines) // লাইনের ভিত্তিতে স্ক্যান সেট করা

	// প্রতিটি লাইন পড়া এবং প্রিন্ট করা
	for scanner.Scan() {
		fmt.Println(scanner.Text())
	}

	// যদি স্ক্যানিং-এর সময় কোনো সমস্যা হয়
	if err := scanner.Err(); err != nil {
		fmt.Println("পড়ার সময় ত্রুটি:", err)
	}
}
```
#### ব্যাখ্যা:
```
প্রথমে os.Open("sample.txt") দিয়ে একটি ফাইল ওপেন করা হয়েছে।

এরপর bufio.NewScanner(file) ব্যবহার করে একটি স্ক্যানার তৈরি করা হয়েছে।

scanner.Split(bufio.ScanLines) – স্ক্যানারের লাইনের ভিত্তিতে বিভাজন করা হয়েছে।

scanner.Text() প্রতিটি লাইন ধরে প্রিন্ট করা হয়েছে।
```
#### উদাহরণ ইউজার ইনপুট থেকে একাধিক লাইন পড়া:
##### এবারের উদাহরণে, আমরা ইউজারের কাছ থেকে একাধিক লাইন ইনপুট নেবো যতক্ষণ না তারা exit লেখে।

```go
import (
	"bufio"
	"fmt"
	"os"
)

func main() {
	scanner := bufio.NewScanner(os.Stdin)
	scanner.Split(bufio.ScanLines) // লাইন-বাই-লাইন ইনপুট নেওয়া

	fmt.Println("লাইন ইনপুট দিন (exit লিখে বন্ধ করুন):")
	for scanner.Scan() {
		text := scanner.Text()
		if text == "exit" {
			fmt.Println("প্রোগ্রাম বন্ধ করা হচ্ছে...")
			break
		}
		fmt.Println("আপনার ইনপুট:", text)
	}

	if err := scanner.Err(); err != nil {
		fmt.Println("ত্রুটি:", err)
	}
}
```
#### ব্যাখ্যা:
```
os.Stdin ব্যবহার করে ইউজারের ইনপুট নেওয়া হয়েছে।

scanner.Scan() প্রতি লাইনে একটি করে লাইন ইনপুট নেয়।

ইউজার যদি exit লিখে, তাহলে লুপ বন্ধ হয়ে যায়।
```
### উদাহরণ API রেসপন্স বা বড় টেক্সট ডাটা লাইনে ভাগ করা:
##### ধরুন, আমরা একটি বড় স্ট্রিং ডাটা নিয়েছি এবং ScanLines ব্যবহার করে সেটাকে লাইনে ভাগ করছি।

```go
package main

import (
	"bufio"
	"fmt"
	"strings"
)

func main() {
	text := `এইটা প্রথম লাইন
এইটা দ্বিতীয় লাইন
এইটা তৃতীয় লাইন`

	// স্ট্রিং থেকে রিডার তৈরি করা
	reader := strings.NewReader(text)
	scanner := bufio.NewScanner(reader)
	scanner.Split(bufio.ScanLines)

	// প্রতিটি লাইন প্রিন্ট করা
	for scanner.Scan() {
		fmt.Println(scanner.Text())
	}

	if err := scanner.Err(); err != nil {
		fmt.Println("ত্রুটি:", err)
	}
}
```
#### ব্যাখ্যা:

strings.NewReader(text) ব্যবহার করে একটি স্ট্রিংকে ইনপুট হিসাবে ব্যবহার করা হয়েছে।
scanner.Split(bufio.ScanLines) দিয়ে প্রতিটি লাইন আলাদা করা হয়েছে।

#### ScanLines ব্যবহার করার সুবিধা:
```
✔ লাইন-বাই-লাইন ডাটা প্রসেস করা যায়
✔ মেমোরি ব্যবহারের দিক থেকে দক্ষ (পুরো ফাইল একবারে লোড করার দরকার নেই)
✔ API, ফাইল, স্ট্রিমিং ডাটা প্রসেসিং ইত্যাদির জন্য উপযোগী
```
#### উপসংহার
bufio.ScanLines মূলত লাইন-বাই-লাইন স্ক্যান করার জন্য ব্যবহৃত হয়।
এটি ফাইল, ইউজার ইনপুট, বা বড় টেক্সট ডাটা স্ক্যান করতে পারে।
বাস্তব জীবনে এটি লগ ফাইল প্রসেসিং, লাইভ স্ট্রিমিং ডাটা প্রসেসিং, এবং API থেকে রেসপন্স পার্স করতে ব্যবহার করা হয়।
