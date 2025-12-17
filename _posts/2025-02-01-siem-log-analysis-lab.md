---
layout: post
title: "SIEM Log Analysis – Failed Login Investigation"
date: 2025-02-01
categories: [lab]
tags: [siem, blue-team, log-analysis]
description: "Analysis of failed login attempts using SIEM logs and detection logic."
permalink: /siem-failed-login-investigation/
---

## Overview
This lab focuses on analyzing authentication logs to identify suspicious
failed login patterns using a SIEM-like approach.

## Objective
- Detect abnormal login behavior
- Identify possible brute-force attempts
- Understand log correlation logic

## Environment
- Log source: Authentication logs
- Platform: Simulated SIEM environment
- Operating system: Linux

## Detection Logic
Indicators used:
- Multiple failed logins from the same IP
- Short time intervals between attempts
- Targeting a single user account

## Analysis
During the analysis, multiple failed authentication attempts were observed
originating from a single IP address within a short timeframe.

This behavior aligns with common brute-force attack patterns.

## Conclusion
The event was classified as **suspicious activity**.
In a real SOC environment, this would trigger:
- Alert escalation
- IP reputation check
- Account protection actions

## Lessons Learned
- Importance of log normalization
- Value of correlation rules
- Early detection reduces risk
