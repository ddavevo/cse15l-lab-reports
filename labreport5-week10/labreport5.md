# Lab 9: Large-Scale Testing
`by Dave Vo | 5/1/22`

## Using vimdiff to Find Differences in Test Results

I can run the command `$ vimdiff ~/<MyMarkdownParseDirectory>/results.txt ~/<OtherMarkdownParseDirectory>/results.txt` to examine the similarities and differences between running MarkdownParse with both implementations. 

It appears for test file [142.md](https://github.com/ddavevo/markdown-parser-davevo/blob/c49348499930cec68465f8043d4259e678ed6070/test-files/142.md), my implementation causes an infinite loop while the latter did not detect any valid URL links.

![](1.1%20vimdiff-infiniteloop.png)

And for test file [367.md](https://github.com/ddavevo/markdown-parser-davevo/blob/c49348499930cec68465f8043d4259e678ed6070/test-files/367.md), my implementation returned a faulty link while the latter did not.

![](1.2%20vimdiff-unnecessary-detection.png)

// Which Implmentation(s) are Correct

Using commonmark preview, I can see how the
markdown files are interpreted and determine whether MarkdownParse should return anything.

For `142.md`, since the markdown does not hold any semblance to the `[linkName](urlHere)` format, the expected result should be `[]` since there are no valid links to return.

Referring back to my vimdiff analysis, the latter implementation correctly does not return anything while my implementation got stuck.

![](1.1%20142-foofunction.png)


And for `367.md`, the same situation should happen here. There are no valid links, thus the return should be `[]`.

The vimdiff shows that the latter demonstrates the proper result while mine incorrectly mistook a non-link for a valid one.

![](1.2%20367-foostar.png)


// Show both acutal outputs & expected outputs (links expected)

// For one, describe the bug, what's wrong with the program, and show what code should be fixed.