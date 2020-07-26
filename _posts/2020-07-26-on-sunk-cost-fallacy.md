---
layout: post
title: How Sunk-Cost fallacy is making us write bad code
date: 2020-07-2]
excerpt:  We are maintaining code just because it is already there. We were forced to write new features to work around the old implementations. No one is ready to let go even if we know that's the right choice. My take away from this is that it is harder to remove code than to add new code. A development team should be really careful about the new code they're adding and vigilant about removing any old code that's unnecessary really soon.
tags: [software, code, maintenance, clean code, sunk-cost ]
published: true
---

We're a team of 7 building an enterprise application with Angular and Angular Material. We have different plans and features that can be toggled for each plan using a set of feature flags. We built granular controls such that, if required the user can be allowed to view a feature but not to interact with or change it. Everything worked great and we've built around 60 such feature flags.

We introduced a new feature that comes inside a set of tabs we already have as basic features. This new feature is supposed to be visible as an extra tab in the middle of other basic tabs.

<iframe class="external-embeds" src="https://stackblitz.com/edit/material-tabs-example-working?embed=1&file=src/app/app.component.ts&view=preview"></iframe>

The above stackblitz is a stripped down version of what we have in our codebase. It worked well in our testing and we were ready to ship it to staging that day and to production later that week. But we never checked if the contents of the individual tabs were rendering if the feature tab is turned off. The tab labels were in place and it felt enough to move forward as other tabs were untouched. In the final rounds of testing, one of our team members noticed a weird issue. Some of the tabs were not rendering the contents inside them.

It was a Friday evening and almost everyone has already shifted into the weekend mindset. This got everyone on their heels and we started debugging it. There were few API timeout issues in the console but that can't be breaking the rendering. We handled API errors gracefully for all the known cases. Any unknown errors will be caught and will be turned into something human readable. So it has to be something else.

We were trying to optimize the initial loading time and one of the change is to delay the off-screen components rendering until they have opened. It's an experiment that's happening in it's own branch. By any chance, that code got into development branch? I checked the commit history and ruled this out.

Luckily the same dev who discovered the issue also figured out why this is happening. We used to use bootstrap with angular and later migrated our codebase to Angular material. Our bootstrap implementation of tabs is just divs toggled with conditions based on the current selection. The conditions that hide and show the divs in bootstrap were copied when we migrated to Angular material. The divs were using a hidden attribute that checks the active tab. When we added the optional tab in the config object and toggled it, the feature tab is removed from the DOM but the config object still maintained the same tab order and count. So when we open each tab the inner condition was always false hiding the contents of the tab (Check app component and its template in the below example).

<iframe class="external-embeds" src="https://stackblitz.com/edit/material-tabs-example-not-working?embed=1&file=src/app/app.component.ts&view=preview"></iframe>

Once we found the issue fixing it was a debate. When asked no one had a clue about why the condition and config are there. The code is sitting there for a while and it bothered no one. The existence of a code block implies that it is tested and it is important. Everyone knows that removing those conditions is the correct solution. But we also kept asking what if we added that to solve something else? What if the condition is removed and the tabs fail in some other edge case? As a fix, we couldn't immediately remove the tab object and the hidden attribute from the inner content div. Instead, we rewrote the tab object by taking the optional feature tab into account.

This is a great example of the [sunk-cost fallacy](https://dictionary.cambridge.org/us/dictionary/english/sunk-cost-fallacy). We are maintaining code just because it is already there. We were forced to write new features to work around the old implementations. No one is ready to let go even if we know that's the right choice.

My takeaway from this is that it is harder to remove code than to add new code. A development team should be really careful about the new code they're adding and vigilant about removing any old code that's unnecessary really soon. Removing old code may not be easy as it sounds even from a well-written codebase. I'm actually a little late to the party, there are few great resources out there explaining how problematic old code can be such as [@swyx's](https://twitter.com/swyx) tweet here.

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Gresham&#39;s Law applied to code: <br><br>Hard-to-Delete code drives out Easy-to-Delete code over time.<br><br>&quot;Technical Debt&quot; is the wrong metaphor. When we are so scared of our own codebase that we put on bandage after bandage, we should worry about &quot;Technical Mummification&quot; instead.</p>&mdash; shawn swyx wang (@swyx) <a href="https://twitter.com/swyx/status/1286624921532080129?ref_src=twsrc%5Etfw">July 24, 2020</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script> 

I found this [Programming is terrible article](https://programmingisterrible.com/post/139222674273/write-code-that-is-easy-to-delete-not-easy-to) that talks great length about writing code that is easy to delete. In my experience, Code that is easy to delete is mostly well written and structured code. I hope this article and the linked resources provoke some thoughts on your mind. Do you have any similar stories? Share with me in the comments.
> Note 1:  To handle the legacy implementation around the tabs, I created a new task in Jira and now testing it after rewriting it. So it is getting removed after all, but with thorough testing.

> I recently [tweeted](https://twitter.com/ShivEnigma/status/1285239421323771904?s=20) about the idea of writing in public, that's after my experiments with learning and building in public. The idea is to publish the blog post from a very early draft stage with a draft warning and iterate the published draft to make it better, this is my attempt at reducing the time it takes to publish articles. What's your thoughts on this?
