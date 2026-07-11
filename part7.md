# Burp Suite Series — Introduction, Editions & Features Overview (TryHackMe Walkthrough)

> **Course:** Cyber Security — Web Application Penetration Testing
> **Topic:** What is Burp Suite, its Editions, Core Features Overview, and a Guided TryHackMe Practical Challenge
> **Reference room used:** TryHackMe — "Burp Suite: The Basics"

---

## Table of Contents

1. [Introduction](#1-introduction)
2. [What is Burp Suite?](#2-what-is-burp-suite)
3. [Burp Suite Editions](#3-burp-suite-editions)
   - 3.1 [Community Edition](#31-community-edition)
   - 3.2 [Professional Edition](#32-professional-edition)
   - 3.3 [Enterprise Edition](#33-enterprise-edition)
   - 3.4 [Editions Comparison Table](#34-editions-comparison-table)
4. [Core Burp Suite Features — Overview](#4-core-burp-suite-features--overview)
5. [Setting Up the Proxy (Recap)](#5-setting-up-the-proxy-recap)
6. [The Target Tab in Detail](#6-the-target-tab-in-detail)
   - 6.1 [Site Map](#61-site-map)
   - 6.2 [Scope](#62-scope)
   - 6.3 [Issue Definitions](#63-issue-definitions)
7. [Navigation Shortcuts](#7-navigation-shortcuts)
8. [Practical Challenge: Finding the Hidden Flag via Site Map](#8-practical-challenge-finding-the-hidden-flag-via-site-map)
9. [Quick Knowledge-Check Q&A (from the TryHackMe room)](#9-quick-knowledge-check-qa-from-the-tryhackme-room)
10. [Summary / Quick Revision Table](#10-summary--quick-revision-table)
11. [Appendix: Additional Burp Suite Topics Not Covered in This Series](#11-appendix-additional-burp-suite-topics-not-covered-in-this-series)

---

## 1. Introduction

This video serves as a **true beginner's introduction** to Burp Suite — intended specifically for those who are starting **completely from scratch** in cybersecurity and have never used Burp Suite before.

> If you already know Burp Suite's basics (installation, general usage, feature overview), this specific video is not for you — refer instead to the more advanced, feature-specific videos in this series (Proxy, Repeater, Intruder, Sequencer, Decoder, Comparer, etc.).

This walkthrough is based on the **TryHackMe** room titled **"Burp Suite: The Basics."**

---

## 2. What is Burp Suite?

In the simplest possible terms:

> **Burp Suite is a web application security testing framework.**

It works as a **"middle man"** (proxy) between your system (the **client**) and the target web application's **server**.

```
   Client (Your System)  ◄──────►  Burp Suite  ◄──────►  Server (e.g., facebook.com)
```

- Every request that leaves your system heading to a web server (e.g., visiting `facebooksignup.in`) can be routed **through Burp Suite first**.
- Burp Suite **intercepts** this traffic — meaning it captures the request before it continues on to the actual server — giving you the opportunity to inspect, modify, or analyze it before deciding whether to let it proceed.

**Why it matters for interviews:** Burp Suite is extremely popular in the cybersecurity/web pentesting field — it's very likely to come up in interview questions such as: _"What is Burp Suite? Who created it? What is it used for? How many editions does it have?"_

---

## 3. Burp Suite Editions

Burp Suite comes in **three main editions**:

### 3.1 Community Edition

- **Free** for all users.
- Lets you perform **most of the same core web pentesting activities** available in the paid Professional edition.
- **Key limitation: Rate limiting.**
  - Example: If you're performing a **brute-force attack** on a login form, the Community Edition **restricts how fast and how many requests** you can send at once (e.g., a cap like 250 or 400 requests, with intentionally reduced speed).
  - Despite this limitation, it remains extremely useful for **beginners and intermediate learners** to practice and learn web pentesting concepts.

### 3.2 Professional Edition

- A **paid** edition requiring an **annual subscription/fee**.
- Removes the rate-limiting restrictions found in Community Edition, among other professional-grade features (e.g., the automated **Scanner**, faster Intruder attacks, session handling rules, etc. — see Appendix).

### 3.3 Enterprise Edition

- Designed for **company/organizational-level use** — typically used by larger companies.
- **Key feature:** Continuous, automated **vulnerability scanning** of an organization's web applications, mobile applications, and websites — similar in concept to how tools like **Nessus** are used for continuous vulnerability scanning of infrastructure.
- It **continuously monitors** all of a company's live web/mobile applications and proactively identifies vulnerabilities over time, rather than requiring manual, one-off scans.

### 3.4 Editions Comparison Table

| Edition          | Cost                  | Best For                                 | Key Trait                                         |
| ---------------- | --------------------- | ---------------------------------------- | ------------------------------------------------- |
| **Community**    | Free                  | Beginners, learning, individual practice | Rate-limited (restricted speed/volume on attacks) |
| **Professional** | Paid (annual fee)     | Individual professional pentesters       | No rate limits; full feature set                  |
| **Enterprise**   | Paid (organizational) | Companies/organizations                  | Continuous automated scanning across many apps    |

> **Note:** Burp Suite is used for testing **both web applications AND mobile applications.**

---

## 4. Core Burp Suite Features — Overview

While Burp Suite has many features, here are the most important/popular ones (each covered in dedicated videos elsewhere in this series):

| Feature       | Purpose                                                                                                                                                                                                               |
| ------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Proxy**     | Acts as the "middle man" — intercepts all traffic between your system and the target server, letting you inspect/modify requests before forwarding them (or send them onward to other modules like Repeater/Intruder) |
| **Repeater**  | Captures a request so it can be **modified and re-sent multiple times** — essential for testing things like SQL injection, where you need to try many different query variations against the same request             |
| **Intruder**  | Used for **brute-forcing** (e.g., login credential guessing) and **fuzzing** (sending multiple randomized requests to test an application's behavior/robustness)                                                      |
| **Decoder**   | Encodes or decodes captured data/values (e.g., URL encoding/decoding)                                                                                                                                                 |
| **Comparer**  | Compares two captured requests or responses side-by-side to identify differences                                                                                                                                      |
| **Sequencer** | Used to test the **randomness/predictability of session cookies or tokens** — determining whether a target application generates session values randomly or in a predictable (guessable) pattern                      |
| **Target**    | Shows information about your current testing target, including **Site Map**, **Scope**, and **Issue Definitions**                                                                                                     |

**Fuzzing (brief definition):** A concept in cybersecurity penetration testing where **multiple random requests** are sent to a web application specifically to test its behavior and robustness — this will be explored in more depth in future practical videos.

---

## 5. Setting Up the Proxy (Recap)

To route your browser's traffic through Burp Suite, there are **two main methods**:

### Method 1: FoxyProxy (Recommended, Easier)

- Install the **FoxyProxy** browser plugin/extension.
- Configure it once, then simply toggle it on/off whenever needed.

### Method 2: Manual Browser Proxy Configuration

1. Go to your browser's **Settings**.
2. Search for **"Proxy"**.
3. Navigate to **Network Settings**.
4. Set up a **Manual Proxy**:
   - **IP Address:** The IP of the system where Burp Suite is installed and running (e.g., `127.0.0.1` for localhost).
   - **Port:** The port Burp Suite is running on — **by default, this is port `8080`.**

**Once configured:**

- Any request sent from your browser (e.g., typing `facebook.com`) will **pause/load indefinitely** until you manually act on it inside Burp Suite.
- In Burp's **Proxy → Intercept** tab, you'll see the captured request, with options to:
  - **Forward** it (let it continue to the server)
  - **Drop** it (block it from going anywhere)
  - **Send** it to another module (e.g., right-click → **Send to Intruder**)

---

## 6. The Target Tab in Detail

The **Target** tab shows all information related to the specific target you're currently performing vulnerability assessment / penetration testing on. It has **three sub-sections**:

### 6.1 Site Map

- As you browse/visit different pages of the target website (through the proxy), Burp Suite **automatically builds a complete map** of every page/endpoint you've visited.
- This gives you a structured, tree-like view of the entire website's discovered structure — useful for finding hidden or unusual pages/endpoints.

### 6.2 Scope

- **Scope** lets you restrict Burp Suite to only capture/care about requests related to **one or more specific IP addresses or domains** that you're authorized to test.
- **Why this matters:** If you're testing `facebook.com` and it contains links/redirects to other unrelated websites, you likely **don't want to capture traffic from those other sites** — only from your actual authorized target.

**How to add a domain to scope:**

1. Right-click on the relevant request/domain (e.g., `facebook.com`) → click to add it to scope.
2. Confirm with **Yes**.
3. From then on, **only `facebook.com`'s traffic** will be actively tracked/tested — other domains' requests will be ignored, even if they briefly pass through the proxy.

**Additional setting for strict scope enforcement:**

- Even after adding a domain to scope, Burp's proxy might still technically capture other domains' traffic by default.
- To fully restrict this: Go to **Settings → Request Interception Rules**, and check the option **"And URL is in target scope."**
- This ensures Burp will **only intercept requests for domains that are explicitly within your defined scope** — all other domains' requests pass through without being captured/tested.

### 6.3 Issue Definitions

- This section contains a **reference library of common web vulnerability types** (e.g., **OS Command Injection**, **SQL Injection**, etc.).
- For each vulnerability type, it provides:
  - A description of what the vulnerability is
  - How it can typically be **fixed/remediated**
  - **References** for further reading
  - **Vulnerability classifications** — i.e., what broader category of vulnerabilities it relates to
- **Practical use:** This information is extremely useful when **writing penetration testing reports** — after identifying and fixing a vulnerability during a professional engagement, you can reference this built-in documentation to properly explain the vulnerability, its risk, and remediation steps in your final report.

---

## 7. Navigation Shortcuts

Burp Suite supports keyboard shortcuts for quickly switching between tabs:

| Shortcut             | Navigates To |
| -------------------- | ------------ |
| **Ctrl + Shift + T** | Target tab   |
| **Ctrl + Shift + P** | Proxy tab    |

_(Additional shortcuts exist for other tabs as well — these two are highlighted as the most commonly used.)_

---

## 8. Practical Challenge: Finding the Hidden Flag via Site Map

**Challenge (from the TryHackMe room):** _"What is the flag you receive after visiting the unusual endpoint?"_
**Hint given:** _"You are looking for a suspicious set of numbers."_

**Step-by-step solution walkthrough:**

1. Copy the target's request/URL as instructed by the room.
2. Paste it into your browser and press **Enter** to load the target hosting page.
3. **Important setup step:** Make sure your **Scope** settings aren't accidentally blocking the target site — e.g., if scope was previously set to a _different_ domain (like `facebook.com` from an earlier demo), you must **remove that old scope entry** first, otherwise the new target's requests won't be captured/loaded properly.
4. Once scope is cleared/corrected, reload the same target URL again — this time, it loads successfully, and its traffic appears properly in Burp Suite.
5. Go to the **Target → Site Map** tab.
6. **Manually explore/browse the entire website** — visit every visible section (e.g., "About Us," "Submit Ticket," "Contact," and any other visible links/pages).
   - As you visit each page, Burp Suite **automatically populates the Site Map** with every endpoint discovered.
7. While exploring, you'll eventually load an **unusual/hidden page** — one not obviously linked from the normal navigation.
8. Checking the **response** of that specific unusual page reveals a **flag** value.
9. **Copy that flag** and submit/paste it as the answer to the challenge.

> 💡 **Learning takeaway:** Thoroughly exploring every visible link/section of a target application (rather than just the obvious pages) — while Burp passively builds the Site Map in the background — is a simple but effective way to **discover hidden or unusual endpoints** that might not be immediately obvious, which is a common real-world reconnaissance technique in web penetration testing.

---

## 9. Quick Knowledge-Check Q&A (from the TryHackMe room)

| Question                                                                                         | Answer                                       |
| ------------------------------------------------------------------------------------------------ | -------------------------------------------- |
| Which Burp Suite feature allows us to intercept requests between our machine and the target?     | **Proxy**                                    |
| Which tool would be used to brute-force a login form?                                            | **Intruder**                                 |
| Which edition of Burp Suite runs on a server and provides constant scanning for target web apps? | **Enterprise Edition**                       |
| Burp Suite is frequently used to test **\_\_** applications. (fill in the blank)                 | **Mobile** (in addition to web applications) |

---

## 10. Summary / Quick Revision Table

| Concept                     | One-Line Summary                                                                                                       |
| --------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| **Burp Suite**              | A web application security testing framework that acts as a "middle man" (proxy) between client and server             |
| **Community Edition**       | Free, but rate-limited (restricted speed/volume for attacks like brute-forcing)                                        |
| **Professional Edition**    | Paid, annual fee, no rate limits, full individual feature set                                                          |
| **Enterprise Edition**      | Paid, organizational use, provides continuous automated vulnerability scanning across an entire company's applications |
| **Proxy**                   | Intercepts traffic between client and server; the foundation feature that routes requests to other modules             |
| **Repeater**                | Re-sends and modifies the same captured request multiple times (e.g., for SQL injection testing)                       |
| **Intruder**                | Used for brute-forcing and fuzzing                                                                                     |
| **Decoder**                 | Encodes/decodes captured values                                                                                        |
| **Comparer**                | Compares two requests/responses to spot differences                                                                    |
| **Sequencer**               | Tests randomness/predictability of session cookies/tokens                                                              |
| **Target tab**              | Contains Site Map, Scope, and Issue Definitions for the current testing target                                         |
| **Site Map**                | Auto-generated map of every page/endpoint visited on the target                                                        |
| **Scope**                   | Restricts Burp's focus to specific authorized domains/IPs only                                                         |
| **Issue Definitions**       | Built-in vulnerability reference library, useful for writing pentest reports                                           |
| **Default Burp proxy port** | 8080                                                                                                                   |
| **Fuzzing**                 | Sending multiple random requests to test an application's behavior/robustness                                          |

---

## 11. Appendix: Additional Burp Suite Topics Not Covered in This Series

Since this series has now covered **Introduction/Editions, Proxy, Target, Repeater, Intruder, Sequencer, Decoder, Comparer, Logger, and Extensions**, here are a few other genuinely important Burp Suite concepts commonly taught in fuller courses, which you may want to explore separately to round out your knowledge:

### a) Installing Burp's CA Certificate (for HTTPS Interception)

- By default, Burp can intercept plain HTTP traffic easily, but for **HTTPS** sites, your browser will show certificate warnings unless you **install Burp's own CA (Certificate Authority) certificate** into your browser/OS trust store.
- Typically done by visiting `http://burp` (or `http://burpsuite`) while the proxy is active, downloading the CA certificate, and importing it into your browser's certificate manager.

### b) Burp Scanner (Professional Edition feature)

- An **automated vulnerability scanner** built into Burp Professional/Enterprise.
- Automatically crawls and tests a target application for common vulnerabilities (XSS, SQLi, etc.) and generates a detailed report — not available in the free Community Edition.

### c) Burp Collaborator

- A service used to detect **out-of-band (OOB) vulnerabilities** — issues that don't show a visible response in the browser but instead cause the target server to make an external network request (e.g., blind SSRF, blind XXE, blind command injection).
- Burp Collaborator provides a unique domain/server that "listens" for these external interactions, confirming that a vulnerability exists even without a direct visible response.

### d) Match and Replace

- A Proxy feature (under **Proxy → Options**) that lets you automatically **find and replace** specific patterns in requests/responses as they pass through Burp — useful for automatically modifying headers, cookies, or parameters on every request without manual editing each time.

### e) Session Handling Rules & Macros

- Found under **Project Options → Sessions**, this lets you automate tasks like **automatically re-logging in** or refreshing an authentication token/cookie whenever a session expires during automated testing (e.g., during Intruder attacks) — extremely useful for testing authenticated parts of an application without manual intervention.

### f) WebSockets Tab

- Burp Suite also supports intercepting and modifying **WebSocket traffic** (used in real-time web applications like chat apps), in addition to standard HTTP/HTTPS traffic.

### g) BApp Store (Extensions Marketplace)

- Accessible via the **Extensions** tab, the **BApp Store** is Burp's built-in marketplace of community-built extensions (many free), such as:
  - **Logger++** (enhanced logging beyond the default Logger)
  - **Autorize** (automated authorization testing)
  - **Turbo Intruder** (high-speed custom Intruder scripting)
  - **JSON Web Tokens** (JWT analysis/manipulation)

### h) Project Options & Saving State

- Burp Suite lets you **save your entire project state** (all captured requests, scope, configuration) to a file, so you can pause and resume a penetration testing engagement across multiple sessions without losing your work.

### i) Filtering the Site Map

- The **Site Map** view includes powerful **filter options** (e.g., filter by file type, by status code, show only in-scope items, hide already-tested items) to help manage large, complex target applications more efficiently.

> 📌 These topics are commonly covered in more advanced Burp Suite courses/certifications (e.g., Burp Suite Certified Practitioner) and would make excellent subjects for future dedicated videos in this series if you'd like to go deeper beyond the basics already covered.

---

_End of Notes — Burp Suite Series: Introduction, Editions & Features Overview_
