# Burp Suite Series — The Intruder Module

> **Course:** Cyber Security — Web Application Penetration Testing
> **Topic:** Understanding and Using Burp Suite's Intruder Tool (Brute-Force Testing)

---

## Table of Contents

1. [Introduction](#1-introduction)
2. [What is the Intruder Module?](#2-what-is-the-intruder-module)
3. [Two Ways to Bypass a Login](#3-two-ways-to-bypass-a-login)
4. [Practice Target: A Vulnerable Banking Login](#4-practice-target-a-vulnerable-banking-login)
5. [Step-by-Step: Sending a Request to Intruder](#5-step-by-step-sending-a-request-to-intruder)
6. [Setting Payload Positions](#6-setting-payload-positions)
7. [The Four Attack Types](#7-the-four-attack-types)
   - 7.1 [Sniper](#71-sniper)
   - 7.2 [Battering Ram](#72-battering-ram)
   - 7.3 [Pitchfork](#73-pitchfork)
   - 7.4 [Cluster Bomb](#74-cluster-bomb)
8. [Configuring Payloads (Wordlists)](#8-configuring-payloads-wordlists)
9. [Running the Attack & Analyzing Results](#9-running-the-attack--analyzing-results)
   - 9.1 [Using Response Length to Spot the Correct Credentials](#91-using-response-length-to-spot-the-correct-credentials)
   - 9.2 [Verifying the Result](#92-verifying-the-result)
10. [Security Best Practice Callout](#10-security-best-practice-callout)
11. [Summary / Quick Revision Table](#11-summary--quick-revision-table)

---

## 1. Introduction

Imagine being given a website and told: _"Guess the correct username and password on the login page to log in."_ Doing this manually by guessing one combination at a time could take a day, a month, or even a year.

This video addresses exactly this problem: **how to test a website's login page (or any input parameter) for security weaknesses by systematically sending different inputs and observing the website's behavior** — automatically, instead of manually guessing one at a time.

This is done using Burp Suite's **Intruder** module.

---

## 2. What is the Intruder Module?

**Intruder** is a Burp Suite module used to **automatically test a website's input fields** with large sets of possible values (a "wordlist"), to find valid or exploitable inputs.

**Typical use cases:**

- Testing a login form to see if a **username/password combination** can be guessed or bypassed.
- Testing input fields for **SQL Injection** or other injection vulnerabilities.
- Any scenario where you need to try **many input values automatically** against a target field, rather than manually one at a time.

> "Bypassing" here means testing the security of a website's login system — checking whether it's possible to log in **without knowing the correct, legitimate username and password** — either by:
>
> 1. Using a large, well-crafted **wordlist** of guessed credentials, or
> 2. Using **injection techniques** (e.g., SQL injection payloads) to manipulate the login logic itself.

---

## 3. Two Ways to Bypass a Login

| Method                         | Description                                                                                                                      |
| ------------------------------ | -------------------------------------------------------------------------------------------------------------------------------- |
| **1. Wordlist-based guessing** | Sending a large list of candidate username/password combinations and checking which one(s) succeed                               |
| **2. Injection-based bypass**  | Sending crafted payloads (e.g., SQL injection strings) that manipulate the backend query logic to bypass authentication entirely |

This video focuses on demonstrating the **Intruder module** as the tool used to automate this kind of testing.

---

## 4. Practice Target: A Vulnerable Banking Login

The demonstration uses a **legally testable, intentionally vulnerable practice website** — a mock online banking login page — to safely illustrate the technique (never test real/production login pages without explicit authorization).

**Setup:**

- Navigate to the practice site's **Sign In** page.
- It presents a standard **username + password** login form.
- Entering random/unknown credentials fails, as expected (since the correct credentials aren't known).
- For demonstration, a plausible-looking guess is entered:
  - **Username:** `admin`
  - **Password:** `password`
- Submitting this shows the login field again (attempt failed) — this is the baseline used to capture the request for Intruder.

---

## 5. Step-by-Step: Sending a Request to Intruder

1. Open **Burp Suite** and go to the **Proxy** tab.
2. Turn **Intercept ON**.
3. Make sure your browser's proxy (e.g., via **FoxyProxy**) is correctly routing traffic through Burp.
4. Go back to the login page and submit the login form again with the same placeholder values (e.g., `admin` / `password`) — this generates a request that Burp will intercept.
5. In the intercepted request, **select the login request** and send it to Intruder:
   - Right-click → **Send to Intruder**
6. Switch to the **Intruder** tab in Burp Suite.

---

## 6. Setting Payload Positions

Once inside Intruder, you need to tell Burp **exactly which parts of the request** should be replaced with test values (these are called "**payload positions**," marked with `§` symbols around highlighted values by default).

**Steps:**

1. Burp automatically highlights all detected parameter values in the request.
2. Since we only want to test the **username** and **password** fields, we first need to **clear all default highlighted positions**:
   - Select all (`Ctrl+A`) → Click **Clear**
3. Now manually select **only** the values you want to test:
   - Highlight the username value (e.g., `admin`) → Click **Add**
   - Highlight the password value (e.g., `password`) → Click **Add**
4. You now have **exactly two payload positions** set — one for username, one for password.

---

## 7. The Four Attack Types

Burp Intruder offers **four attack types**, which determine _how_ the payload values are combined and tested across the defined positions:

| Attack Type       | Number of Payload Sets Used |
| ----------------- | --------------------------- |
| **Sniper**        | 1 wordlist                  |
| **Battering Ram** | 1 wordlist                  |
| **Pitchfork**     | 2 wordlists                 |
| **Cluster Bomb**  | 2 wordlists                 |

### 7.1 Sniper

- Uses **only one wordlist**, applied to **one position at a time**.
- **Mechanism:**
  - Keeps the **first** field (e.g., username = `admin`) fixed.
  - Tries **every value** in the wordlist against the **second** field (password) — one at a time.
  - Once all wordlist values have been tried against the password field, Sniper then **reverses roles**: it keeps the password field fixed (e.g., at its original value) and tries every wordlist value against the username field instead.
- **Best used when:** You already know **one correct value** (e.g., you know the target username) and just want to try many different values against the **other** field (e.g., cracking just the password).
- **Real-world relevance:** Many devices/websites are left with **default usernames** — if you already know the default/target username, you can fix it and brute-force just the password.

### 7.2 Battering Ram

- Also uses only **one wordlist** — but unlike Sniper, it applies the **same value simultaneously to all payload positions** at once.
- **Example:** If the wordlist contains `12345`, Battering Ram will test `12345` as **both** the username **and** the password at the same time, in the same request.
- **Difference from Sniper:** Sniper fixes one field and varies the other; Battering Ram varies **all fields together with the same value** in every attempt.

### 7.3 Pitchfork

- Uses **two separate wordlists**, one per payload position.
- **Mechanism:** The two wordlists are stepped through **in parallel, position-by-position (line-by-line)** — e.g., wordlist #1 entry #1 pairs with wordlist #2 entry #1, wordlist #1 entry #2 pairs with wordlist #2 entry #2, and so on.
- **Best used when:** You have **two separate wordlists** — one for usernames, one for passwords — and want each row in one list matched against the corresponding row in the other (rather than testing every combination).
- **Benefit:** Significantly reduces total time/requests needed compared to testing every possible combination, when you have a _paired_ dataset (e.g., a known list of username+password pairs, tested one pair at a time in matching order).

### 7.4 Cluster Bomb

- The **most commonly used** and most powerful of the four attack types.
- Also uses **two wordlists**, but instead of stepping through them in parallel (like Pitchfork), it tries **every possible combination** between the two lists.
- **Mechanism (example):**
  1. Take the **first value** from wordlist #1 (e.g., `admin`) and lock it in.
  2. Try **every single value** in wordlist #2 against it, one at a time.
  3. Once all of wordlist #2 has been tried against `admin`, move to the **next value** in wordlist #1 (e.g., `admin123`).
  4. Repeat — try every value in wordlist #2 against this new value.
  5. Continue this process for **every value** in wordlist #1, until all combinations have been exhausted or a correct result is found.
- **Best used when:** You don't know either the username or the password precisely, and want to try **every possible combination** of two separate wordlists (usernames × passwords) — this is the classic full brute-force login attack.

---

## 8. Configuring Payloads (Wordlists)

After setting the **attack type**, go to the **Payloads** tab in Intruder:

- Here you'll see **Payload Sets** — e.g., "Payload Set 1," "Payload Set 2" — one for each payload position you defined earlier.
- **Sniper** only shows **one** payload set (since it only uses one wordlist).
- **Cluster Bomb / Pitchfork** show **two** payload sets — one for each position (e.g., Set 1 = usernames, Set 2 = passwords).

**Loading a wordlist:**

- For each payload set, load your prepared **wordlist file** (a `.txt` file containing candidate values, one per line).
- Example demonstrated: a small custom wordlist containing several candidate passwords, prepared specifically for this test.
- **Note:** Wordlist quality matters a lot — a well-crafted, targeted wordlist significantly improves the chances of success. (Refer to the referenced earlier video in this channel's playlist for details on how to build effective wordlists.)

**In this demonstration:**

- Both payload positions (username and password) had their respective wordlists loaded.
- **Attack Type used: Cluster Bomb** (to try every combination between the two lists).

---

## 9. Running the Attack & Analyzing Results

1. Once payload positions and wordlists are configured, click **Start Attack**.
2. Burp Suite will send a request for **every combination** of the two wordlists (per the Cluster Bomb logic) and log each attempt in a results table.
3. This table typically shows columns like: **Request number, Payload values used, HTTP Status Code, Response Length**, etc.

### 9.1 Using Response Length to Spot the Correct Credentials

- While scrolling through the attack results, pay close attention to the **"Length"** column (response length).
- Most failed login attempts will return responses of a **similar, consistent length** (since the server returns the same "login failed" page each time).
- **A request with a noticeably different response length** is a strong signal that something different happened on the server — very possibly indicating a **successful login**.
- In the demonstration, one particular request stood out with a **different length value**, and the payload used for that request was: **username = `admin`, password = `admin`**.

### 9.2 Verifying the Result

1. Go back to the actual login page.
2. Manually enter the discovered credentials: `admin` / `admin`.
3. Click **Login**.
4. Confirm success by checking the response — in this case, the login succeeded, and the response returned an **HTTP 302** status code (a redirect), which — in the context of that particular application's behavior — indicated a valid login had occurred and the credentials were confirmed correct.

---

## 10. Security Best Practice Callout

> ⚠️ **Important lesson from this demo:** Devices and websites should **never** be left with **default usernames or default passwords** (e.g., `admin`/`admin`, `admin`/`password`). Default credentials are one of the most common and easily exploitable weaknesses attackers look for — as this demonstration itself shows.

---

## 11. Summary / Quick Revision Table

| Concept               | One-Line Summary                                                                                                        |
| --------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| **Intruder**          | Burp Suite module for automatically testing input fields with large sets of candidate values (wordlists)                |
| **Use cases**         | Login brute-forcing, credential guessing, injection payload testing on any input parameter                              |
| **Bypass methods**    | (1) Wordlist-based guessing, (2) Injection-based bypass                                                                 |
| **Payload positions** | The specific parts of a request marked for value substitution during the attack (set via Clear → highlight → Add)       |
| **Sniper**            | 1 wordlist; tests one position at a time while keeping the other fixed, then reverses                                   |
| **Battering Ram**     | 1 wordlist; applies the same value to all positions simultaneously                                                      |
| **Pitchfork**         | 2 wordlists; steps through both in parallel (line-by-line pairing)                                                      |
| **Cluster Bomb**      | 2 wordlists; tries **every possible combination** between them (most thorough, most commonly used)                      |
| **Payloads tab**      | Where wordlists are loaded for each payload set/position                                                                |
| **Response Length**   | Key indicator during results analysis — a differing length often signals a different (e.g., successful) server response |
| **Security lesson**   | Never leave devices/applications with default usernames/passwords — they're a common attack target                      |
| **Legal reminder**    | Only perform this kind of testing on systems you own or are explicitly authorized to test                               |

---

_End of Notes — Burp Suite Series: Intruder Module_
