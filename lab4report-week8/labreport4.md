# **Lab 7: Inter-Group Repository Critiques**

`by Dave Vo | 5/20/22`


[My Repository](https://github.com/ddavevo/markdown-parser-davevo)

[Other Group's Repository](https://github.com/nquach1515/markdown-parser-cse15l)

## How the Markdown Links Gets Translated

![](snippetpics-myrepo/2.%20testSnippet1Preview.png)

![](snippetpics-myrepo/2.%20testSnippet2Preview.png)

![](snippetpics-myrepo/2.%20testSnippet3Preview.png)

## Tests for Snippet Files

Based on the snippet file contents, I expect them to pick out the URLs.

![](snippetpics-myrepo/1.%20testSnippet1.png)

![](snippetpics-myrepo/1.%20testSnippet2.png)

![](snippetpics-myrepo/1.%20testSnippet3.png)


## My Repository's JUnit Test Results

```
The first test case successfully extracts the URL links in snippet 1, but returns different results than expected with snippets 2 & 3.
```

![](snippetpics-myrepo/3.testSnippet1Pass.png)

![](snippetpics-myrepo/3.testSnippet2Fail.png)

![](snippetpics-myrepo/3.testSnippet3Fail.png)

## Other Repository's JUnit Test Results

The first test case successfully extracts the URL links in snippet 1, but fails tests for snippets 2 & 3.

However, the other repository's results differ from my results in the test cases that failed. Unlike mine for snippet 2, their repository did identify a link in the first line. Nonetheless, their parsing incorrectly extracted the nested link `a.com` in the brackets instead of the link in parentheses `b.com`.

As as for snippet 3, their parsing logic performed slightly better since they managed to correctly extract the URL `https://www.twitter.com` at the beginning of the call instead of the end.

![](snippetpics-otherrepo/3.5.%20testSnippet1OtherPass.png)

![](snippetpics-otherrepo/3.5.%20testSnippet2OtherFail.png)

![](snippetpics-otherrepo/3.5.%20testSnippet3OtherFail.png)

## What Would it Take to Improve My MarkdownParse?

`Snippet 1`

MarkdownParse successfully handles Snippet 1 because the program logic does not consider backticks to begin with. With or without them, the same links would be returned as long as the brackets and parentheses are in pairs.

`Snippet 2`

The first link `[a [nested link] (a.com)](b.com)` did not show up in what was returned because it was unintentionally removed when parsing. The cause of removal is an if condition that is supposed to handle image links, but if there is no exclamation mark, the first link always get taken out.

The fix should be under 10 lines of code. The logic should first confirm if an exclamation mark exists, then remove the image link if the exclamation mark exists adjacently before the open bracket.

`Snippet 3`

Having analyzed the error of Snippet 2, it appears the same logic error affected Snippet 3 in the same way. However, the first link ended up appearing in the end in the return because the program regressed and recognized the first link at the very end of the call. 

For the rest of the markdown file, the problem is much more complex. It appears the logic regression is a result of the inability to properly deal with the missing closed parenthesis + the lack of handling empty space.
While the process of fixing the code seems nuanced, the first thing that would help my code would be figuring out how to eliminate empty space.
