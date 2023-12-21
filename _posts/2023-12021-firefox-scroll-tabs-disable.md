---
layout: post
title: Disable scrolling tabs within Firefox
date: 2023-12-21 0500
tags: firefox, browser, tabs, abour config
---

The scrolling tab feature in Firefox hasa been bugging for ages, I know it's a personal prefernce lots of tiny tabs which are hard to distunguish is bad ui too. But I prefer it that way, I like to be able to see what is going on and tabs getting hidden behind scrolling just didn't work for my brain.

I wen looking and found many hacky ways to do this, whcih were fine but involved adding custom CSS which is not really desirable. So I wen spelunking in `about:config`.  

In here there is the paramater `browser.tabs.tabmanager.enabled` which when it's value is toggled to `false` the scrolling is disbaled and you get all tabs on screen at once.

For good mesaure I also changed the value of `browser.tabs.tabMinWidth`down to `20` too
