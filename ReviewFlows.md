# Review Flows

This chapter outlines the code review flows we will honour during our development processes.

### All Code Shall Be Reviewed

Every commit should be initially sent to a feature or fix branch, which is then opened as a PR against master. There is no alternative to this and nobody, including the mobile tech lead, is exempt from this rule.

### Everyone Should Review

Every PR should have a single assignee but the rest of the mobile team should be added as a reviewer so they get the notification to read through the proposed code change. This will enable better communication within the team and will give everyone a chance to benefit from each other's code and thoughts.

### PRs Should Be Compact But Detailed

Every PR should include at least one link to the related ticket or tickets. Changes that have UI impact should include a screenshot or a video / GIF that outlines the proposed changes. On the other hand, proposed code changes should be as small and compact as possible, to prevent reviewer fatigue.

### Commit Messages

Commit logs are very useful for reviewing activity within a PR, a branch, or even a release. We should do our best to keep our commits readable and descriptive, so they benefit other team members.

A good commit should answer the following questions:

* Why is this change necessary?
* How does this commit address the issue?
* What side effects does this change have?

Our proposed commit format is as follows:

    Summary line with short description of change
    
    Ticket link or other relevant context link
    
    Detailed explanation of the change, along with side effects
    
    * Optional list of implementation details
    * Or other list elements

Here is an example:

    Fix breadcrumb animation bugs in refill flow
    
    https://hipo.codebasehq.com/projects/avocare/tickets/353
    
    Breadcrumb animations were broken in some cases, these changes will ensure 
    that breadcrumb animates smoothly with every navigation push/pull.

Don't use `git commit -m "Fix the fixes"` format, since it will put you in the wrong mindset in terms of commit formatting and length.
