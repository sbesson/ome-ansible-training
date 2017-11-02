---
title: "Overview"
teaching: 15
exercises: 0
objectives:
- "Learn "
keypoints:
- "What is Ansible"
- "What are inventory files"
- "Where does Vagrant fit in?"
---

This episode describes how we go about designing lessons and why.
For more information on how we design lessons and why,
see [the instructor training course][training].

## Basics

Ansible
Hosts
Inventory
Vagrant

1.  thing


3.  **Formative Assessments**:
    describe the exercises that learners will do during the lesson.
    To switch examples for a moment,
    it wouldn't be fair to ask someone to parallel park on a driving test
    if they'd never done it before,
    so two formative assessments in a driving course might be
    "back up" and "parallel park between some safety cones".

4.  **Connect the Dots**:
    put the formative assessments in order
    and develop lesson episodes to go from one to the next.
    It is common to sketch a concept map for each lesson episode,
    both to outline its key ideas
    and to check that it's not too big.
    The ordering of lesson episodes is constrained by dependencies
    but is usually not completely determined by them:
    there are often several different orders in which ideas can sensibly be introduced.
    It is common to discover a need for more formative assessments at this stage;
    to continue with the driving example,
    the lesson author might realize that a third exercise on turning while backing up is needed
    (since many people initially turn the steering wheel the wrong way when they're in reverse).

In practice, the process often looks more like this:

1.  Draft the assumptions and major outcomes.

2.  Describe the summative assessments for each half day of material
    (i.e., one summative assessment for a three-hour lesson and two for a full-day lesson).

3.  Write a one- or two-line description of the formative assessments
    building up to those summative assessments.
    These should be paced at roughly 15-minute intervals,
    i.e.,
    four per hour.

4.  Get early feedback from peers,
    particularly on how realistic the time estimates are.

5.  Do a second pass to flesh out the assumptions and assessments.

6.  Get more feedback.

7.  Start writing the lesson content.

Once work starts on step 7,
the detailed milestones should be moved into lesson episode files.
For an example of this,
see the [novice Python lesson using the gapminder data][python-gapminder].

## What Makes a Good Formative Assessment

The two purposes of formative assessment are
(a) to help learners prepare for the summative assessment and
(b) to tell them and their instructor *during the lesson*
whether they're making progress (and if not, what obstacles they have hit).
If lesson episodes are 10-15 minutes long,
then formative assessments should take no more than 5 minutes.
This means that formative assessments should be:

*   multiple choice questions,
*   Parsons Problems
    (in which the learner is given the parts of the solution in scrambled order
    and has to put them in the right order),
*   debugging exercises
    (in which the learner is given a few lines of code that do the wrong thing
    and asked to find and fix the bug), or
*   extensions of examples show in the lecture.

Good formative assessments do *not* require learners to write lots of code from scratch:
it takes too long,
there are usually too many possible right solutions to discuss in just a couple of minutes,
and many novices find a blank page (or screen) intimidating.

{% include links.md %}
