# ShiftLeft September 2019 Updates

## ShiftLeft Ocular

**[New Tutorial on Identifying Incorrect or Zero Memory Allocation Bugs in C](..using-ocular/tutorials/c-allocation-bugs.md)** Arithmetic operations can lead to integer overflow or the arithmetic operation computes to zero. These conditions may lead to exploitable vulnerabilities. This tutorial uses ShiftLeft Ocular to determine if such a condition exists.

## ShiftLeft Dashboard

* **[Improved UI](../using-inspect-protect/using-dashboard/app-list.md)**. The ShiftLeft Dashboard has a new and improved UI that makes it easier to find, view and understand your application's vulnerabilities as found by ShiftLeft Inspect and ShiftLeft Protect. 

* **[Filter Analysis Results](../using-inspect-protect/using-dashboard/filter-results.md)**. Instead of scrolling through a list of results, you can quickly find vulnerabilities of interest by filtering. Filters can also be saved and reused as a build rule to automatically tag a build in your CI-CD pipeline based on ShiftLeft Inspect's results.

## ShiftLeft Inspect

* **[Failing a Build Based on ShiftLeft Inspect Results](../using-inspect-protect/inspect/fail-build.md)**. A build in your CI-CD pipeline can now be automatically failed based on ShiftLeft Inspect's code analysis results. 

* **[Identifying Branch Names in ShiftLeft Inspect](../using-inspect-protect/inspect/identify-branches.md)**. You can include branch names in the analysis results of ShiftLeft Inspect. Doing so ensures that the automated analysis by ShiftLeft Inspect of these branches does not get mixed up with the analysis of the main branch. It also allows you to [view analysis results of individual branches separately](../using-inspect-protect/using-dashboard/view-results.md#displaying-results-by-branch-name).

* **[Improved Analysis Results Notification](../using-inspect-protect/using-dashboard/view-results.md)**. When you submit an application referencing a part of a library ShiftLeft has not seen before, it may take ShiftLeft Inspect additional time to complete a full analysis. In those situations, ShiftLeft Inspect initially displays the partial analysis results, which is indicated by a message in the UI. From this message, you can specify that you want to receive notification when the full analysis results are available.

## ShiftLeft Protect

* **[Improved ShiftLeft Protect Documentation](../using-inspect-protect/protect/securing-applications.md)**. ShiftLeft Protect documentation has been expanded and improved.
