---
layout: post
title:  "A simple testing checklist"
categories: testing best-practices
---

> Code without tests is broken as designed.
&mdash; Jacob Kaplan-Moss

But testing is hard. Do you find it hard to know what to test in a piece of code? I know I do sometimes. Do you wish you had some mental or physical checklist to make it easier?

Here is one that you may find useful. Feel free to take what you may of it and remix to create your own flavour. Also please do share tips and tricks on testing you have found useful (for you see, I'm but a newbie in this thing). 


Anyway, the checklist. I have put them in question form. Let your test cases answer them with those nice green checks we all love to see.

## Does the code work?
By 'work', I mean does the code do what it is meant to do, according to the requirement or specification? Add some test cases that question or verifies your assumptions about the problem you are solving. A passing test is a witness you can call to defend your implementation. At least, for now.

## Does the code fail? On wrong inputs?
Okay, so the code works well given the right inputs. But does it fail when given the wrong ones? And if yes, how does it fail? Does it fail gracefully or does it leave behind a trail of broken wares after failing? Beware of code that treats good and bad inputs the same. It is a ticking time bomb waiting to explode in the face of your users and make them very very unhappy.

## Does it handle edge cases well?
What is an [edge case](https://en.wikipedia.org/wiki/Edge_case)? Well, let's just say for the sake of simplicity that it is the values at the  extreme ends of the input type for the code. Also, they are the less-likely-to-show-up inputs to the code. But they can cause a scene when they show up. For example, if the code accepts strings as inputs, how does it handle an empty string? On the other end, how about very long strings? Or more interesting, what happens if a non-latin string shows up to the party?

## Are the tests fast?
If not, why not? A unit test should be fast, I think. A usual culprit for slow unit tests is some network request going on. Find them and [mock them](https://stackoverflow.com/questions/2665812/what-is-mocking).
There might also be other reasons for a slow test. A large test case for example.

## Conclusion
There it is. A simple checklist for testing that I bet, is not hard to follow. Thanks for reading.


_Fun little fact: I wrote this article and lost it when I refreshed the page. I had assumed (and wrongly so) that the editor was auto-saving as I wrote along. If only I had tested that assumption :))_

This post first appeared on [dev.to](https://dev.to/mekicha/a-simple-testing-checklist-2fmj)