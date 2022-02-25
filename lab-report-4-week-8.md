# Week 8 Lab Report 4

My [GitHub - dingyifei/markdownParser: lab2 repo](https://github.com/dingyifei/markdownParser) and the reviewed repository [GitHub - floatboat/markdown-parse](https://github.com/floatboat/markdown-parse) are compared. The test files `test1, test2, test3` are put into each repository



## Test on my code

The following code were added to my MarkdownParseTest

```java
    @Test
    public void Test1() throws IOException, NoSuchFileException {

        //passes if running Markdown parse returns the correct text
        List<String> correctOutput = List.of("`google.com", "google.com","ucsd.edu");
        Path fileName = Path.of("test1");
        // read the file contents into a string
        String contents = Files.readString(fileName);
        // run getLinks on the contents of the file
        ArrayList<String> links = MarkdownParse.getLinks(contents);
        assertEquals(correctOutput,links);
    }

    @Test
    public void Test2() throws IOException, NoSuchFileException {

        //passes if running Markdown parse returns the correct text
        List<String> correctOutput = List.of("a.com", "a.com(())", "example.com");
        Path fileName = Path.of("test2");
        // read the file contents into a string
        String contents = Files.readString(fileName);
        // run getLinks on the contents of the file
        ArrayList<String> links = MarkdownParse.getLinks(contents);
        assertEquals(correctOutput,links);
    }
    @Test
    public void Test3() throws IOException, NoSuchFileException {

        //passes if running Markdown parse returns the correct text
        List<String> correctOutput = List.of("https://ucsd-cse15l-w22.github.io/");
        Path fileName = Path.of("test3");
        // read the file contents into a string
        String contents = Files.readString(fileName);
        // run getLinks on the contents of the file
        ArrayList<String> links = MarkdownParse.getLinks(contents);
        assertEquals(correctOutput,links);
    }
```



All three tests failed:

```bash
javac -cp .:lib/junit-4.13.2.jar:lib/hamcrest-core-1.3,jar MarkdownParse.java
javac -cp .:lib/junit-4.13.2.jar:lib/hamcrest-core-1.3,jar MarkdownParseTest.java
java -cp .:lib/junit-4.13.2.jar:lib/hamcrest-core-1.3.jar org.junit.runner.JUnitCore MarkdownParseTest
JUnit version 4.13.2
...E.E.E....
Time: 0.011
There were 3 failures:
1) Test1(MarkdownParseTest)
java.lang.AssertionError: expected:<[`google.com, google.com, ucsd.edu]> but was:<[url.com, `google.com, google.com]>
	at org.junit.Assert.fail(Assert.java:89)
	at org.junit.Assert.failNotEquals(Assert.java:835)
	at org.junit.Assert.assertEquals(Assert.java:120)
	at org.junit.Assert.assertEquals(Assert.java:146)
	at MarkdownParseTest.Test1(MarkdownParseTest.java:93)
2) Test2(MarkdownParseTest)
java.lang.AssertionError: expected:<[a.com, a.com(()), example.com]> but was:<[a.com, a.com((, example.com]>
	at org.junit.Assert.fail(Assert.java:89)
	at org.junit.Assert.failNotEquals(Assert.java:835)
	at org.junit.Assert.assertEquals(Assert.java:120)
	at org.junit.Assert.assertEquals(Assert.java:146)
	at MarkdownParseTest.Test2(MarkdownParseTest.java:106)
3) Test3(MarkdownParseTest)
java.lang.AssertionError: expected:<[https://ucsd-cse15l-w22.github.io/]> but was:<[]>
	at org.junit.Assert.fail(Assert.java:89)
	at org.junit.Assert.failNotEquals(Assert.java:835)
	at org.junit.Assert.assertEquals(Assert.java:120)
	at org.junit.Assert.assertEquals(Assert.java:146)
	at MarkdownParseTest.Test3(MarkdownParseTest.java:118)

FAILURES!!!
Tests run: 9,  Failures: 3

make: *** [Makefile:8: test] Error 1

```

## Test on their code

Since their code changed the header, the implementation for their code is different:

```java
    @Test
    public void test1() throws IOException {
        String regFile = Files.readString(Path.of("test1"));
        String[] regLines = regFile.split("\n");
        assertEquals(List.of("`google.com", "google.com","ucsd.edu"), MarkdownParse.getLinks(regLines));
    }
    @Test
    public void test2() throws IOException {
        String regFile = Files.readString(Path.of("test2"));
        String[] regLines = regFile.split("\n");
        assertEquals(List.of("a.com", "a.com(())", "example.com"), MarkdownParse.getLinks(regLines));
    }
    @Test
    public void test3() throws IOException {
        String regFile = Files.readString(Path.of("test3"));
        String[] regLines = regFile.split("\n");
        assertEquals(List.of("https://ucsd-cse15l-w22.github.io/"), MarkdownParse.getLinks(regLines));
    }
```



All three tests failed:

```bash
javac -cp .:lib/junit-4.13.2.jar:lib/hamcrest-core-1.3,jar MarkdownParseTest.java
java -cp .:lib/junit-4.13.2.jar:lib/hamcrest-core-1.3.jar org.junit.runner.JUnitCore MarkdownParseTest
JUnit version 4.13.2
....E.E.E.
Time: 0.01
There were 3 failures:
1) test1(MarkdownParseTest)
java.lang.AssertionError: expected:<[`google.com, google.com, ucsd.edu]> but was:<[url.com, `google.com, google.com, ucsd.edu]>
	at org.junit.Assert.fail(Assert.java:89)
	at org.junit.Assert.failNotEquals(Assert.java:835)
	at org.junit.Assert.assertEquals(Assert.java:120)
	at org.junit.Assert.assertEquals(Assert.java:146)
	at MarkdownParseTest.test1(MarkdownParseTest.java:35)
2) test2(MarkdownParseTest)
java.lang.AssertionError: expected:<[a.com, a.com(()), example.com]> but was:<[a.com, a.com((, example.com]>
	at org.junit.Assert.fail(Assert.java:89)
	at org.junit.Assert.failNotEquals(Assert.java:835)
	at org.junit.Assert.assertEquals(Assert.java:120)
	at org.junit.Assert.assertEquals(Assert.java:146)
	at MarkdownParseTest.test2(MarkdownParseTest.java:41)
3) test3(MarkdownParseTest)
java.lang.AssertionError: expected:<[https://ucsd-cse15l-w22.github.io/]> but was:<[, , ]>
	at org.junit.Assert.fail(Assert.java:89)
	at org.junit.Assert.failNotEquals(Assert.java:835)
	at org.junit.Assert.assertEquals(Assert.java:120)
	at org.junit.Assert.assertEquals(Assert.java:146)
	at MarkdownParseTest.test3(MarkdownParseTest.java:47)

FAILURES!!!
Tests run: 7,  Failures: 3

make: *** [makefile:8: test] Error 1

```

## Analyze my program

### Snippet 1

- Yes, it is possible to add a small change to resolve this issue.
- Since we observed that backticks enter and exist code blocks, we can implement a boolean that indicate if we are looping inside the code block. If we are in the code block, then `[`, `]` and `(`  would not trigger the detection of link, but a link can have a backtick in it by letting `)`  still be triggered in the program. We can add another boolean for `\n` detection so we can back track to include links with one backtick instead of a code block.  the second detection for line skip would exceed small code change.

### Snippet 2

- Yes

- The issue with our code is that it detects the end bracket `)`and stop including the rest of the brackets in the link for the second link. The solution would be to could the number of open and end bracket so the code cut off the link when both the open and close brackets are balanced. It can be implemented by adding a variable for counting the additional open brackets that need end bracket and another variable to back track to the last end bracket. As soon as we balanced the open/end bracket. we can cut off the link (the link is complete).

### Snippet 3



- Yes
- The issue with our code is the contains space detection exclude the link for it containing space at the beginning of the line. We need to pre-process the substring we obtained containing the link to remove all spaces right after `\n` and `\n`until the next character appears. By creating another function that loop through the substring of the link and have a boolean and a if statement checking if we are right after \n or have touched any other character and substring to remove the `\n` and space one by one until we touch a non-space/non-`\n` character.