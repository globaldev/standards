# Code Reviews and Pull Requests

All code that gets deployed on to our platform must go through at least one
phase of code review.  This guide details how we perform code reviews and
includes information for both reviewers and those requesting a review.

Please note: this is a guide only, and there will be plenty of times where
something doesn't quite fit in to what's described here.  The important part is
that the general principle of code review is applied.  Also, how we implement
code reviews is always an open topic for discussion.  If you've got a question
or an idea on how to improve things, speak up.

## Why Perform Code Reviews?

Performing code reviews is an incredibly beneficial part of the development
process.  Just some of the benefits of codes reviews include:

* Helping everyone learn: not just about the system, but about development
  techniques in general.
* Avoiding tunnel vision from the implementor: it's all too easy to have
  blinkers on while coding, and not thinking about wider issues.
* Heading off bad design decisions before they get too ingrained within the
  implementation, and therefore more expensive to change.
* Ensuring adherence to coding and development standards.
* Catching bugs that otherwise wouldn't have been picked up until further in to
  the development cycle (or at all!).
* Ensuring adherence to security standards, and avoiding falling into any traps
  which may leave our systems vulnerable.

## Types of Code Review

Someone may ask for a code review for a number of different reasons.  In
general, we break these reasons down in to the following three categories.

### Thirty Percent

This type of review is less of a "code" review and more of an "approach"
review.  You've thought about a problem and have created an initial spike that
demonstrates your thinking, but you want to get feedback before committing
yourself.  Asking for a review at this time is incredibly important because it
can help steer the direction of your work before you've gone too far and have
to backtrack.  The earlier that a bug (or an incorrect approach) can be
spotted, the cheaper it is to correct course.

The reviewer should ignore things like syntactic errors, inconsistencies with
agreed coding standards etc, and assume that these will be fixed down the line,
and instead focus on:

* **Reasoning**
    * Has the developer understood the problem at hand?
    * Does the approach taken actually solved the problem?
* **Approach**
    * Is the approach taken generally correct?
    * Are there any significant edge cases that need to be considered?
* **Architecture**
    * Is the general code layout in keeping with the rest of the application?
    * Has a sensible breakdown of classes and modules been used?
* **Security**
    * Has the developer considered any potential
      [security implications](security.md)?

### Ninety Percent

This type of review is all about catching problems before they get into
production.  The reviewer should be going over the finer details and
highlighting any problems they find.  The assumption here is that the approach
has already been ratified 

* Catching implementation bugs
* Coding style inconsistencies, including breaking coding standards
* Typos
* Potential security holes
* All the little details

### Specific

Other than more general approach and code reviews, you might want to home in on
one particular area that you feel you need assistance on.  You're more likely
to want to do this if you are:

* working on an area of the system that's new to you
* working on part of the system where you know one or two other people have
  very good knowledge
* using a language that's outside your normal remit (e.g. a CF dev doing JS)
* unsure of the security ramifications of the work you're undertaking

Specific review points like this should work via the same pull request as more
general reviews.  Simply [@-mention][atmention] one or more users and ask them
the specific questions you need answering.

[atmention]: https://github.com/blog/821

## Requesting a Code Review

We use a branching model which follows [GitHub Flow][ghflow], and use GitHub's
[Pull Requests][prs] as a means to perform code reviews. 

The creation of a new pull request is effectively a request for a code review of some format on what
you've submitted. 

To give the whole team visibility it's important to post a link to the pull request in the relevant [Slack][slack] channel e.g. post ruby work in #ruby. This allows people to both review your changes and keep up to date with different projects.

[slack]: https://globaldev.slack.com
[ghflow]: https://guides.github.com/introduction/flow/
[prs]: https://help.github.com/articles/using-pull-requests/

### Creating a Pull Request

Think of raising a pull request to be like [crafting a commit
message][crafting].  It should include both a summary (title) and a detailed
description of what the pull request contains.  The title short be short and to
the point: a good rule of thumb is that if the GitHub interface line-wraps the
title then it's too long.

[crafting]: http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html

The description should include the following pieces of information:

* **What the changes do.**  A reasonably detailed description of what your
  changeset does.  A small change does not necessarily equal a small
  description.  Give any background as to why you've implemented it in the way
  you have.  Try to preempt any questions of "why have you done it like that?"

* **Why those changes are required.**  This is very important for the review
  process: it gives a reviewer who might not be as familiar with the
  requirements as you are the background they need to perform a good code
  review.  By all means link to Jira tickets or other pull requests, but
  include a text description too, so the reviewer has everything they need in
  one place.

* **What you expect to get out of the code review.**  This bit is often
  forgotten, but it's very important to specify what it is you're expecting to
  get out of a code review.  At the very least you should highlight if you're
  asking someone specific to review something, if you're asking for an approach
  review ("Thirty Percent"), or a final pre-merge review ("Ninety Percent").

* Any other detail you think will help either a reviewer or someone looking at
  the pull request in the future to understand the reasoning behind the change,
  such as a brief set of annotations guiding a reviewer through the changes:
  which files to look at first; defence of the reasoning and methods behind
  each modification.

Remember that once a pull request is closed, it forms part of the documentation
of the system you are working on, so ensuring the description includes all the
details above is as much of a maintenance issue as it is for code review.
Imagine how good it would be in six months' time to track down a specific pull
request, to discover a complete story of what the change introduced and why it
was made?

## Performing a Code Review

* Give it the time it needs: if you can't review right away, let the submitter
  know.
* Keep your review points relevant to the type of review that's been requested.
* If you're criticising, be constructive.  Don't just say "this is wrong," but
  explain why, and give an example of how the work could be improved.
* Be patient, clear and concise. As the submitter, assume that everyone's
  trying to help, not that they're just there to rubbish your hard work.
* Be positive and highlight things you like or have learned, rather than just
  problems with the submission.
* Avoid falling in to religious discussions on the One True Way™ to implement
  X.

## Code Review Checklist

The following is a checklist for items that should be covered through code
review of a piece of work. It is not exhaustive, and does not go in to detail
about each area, but should act as a guide:

### Approach and Architecture

* Has the developer understood the problem, and does the code under review
  solve that problem?
* Is the architecture suitable for the scale of the problem?
* Is code being reused and not duplicated?

### Maintenance

* Will the code be easy to maintain by someone other than the original author?
* Has appropriate documentation been created?

### Coding Standards

* Does the code follow the global development guidelines?
* Does the code meet the coding standards set for the appropriate language?

### Security

* Is all user input sanitised and escaped before being output back to the user?
* Are there any passwords or secret keys stored within the source-controlled
  code or configuration files?
* Have SQL injection attacks been mitigated by using the appropriate
  language-specific approaches?
* Have newly-introduced external dependencies been vetted for their security
  and performance impact?
* Is any logging sanitising sensitive information?

### Performance and Data

* Will the code perform well under the expected, production-level load?  This
  includes checking approach, algorithms and integration with external services
  such as adding appropriate SQL indexes.
* Is data being either permanently or temporarily stored unnecessarily?

### Testability

* Does the approach lend itself well to being easy to test, either through
  unit, integration or black-box testing?
* Are appropriate automated test cases included?
* Are all code paths tested?

## Frequently Asked Questions

### When Should I Ask for a Code Review?

Early and often, and whenever you need input.

### Who Should Perform a Code Review?

Everyone.  If you don't understand something, ask for an explanation.  In
general, if you need to ask, then it's a sign to the person requesting the
review that either their description within the pull request isn't good enough,
or that the code itself isn't clear.

Code reviews are the perfect arena for not just catching bugs, but also sharing
information and helping everyone learn.  It's worth skimming over pull requests
in areas of the platform that you're not working on, just to keep up to date
with the sorts of things that are going on across the team.

### What's the Best Size for a Code Review?

In general: the smaller the better.  If you've found yourself working for a
couple of days without asking for a review, then you've probably left it too
late.  Smaller pull requests are easier for the reviewer to understand; with
larger requests, it's far too easy to miss something.  In general, remember
[this adage][bigreview]: "Ask a programmer to review 10 lines of code: they'll
find 10 issues. Ask them to do 500 lines and they'll say it looks good."

[bigreview]: https://twitter.com/girayozil/status/306836785739210752

### What if I'm Not Happy with Someone's Comments?

To start with, assume everyone's trying to help, not to shoot you down.  If
you're uncomfortable or not happy about how someone's giving you feedback in a
code review, then talk to them about it.  Don't just grumble.  This whole
process is about improving communication, so actually walk over to the person
in question and speak to them about it.  If you're not in the same location,
prefer Skype / Hangouts over IM.

## Further Reading

* OWASP have produced a [Code Review Guide][owasp-guide] which covers the
  mechanics of reviewing code for certain vulnerabilities, and provides limited
  guidance on how the effort should be structured and executed.  V2 is
  currently being compiled, but V1.1 is complete (although it will be somewhat
  out of date, having been last published in 2008).
* SmartBear have published a whitepaper containing [11 Best Practices for Peer
  Code Review][sbear-guide]; this covers the size and speed of code reviews,
  developer annotations to guide reviewers, review metrics and other items to
  improve code reviews.

[owasp-guide]: https://www.owasp.org/index.php/Code_Review_Guide
[sbear-guide]: http://smartbear.com/SmartBear/media/pdfs/WP-CC-11-Best-Practices-of-Peer-Code-Review.pdf
