# Week 10 Lab Report 5

## Method to find the difference

With the script `script.sh`, we can test one version of markdown parser with all the given test files. Save them to two files:

```bash
[yifeiding@yifei-framework markdownParse]$ cd ..
[yifeiding@yifei-framework test]$ cd markdown-parse/ 
[yifeiding@yifei-framework markdown-parse]$ bash script.sh >> original.log
[yifeiding@yifei-framework markdown-parse]$ 
```
[yifeiding@yifei-framework markdownParse]$ bash ./script.sh >> ours.log
[yifeiding@yifei-framework markdownParse]$ 
```

Compare two files with `diff -y -W 200 <left> <right> | grep "|"` to compare two files side by side (`--y`), list the difference with width of 200 (`-W 200`) and filter only the lines with difference, we get


```bash
[yifeiding@yifei-framework markdown-parse]$ cd ..
[yifeiding@yifei-framework test]$ diff -y -W 200 ./markdown-parse/original.log ./markdownParse/ours.log | grep "|"
test-files/14.md[/foo]										   |	test-files/14.md[]
test-files/194.md[url]										   |	test-files/194.md[]
test-files/201.md[baz]										   |	test-files/201.md[]
test-files/489.md[]										   |	test-files/489.md[foo
test-files/490.md[]										   |	test-files/490.md[<foo
test-files/495.md[foo(and(bar))]								   |	test-files/495.md[foo(and(bar]
test-files/496.md[]										   |	test-files/496.md[foo(and(bar]
test-files/497.md[]										   |	test-files/497.md[foo\(and\(bar\)]
test-files/498.md[]										   |	test-files/498.md[<foo(and(bar]
test-files/499.md[foo\]										   |	test-files/499.md[foo\)\:]
test-files/510.md[/uri]										   |	test-files/510.md[]
test-files/511.md[/uri]										   |	test-files/511.md[]
test-files/512.md[/uri]										   |	test-files/512.md[]
test-files/573.md[/url]										   |	test-files/573.md[]
test-files/577.md[train.jpg]									   |	test-files/577.md[]
test-files/579.md[<url>]									   |	test-files/579.md[]
test-files/580.md[/url]										   |	test-files/580.md[]

```

## Analyze 

### 14.md

```
test-files/14.md[/foo]					      |	test-files/14.md[]
```

The original is incorrect, as it includes `/foo`. The expected output is `[]`. In VScode markdown render, it isn't a link since the left square bracket is escaped. The escaped left square bracket is being considered as not escaped `\[not a link](/foo)` in original solution, where it doesn't have any detection of escaped character.

The iterative solution in the original solution needs to check if `[` is escaped before it is being used for "open paren. Escape character could be checked with a loop where we loop until we found a `[` that doesn't contain a escape character righht before it.

```java
            int nextOpenBracket = markdown.indexOf("[", currentIndex);
```

### 194.md

```
test-files/194.md[url]					      |	test-files/194.md[]
```

The original is incorrect, The expected output is `[]`.`[Foo*bar\]]:my_(url) 'title (with parens)'` has other strings inbetween `](`, making it not a link. This is the result of not checking `]` and `(` together. As we can see in the original code, they are checked separately. because it checks the `]` and `(` separately. We can check `openParen-nextCloseBracket == 1` to see if they are one after another. Adding a line after these two lines below:


```java
            int nextCloseBracket = markdown.indexOf("]", nextOpenBracket);
            int openParen = markdown.indexOf("(", nextCloseBracket);
```
