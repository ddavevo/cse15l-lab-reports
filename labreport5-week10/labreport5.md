# Lab 9: Large-Scale Testing
`by Dave Vo | 5/1/22`

## Using vimdiff to Find Differences in Test Results

I can run the command `$ vimdiff ~/<MyMarkdownParseDirectory>/results.txt ~/<OtherMarkdownParseDirectory>/results.txt` to examine the similarities and differences between running MarkdownParse with both implementations. 

It appears for test file [142.md](https://github.com/ddavevo/markdown-parser-davevo/blob/c49348499930cec68465f8043d4259e678ed6070/test-files/142.md), my implementation causes an infinite loop while the latter did not detect any valid URL links.

![](1.1%20vimdiff-infiniteloop.png)

And for test file [367.md](https://github.com/ddavevo/markdown-parser-davevo/blob/c49348499930cec68465f8043d4259e678ed6070/test-files/367.md), my implementation returned a faulty link while the latter did not.

![](1.2%20vimdiff-unnecessary-detection.png)

## Expected Outputs & Actual Outputs

```
Expected (142.md)
```

Using commonmark preview, I can see how the
markdown files are interpreted and determine whether MarkdownParse should return anything.

For `142.md`, since the markdown does not hold any semblance to the `[linkName](urlHere)` format, the expected result should be `[]` since there are no valid links to return.

![](1.1%20142-foofunction.png)

```
Actual (142.md)
```

 My implementation got stuck in an `OutOfMemoryError`, thus no return value to examine.

![](2.1.%20142-OutOfMemory.png)

Conversely, the latter implementation runs without any errors and correctly returns `[]`.

![](3.2.%20142-CorrectOutput.png)

```
Expected (367.md)
```
And for `367.md`, the same situation should happen here. `*(*foo)` is not a valid link, thus the return should be `[]`.

![](1.2%20367-foostar.png)

```
Actual (367.md)
```

My implementation incorrectly incorrectly mistook a non-link `*foo` for a valid link and returned `[*foo]`.

![](3.1%20367-InvalidLink.png)

The latter demonstrates the proper result `[]` by not returning it as a link.

![](3.3.%20367-CorrectOutput.png)

## Deconstructing One of the Bugs: My MarkdownParse Implementation for 142.md

Recall my implementation for MarkdownParse. When running it on test file `142.md`, an `OutOfMemoryError` occurs. 

![](2.1.%20142-OutOfMemory.png)

Looking at the stack trace and how the lines run, the `OutOfMemoryError` occurs because this while loop condition never gets satsified.

![](2.2.%20142-WhileLoopCondition.png)

In the previous image, the while loop ran when `currentIndex = 18` already. It is evident that `currentIndex`, the while loop condition did not actually increase from what it started as.

![](2.3.%20142-CurrentIndexUpdates.png)

Arriving at line 37, where the program faced issues, we see that running `step` again initiates the cycle again. This confirms how  the `OutOfMemoryError` is happening.

![](2.4.%20142-Loop.png)

## Fixing The Code: Where to Start

To start, the fix requires ensuring that the while loop condition does increase so that the loop eventually ends.

![](2.5%20142-ProblematicCode.png)

Also recall what the test file `142.md` looks like. MarkdownParse works by pinpointing the indices for brackets and parentheses. However, there are only 1 pair of parentheses. There is also additional text after without any `[]` or `()`, which does not help in progressing `currentIndex` to the end of the markdown file.

![](1.1%20142-foofunction.png)