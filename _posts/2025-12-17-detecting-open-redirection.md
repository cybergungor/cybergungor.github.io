---
layout: post
title: "Detecting Web Attacks 2 [Open Redirection] Walkthrough LetsDefend"
date: 2025-12-17
categories: labs
tags: labs
---

# Detecting Web Attacks 2 [Open Redirection] Walkthrough LetsDefend

## Lab Analysis Steps

1. **Connect to the lab environment** after completing the lesson  
   (access link is located at the bottom of the page).

2. **Open the log file** located at:  
   `/root/Desktop/QuestionFiles/Open-Redirection/access.log`  
   (Password: `access`).

3. **Analyze the logs**. As mentioned during the training,  
   Open Redirection attacks may contain encoded characters such as `%2f`,  
   which represents `/`. We analyze the logs accordingly.

   Look for parameters such as:
   - `?next`
   - `?url`

4. After identifying the relevant keywords, determine the **first timestamp**  
   at which the attack activity started.

   ![Open Redirection Log Analysis](/assets/images/openred1cybergungor.png)

5. **Record the findings**:

   - **Exploitation start date:** `27/Apr/2023 15:45:22`  
   - **Attacker IP address:** `86.236.188.85`  
   - **Parameter attacked:** `postID`

---

## Analysis Result

Even though it was not explicitly asked, we can conclude that the attack  
was **not successful**.  

- The HTTP response returned **400 Bad Request**.  
- The **byte size** of the response is small, indicating that the operation failed.  
- The page sent a **warning message**, showing that the redirection was not processed.

This walkthrough demonstrates how to **identify open redirection attempts**  
from logs, determine the attacker’s IP, the parameter targeted, and confirm  
whether the attack succeeded.
