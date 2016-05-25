---
layout: post
title:  "Never Use TFS"
date:   2015-11-08
categories: blog

cover_image: Tfs.PNG

excerpt: "I've just spent three months using TFS, for the first  time, on an enterprise scale project, and I've come to the conclusion that TFS should never be used ever."
---
I've just spent three months using TFS, for the first  time, on an enterprise scale project, and I've come to the conclusion that TFS should never be used ever. I'm specifically talking about TFS as version control (TFVC), but based on my experience there, I can only assume the rest of it is rubbish too. This is why:

# Substandard
* There are no local commits or local branches! Git allowed us to use version control for our individual work when it arrived in 2005. In 2015 it's ridiculous that I can't version and branch my local changes. Ok, I can shelve my changes instead of committing or branching locally, but you can't collaborate on shelvesets. You can't cherry-pick between different shelvesets. You can't add a shelveset on top of shelveset and maintain the distinction.
* No cherry-picking between branches, and it's oddly difficult to apply a shelveset to a different branch.
* TFS thinks things have changed even when I've changed them back to exactly how they were, cluttering my commit: this seems to be based on the timestamp of last change, not on the actual difference
* Only one repo: can't commit demos, examples, other resources (maybe commit to \Documents?)
* Not distribute, so you only need one server to go down and work comes to a standstill. Compounding this is the fact that if you have your workspace on the server rather than local (and there are various reasons you might do this) then visual studio wants to talk to the server all the time, and things really slow down if it isn't reachable.

# Ridiculously tight coupling with Visual Studio
* TFS doesn't know about changes made other than through Visual Studio - so if you want to use other tools to manipulate files, you risk losing those changes, or confusing TFS.
* Analysts, testers, designers or other non-devs need to use Visual Studio to use it, requiring even more money on licences.
* Slows Visual Studio down quite a bit, and sometimes crashes it.
* You need to link each project to TFS explicitly, which means Visual Studio will make and maintain changes to the solution file which specify the source control being used. I don't think code and source control should be coupled like this, and it makes it hard to use any other client than visual studio.

# Default diff and merge engine both suck
[But at least you can replace it with e.g. tortoise diff/merge.](http://blogs.msdn.com/b/jmanning/archive/2006/02/20/diff-merge-configuration-in-team-foundation-common-command-and-argument-values.aspx)
Even once you've done this, there doesn't seem to be a way to view whitespace changes (regardless of the diff engine you choose)

# Unreliable
* Getting latest won't overwrite local changes nor will it replace files which think they are the latest version. So if someone made a change to a file you are working on, you can easily overwrite their change when you next commit, and never realise.
* Getting latest for a single file (or subset of files) will make it think all files are up to date because it looks at the timestamp of the most recent get. Note: I haven't verified this and it's hard to recreate reliably, but it is widely reported on the web, and I saw it happen once or twice to certain files. You'll notice it is the habit of anyone who has used TFS for more than a few months to always update all files at once and using the "get specific version -> latest version" option, because they have learnt not to trust TFS.

# Branches suck dangerously hard
* Switching and merging branches is slow and conflict-ridden, probably worse than SVN, hence the need for "shelvesets" to exist
* [Any and all folders can be branched independently!](https://msdn.microsoft.com/en-us/library/ms181425.aspx) This is extremely dangerous unless handled very carefully - some parts of your code might be on a different branch to other parts, and some parts may contain branch-specific changes but not actually be branched. You could argue that it makes for a lot of flexibility, but I've only ever seen it cause chaos.

# Peer Review Workflow
TFS doesn't integrate with Crucible or many other decent review tools, so you're kind of stuck with the built in TFS tool. Fortunately, the review tool is generally very good, with a few drawbacks:
* You need a Visual Studio Ultimate licence to do reviews in Visual Studio. You can do them on the web instead, but... 
* Web comments don't show in Visual Studio reviews and vice versa
* If you don't have the VS licence, you can't be invited to reviews, so there's an extra manual "send an email" step for everyone
* Can't add multiple revisions to the same review, or create a review for the changes since the last review - this is partly due to the limited nature of shelvesets

# Other Issues
* CI build failure reports don't link to the relevant changeset(s). (Well ok, the web pages do, but you have to hunt. Emails don't.)
* TFS doesn't delete empty folders, which leaves clutter
* Can't specify lock-before-edit rules for e.g. only documents, or with a local workspace
* Users are closely tied to Active Directory: so you lose all the history when migrating to a new AD system
* There is little or no rescue from git clients like Git-TFS, which give you local commits and branches, but have other downsides because of the strange quirks of TFS. I'll go into my experience with Git-TFS in [a later blog post](/blog/2016/05/25/git-tfs/).


# PROS

But wait, there must be some good things about TFS? These are the ones I can think of...

* It's simple for analysts/testers, with nice easy GUI in VS: git might be too complex for some. But that means non-devs need a full clunky Visual Studio with the associated licence cost, just to check a few files into source control. And there are lots of gui tools for git now that make simple pull/commit/psuh workflows nice and easy for everyone.
* The Visual Studio integration is excellent, and peer reviews are good if you all have the same VS licence. But as discussed earlier, it slows down the IDE, divides the team unless everyone has the same licence, and means one tool does too much.
* You can commit a review directly from the shelveset, a bit like merging a pull request. This is a nice way to make sure what was reviewed is what gets committed. I'm not sure what happens if there are merge conflicts though. Anyway this can be achieved with a pull request system or a workflow like [Gerrit](https://code.google.com/p/gerrit/).
