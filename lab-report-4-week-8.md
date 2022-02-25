# Week 8 Lab Report

## Markdown-Parse Repository links:
- [My Repository](https://github.com/Aziiz0/markdown-parse-Fireflies)
- [Reviewed Repository](https://github.com/mBookUCSD/markdown-parse)

---

## Markdown Tests:

>## Snippet 1

~~~
`[a link`](url.com)

[another link](`google.com)`

[`cod[e`](google.com)

[`code]`](ucsd.edu)
~~~

### Expected Output

~~~
(`google.com, google.com, ucsd.edu)
~~~

### MarkdownParseTest.java

~~~
@Test
public void snippet1() throws IOException{
        ArrayList<String> result = MarkdownParse.getLinks(Files.readString(Path.of("snippet1.md")));
        List<String> newList = new ArrayList<String>();
        newList.add("'google.com");
        newList.add("google.com");
        newList.add("ucsd.edu");

        assertEquals(newList, result);
}
~~~

### My Implementation

~~~
java.lang.AssertionError: expected:<['google.com, google.com, ucsd.edu]> but was:<[url.com, `google.com, google.com, ucsd.edu]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at MarkdownParseTest.snippet1(MarkdownParseTest.java:76)
~~~

### Reviewed Implementation

~~~
java.lang.AssertionError: expected:<['google.com, google.com, ucsd.edu]> but was:<[url.com, `google.com, google.com]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at MarkdownParseTest.snippet1(MarkdownParseTest.java:172)
~~~

>## Snippet 2

~~~
[a [nested link](a.com)](b.com)

[a nested parenthesized url](a.com(()))

[some escaped \[ brackets \]](example.com)
~~~

### Expected Output

~~~
(a.com, a.com(()), example.com)
~~~

### MarkdownParseTest.java

~~~
@Test
public void snippet1() throws IOException{
        ArrayList<String> result = MarkdownParse.getLinks(Files.readString(Path.of("snippet1.md")));
        List<String> newList = new ArrayList<String>();
        newList.add("");

        assertEquals(newList, result);
}
~~~

### My Implementation

~~~
java.lang.AssertionError: expected:<[a.com, a.com(()), example.com]> but was:<[a.com, a.com((), example.com]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at MarkdownParseTest.snippet2(MarkdownParseTest.java:87)
~~~

### Reviewed Implementation

~~~
java.lang.AssertionError: expected:<[a.com, a.com(()), example.com]> but was:<[a.com, a.com((, example.com]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at MarkdownParseTest.snippet2(MarkdownParseTest.java:183)
~~~

>## Snippet 3

~~~
[this title text is really long and takes up more than 
one line

and has some line breaks](
    https://www.twitter.com
)

[this title text is really long and takes up more than 
one line](
    https://ucsd-cse15l-w22.github.io/
)


[this link doesn't have a closing parenthesis](github.com

And there's still some more text after that.

[this link doesn't have a closing parenthesis for a while](https://cse.ucsd.edu/



)

And then there's more text
~~~

### Expected Output

~~~
(https://www.twitter.com, https://ucsd-cse15l-w22.github.io/, https://cse.ucsd.edu/)
~~~

### MarkdownParseTest.java

~~~
@Test
public void snippet1() throws IOException{
        ArrayList<String> result = MarkdownParse.getLinks(Files.readString(Path.of("snippet1.md")));
        List<String> newList = new ArrayList<String>();
        newList.add("");

        assertEquals(newList, result);
}
~~~

### My Implementation

~~~
java.lang.AssertionError: expected:<[https://www.twitter.com, https://ucsd-cse15l-w22.github.io/, https://cse.ucsd.edu/]> but was:<[
    https://www.twitter.com
,
    https://ucsd-cse15l-w22.github.io/
, github.com

And there's still some more text after that.

[this link doesn't have a closing parenthesis for a while](https://cse.ucsd.edu/



]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at MarkdownParseTest.snippet3(MarkdownParseTest.java:98)
~~~

### Reviewed Implementation

~~~
java.lang.AssertionError: expected:<[https://www.twitter.com, https://ucsd-cse15l-w22.github.io/, https://cse.ucsd.edu/]> but was:<[]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at MarkdownParseTest.snippet3(MarkdownParseTest.java:194)
~~~

---

## Answering Questions


1. Do you think there is a small (<10 lines) code change that will make your program work for snippet 1 and all related cases that use inline code with backticks? If yes, describe the code change. If not, describe why it would be a more involved change.

> Yes we could easily adjust and correct for the ''' being present before a first bracket. If we had some check character for ''' before a first bracket we could ignore that link and jump to the last parenthesis and not add the link to our array list.

2. Do you think there is a small (<10 lines) code change that will make your program work for snippet 2 and all related cases that nest parentheses, brackets, and escaped brackets? If yes, describe the code change. If not, describe why it would be a more involved change.

> It may be a little challenging but we could have some sort of check for repeated parenthesis doubling. Say if we have a first open parenthesis, and another open aprenthesis then we would need to look for two close parenthesis to make it a link. This would be difficult but I belive I would first start by having some loop check for an open parenthesis first then a close one and have some count that will count how many open parenthesis we end up with then once we hit a close parenthesis we look for that many for which our count had for open parenthesis.

3. Do you think there is a small (<10 lines) code change that will make your program work for snippet 3 and all related cases that have newlines in brackets and parentheses? If yes, describe the code change. If not, describe why it would be a more involved change.

> This one looks very challenging since we have a link present without a bracket name. The link https://www.twitter.com exists as a link and as its displayed name. I would not be able to percieve a code to detect this and have the register pick up a link like this while also being able to pick up other regular links or non-links with gaps between text.