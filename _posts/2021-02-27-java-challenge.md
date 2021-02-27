---
title:  "What does this program do? and how?"
last_modified_at: 2021-02-27T23:20:25-04:00
categories:
  - blog
tags:
  - [java, challenge, math]
excerpt: ""
header:
  overlay_image: /assets/java-challenge/ancient-calender.jpg
---

{% highlight wl linenos %}
{% raw %}
import java.io.*;
public class Main {
        private static void recurse(int x, int n) {
                if(0 != (x & 1073741824))
                        recurse(x * 2, n - n / n);
                else
                        recurse(x * 2, n - n / n);
        }
        public static void main(String[] args) {
                int x = 0, y = Integer.parseInt(args[0]);
                try {
                        recurse(Integer.parseInt(args[1]), 30);
                } catch (Exception e) {
                        StringWriter s = new StringWriter();
                        e.printStackTrace(new PrintWriter(s));
                        for(byte b : s.toString().getBytes()) switch(b) {
                                case 53: x += y;
                                case 55: y += y;
                        }
                }
                System.out.println(x);
        }
}
{% endraw %}
{% endhighlight %}

I found this intersting challenge while I was reading an article on Linus Åkesson [site](http://www.linusakesson.net/), It attracted my attention, and I spent hours trying to figure it out until I successfully did! It was worth it.

Here is my explanation to the program, and my thought process.

I am not a fan of Magic Numbers so first thing that took my attention were these numbers `1073741824`, `30`, `53`, `55`.

In computer programming, the term [Magic Number](https://en.wikipedia.org/wiki/Magic_number_(programming)) is the unique values with unexplained meaning or multiple occurrences which could (preferably) be replaced with named constants
{: .notice--info}


1. `1073741824` is the value of \\(2^{30}\\), (so I knew that I am going to deal with a lot of 0s and 1s).
2. `30`, something related to #1.
3. `53`, ASCII representation for the number `5`.
3. `55`, ASCII representation for the number `7`.

5 and 7? hmmm, let's just remember these numbers and take them into considration.
{: .notice--info}


Now, let's break this program into two major parts:
1. The `recurse` function
2. The for loop over the exception message bytes!

🤯  **My mind at some point**: Why would anyone in the world loop over every byte in an Exception message?!! What is this weirdo recursive function trying to do??!! This program must be drunk!
{: .notice--warning}

---
I wrote a lot of Java code, but to be honest, this is the first time I see this syntax, so it was something new for me.
```java
for(byte b : s.toString().getBytes()) switch(b) {
  case ..
  case ..
}
```
But of course this is not the biggest problem now.

---

The program takes as input two numbers let's say they are `(a)` and `(b)` respectively. We assigne to the vairable `y` the value of `a` and pass `b` to that weird `recurse` function.

At the first moment I thought there is something wrong with `recurse` function; the two conditional branches does the same thing! What is the value of this check `if(0 != (x & 1073741824))` then?!
What's the difference between line `5` and `7` they have the same exact line of code?!

💡 **Gotcha moment:**<br />WAIT A MINUTE!! `5` and `7`? I saw these numbers before!
{: .notice--info}

Exactly they are what switch cases referes to. Apparently that weird part that loops over the exception bytes, checks every stack frame that contains a number indicating the line number the `recurse` function were called from. (If it was coming from line `5` or line `7`).

Now the important question is:
**What exactly the `recurse` function trying to do?**

The `recurse` function take as input two arguemnts the number `b` (second arguments to the program) and the value `30`, which will help in reaching the stopping condition.
The recursive function stopping condition here is not something normal, it's an exception thrown by line 5 or 7 indicating a division by zero, that we catch in line 13.

💡 **Gotcha moment:**<br />That's why `n - n / n` is not same as `n - 1`.<br />At least in this program :)
{: .notice--info}

To understand the effect of passing `x * 2` to the `recurse` function we need understand how multiplying by `2` changes the bits in the binary representation.

```
3 (0b0011) * 2 = 6  (0b0110)
4 (0b0100) * 2 = 8  (0b1000)
5 (0b0101) * 2 = 10 (0b1010)
```

When we multiply by 2 we left shift bits by 1.
{: .notice--info}

The number `1073741824` in binary is
```
0b1000000000000000000000000000000
```
There are `30` 0s and only the most significant bit is on, so if we `AND` any value smaller than `1073741824` with it we will get `0`.

if we started by passing the value `3` to the `recurse` function the `if` condition will always be false until and the `recurse` call in line 7 will get called which we reach when we get a value bigger than `1073741824` and the bit number 31 is on, in this case the condition will be true and we will go to line 5.

**How many calls we need to reach line 5?**<br />
- 29 calls exactly.

because
```
2^29 * 3 = 1610612736 > 1073741824 (and bit 31 is `1`)
2^28 * 3 = 805306368 < 1073741824
```

The number 3 (`0b0011`) has two 1s, so when we shift them 29 times and `AND` them with 2^30 that has `1` at bit `31` it will make the condition true, and we will go to line 5.

And we will have a call stack like this:
```
(recurse:5) --> 1
(recurse:5) --> 1
(recurse:7) --> 0
(recurse:7) --> 0
(recurse:7) --> 0
(recurse:7) --> 0
(recurse:7)  ...
(recurse:7)  ...
(recurse:7)  ...
(recurse:7)
(recurse:7)
(recurse:7)
(recurse:7)
(recurse:7)
(recurse:7)
(recurse:7)
(recurse:7)
(recurse:7)
(recurse:7)
(recurse:7)
(recurse:7)
(recurse:7)
(recurse:7)
(recurse:7)
(recurse:7)
(recurse:7)
(recurse:7)
(recurse:7)
(recurse:7)
(recurse:7)  ...
(recurse:7) --> 0
```

At the top of stack we will find the two calls from line `5` after we shifted the 1s bits in number `3` 29 times.

💡 **Gotcha moment:**<br />
The `recurse` function is trying to represent the number `3` in binary using the exception stack frames.
{: .notice--info}

That now answers a lot of questions regarding the for loop over the exception bytes.
This program is trying to loop over the bits of second number (`b`) starting form the righmost bit, and the switch cases is differentiating between the zeros and ones. It's like saying
```java
case 0: x += y;
case 1: y += y;
```

Now comes the last and hardest part, understanding what this program is trying to achieve.

if a = 3, and b = 7
we will have X = 0, Y = 3.

While looping over the bits of `b`, every time we encounter `0` we will double the value of Y (3, 6, 12, 24, 48, ...)
and every time we encounter `1` we will add previous value of Y to X "and" double the value of Y


🎗️ **Recall:**<br />
In Java we need to break after each case to avoid executing the next case block.
{: .notice--info}

initial values:
```
-----------------------------
|   X     | Y  |      b     |
-----------------------------
|   0     | 3  | 0b0111 (7) |
-----------------------------
```

iteration #1 (bit: `1`):
```
X = 0 + 3 = 3, Y = 6
```

iteration #2 (bit: `1`):
```
X = 3 + 6 = 9, Y = 12
```

iteration #3 (bit: `1`):
```
X = 9 + 12 = 21, Y = 24
```

all coming iteration won't change the value of X so thet doesn't matter. The final value of X is 21
so when b = 7, Y = 3 X will equal 21, this looks like a multiplication!

let's try another set of value to make sure our hypothesis is correct
```
-----------------------------
| X (res) | Y  |      b     |
-----------------------------
|   0     | 4  | 0b0111 (7) |
-----------------------------
```

iteration #1 (bit: `1`):
```
X = 0 + 4 = 4, Y = 8
```

iteration #2 (bit: `1`):
```
X = 4 + 8 = 12, Y = 16
```

iteration #3 (bit: `1`):
```
X = 12 + 16 = 28, Y = 32
```
4 * 7 = 28 ✅


Let's generalize this pattern:
```
4 (0b0100) * 3 ---> (    4    )  * 3
5 (0b0101) * 3 ---> (  4 + 1  )  * 3
6 (0b0110) * 3 ---> (  4 + 2  )  * 3
7 (0b0111) * 3 ---> (4 + 2 + 1)  * 3
8 (0b1000) * 3 ---> (    8    )  * 3
```

That's exactly what the program is trying to do, it converts `b` to binary representation using the exception stack frames trick, then it loops over the binary bits (frames) and for every `1` bit it tries to solve a sub-problem (something like dynamic programming) depending on the position of the bit. e.g If it was the third bit from the right then 2^(position-1) 2^2=4, let `a = 3` then `(4 * 3) = 12`, it stores this value in `X`.
Now if it encountered another `1` bit in the forth bit i.e (0b1100) which is binary representation of `12` then it sums the previous value of `X` (12) with the value of (2^3=8) * 3 which is equal to 24, then we have the final result = `24 + 12 = 36` and that's the value of `3 * 12`.


It turns out that this way of multiplication is called [Ancient Egyptian multiplication](https://en.wikipedia.org/wiki/Ancient_Egyptian_multiplication) ;)
{: .notice--info}

Than was very intersting indeed, hope you enjoyed as much as I did.
