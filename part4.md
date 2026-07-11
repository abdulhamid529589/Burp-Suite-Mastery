# Burp Suite Series — The Repeater Module

> **Course:** Cyber Security — Web Application Penetration Testing
> **Topic:** Understanding and Using Burp Suite's Repeater Tool

---

## Table of Contents

1. [Introduction](#1-introduction)
2. [Recap: Setting Up the Proxy](#2-recap-setting-up-the-proxy)
   - 2.1 [Using FoxyProxy for Quick Proxy Switching](#21-using-foxyproxy-for-quick-proxy-switching)
3. [What is the Repeater Module?](#3-what-is-the-repeater-module)
4. [Why Do We Need Repeater?](#4-why-do-we-need-repeater)
5. [Understanding HTTP Requests: GET vs POST](#5-understanding-http-requests-get-vs-post)
6. [Sending a Request to Repeater — Step by Step](#6-sending-a-request-to-repeater--step-by-step)
7. [The Repeater Interface](#7-the-repeater-interface)
   - 7.1 [Request Panel](#71-request-panel)
   - 7.2 [Response Panel](#72-response-panel)
   - 7.3 [Response View Modes](#73-response-view-modes)
8. [HTTP Status Codes Overview](#8-http-status-codes-overview)
9. [Practical Use Case 1: Testing Login Credentials](#9-practical-use-case-1-testing-login-credentials)
10. [Practical Use Case 2: Manual SQL Injection Testing](#10-practical-use-case-2-manual-sql-injection-testing)
11. [Why Repeater is Better Than Manually Retesting on the Website](#11-why-repeater-is-better-than-manually-retesting-on-the-website)
12. [What's Coming Next: The Intruder Module](#12-whats-coming-next-the-intruder-module)
13. [Summary / Quick Revision Table](#13-summary--quick-revision-table)

---

## 1. Introduction

This installment of the Burp Suite series focuses on the **Repeater** module — one of the most frequently used tools during **web application penetration testing**.

Previous videos in this series already covered:

- What Burp Suite is
- How to use Burp Suite generally
- How to install Burp's SSL certificate
- How to set up a virtual lab environment in the cloud

This video builds on that foundation and dives specifically into the **Repeater** tab: what it is, why it's used, and how to use it practically.

---

## 2. Recap: Setting Up the Proxy

Before using Repeater, the **Burp Proxy** must be correctly configured to intercept browser traffic.

**Steps to launch Burp Suite:**

1. Open a terminal.
2. Navigate to Burp Suite's install directory / launch command.
3. Run **`burpsuite`** and press Enter.
4. Skip any update prompts (click **Close**) if you don't want to update immediately.
5. Click **Next**, then **Start** to reach the Burp Suite **Dashboard**.

Once inside Burp Suite, the browser being used (e.g., **Mozilla Firefox**, or Burp's own **built-in browser**) needs to have its traffic routed through Burp's proxy.

### 2.1 Using FoxyProxy for Quick Proxy Switching

Instead of manually changing browser proxy settings every time (via Settings → search "proxy" → manually enter Burp's proxy details), you can use the **FoxyProxy** browser extension to toggle the proxy on/off instantly.

**Setting up FoxyProxy (first-time setup):**

1. Click on the **FoxyProxy extension** icon in the browser toolbar.
2. Go to **Options**.
3. Click **Edit** on the proxy profile.
4. Enter the details of the system through which you want all your website traffic to be routed — i.e., the system where you'll be **intercepting**, **modifying**, and **analyzing** your queries (this is typically `127.0.0.1` with Burp's proxy port, e.g., `8080`).
5. Save/go back.
6. You can now simply click the extension to **turn the proxy ON or OFF** whenever needed, without repeating the manual setup each time.

> 📌 For full step-by-step proxy configuration details, refer back to the first video in this Burp Suite series.

---

## 3. What is the Repeater Module?

**Repeater** is a Burp Suite module that lets you **capture a single HTTP request and then resend ("replay") it repeatedly**, making manual modifications to it each time, **without having to go back to the actual website in your browser** for every test.

In short:

> Repeater = capture once, then repeatedly edit and resend the same request directly from within Burp Suite, observing the server's response each time.

---

## 4. Why Do We Need Repeater?

During a typical proxy interception workflow (as covered in earlier videos):

1. You turn on **Intercept** in the Burp Proxy tab.
2. You perform an action on the website (e.g., click a button, submit a form).
3. The request gets **captured/intercepted** before it reaches the server.
4. You can inspect and even modify the request.
5. You click **Forward** to send it on to the server.

**The problem:** If you want to test **multiple variations** of a request (e.g., trying different parameter values one at a time), doing this through the Proxy/Intercept tab becomes extremely **tedious and repetitive** — you'd have to:

- Go back to the website in the browser every time
- Re-trigger the same action
- Re-intercept the new request
- Modify it again
- Forward it again
- Check the result on the website

This becomes highly **hectic** for repeated testing.

**The solution — Repeater:**

- Send the captured request to Repeater **once**.
- From then on, you can edit the request directly inside Repeater and hit **Send** as many times as you want.
- You immediately see the server's **response** right there, without ever needing to reload or revisit the actual website.

---

## 5. Understanding HTTP Requests: GET vs POST

While intercepting traffic, you'll notice different types of HTTP requests. The two most common are:

| Request Type | Description                                                                                            |
| ------------ | ------------------------------------------------------------------------------------------------------ |
| **GET**      | Used to retrieve/request data (e.g., loading a profile page) — parameters are often visible in the URL |
| **POST**     | Used to submit data to the server (e.g., login forms, search submissions)                              |

> Other HTTP methods exist too, but **GET** and **POST** are by far the most commonly encountered during web pentesting.

Additional info visible in a captured request includes: **User Info**, **Host**, and other headers — all viewable directly within the intercepted request.

---

## 6. Sending a Request to Repeater — Step by Step

1. Go to the **Proxy** tab and make sure **Intercept is ON**.
2. Perform the action on the target website whose request you want to capture (e.g., navigate to a login page, click **Login**).
3. The request gets intercepted and appears in the **Proxy → Intercept** tab.
4. **Right-click** on the intercepted request.
5. Select **"Send to Repeater"**.
6. Switch to the **Repeater** tab — your captured request is now loaded there, ready for editing and repeated testing.

> 💡 Tip: You can turn **Intercept OFF** in the Proxy tab after sending the request to Repeater — you no longer need live interception since you already have the request saved inside Repeater.

---

## 7. The Repeater Interface

Once inside Repeater, the interface is split into two main panels:

### 7.1 Request Panel

- This is the data being sent **from the client (your system) to the server**.
- You can directly **edit any part** of this request — headers, parameters, form field values, cookies, etc.
- After editing, click **Send** to fire the modified request off to the server.

### 7.2 Response Panel

- This shows exactly what the **server sends back** in reply to your request.
- Updates every time you click **Send**, without needing to reload any browser page.

### 7.3 Response View Modes

Burp's Repeater response panel offers multiple ways to view the same response data:

| View Mode  | Description                                                                                                        |
| ---------- | ------------------------------------------------------------------------------------------------------------------ |
| **Render** | Shows how the response would actually **look/render** as a webpage — i.e., what the browser would visually display |
| **Raw**    | Shows the **raw, unformatted** HTTP response exactly as received                                                   |
| **Pretty** | A cleaned-up, nicely formatted version of the raw response (same content as Raw, easier to read)                   |
| **Hex**    | Shows the response data in **hexadecimal** format, useful for detailed byte-level analysis                         |

---

## 8. HTTP Status Codes Overview

While testing, the HTTP response status code tells you the outcome of your request at a glance.

| Status Code Range | General Meaning                                |
| ----------------- | ---------------------------------------------- |
| **2xx**           | Success (e.g., 200 OK)                         |
| **3xx**           | Redirection (e.g., 302 Found/Redirect)         |
| **4xx**           | Client error (e.g., 404 Not Found)             |
| **5xx**           | Server error (e.g., 500 Internal Server Error) |

**Example from practice:** Submitting an incorrect username/password to a login form returned an **HTTP 302** status — a redirection response — indicating the login attempt was rejected and the user was redirected (e.g., back to the login page).

---

## 9. Practical Use Case 1: Testing Login Credentials

**Scenario:** Testing whether a guessed username/password combination works, without repeatedly reloading the actual login page in the browser.

**Steps demonstrated:**

1. Intercept the login request while submitting an intentionally **incorrect** username and password.
2. Send that intercepted request to **Repeater**.
3. Click **Send** in Repeater — observe the response.
   - Result: **HTTP 302** (redirect) — indicates the incorrect credentials were rejected.
4. Now, **edit the request directly inside Repeater** — change the password field to a guessed value (e.g., `test`), matching a guessed username (e.g., `test`).
5. Click **Send** again.
6. Check the **Render** tab of the response — if the response shows a successfully logged-in page, the guessed credentials were correct.
   - In the demonstrated example, using `test` / `test` resulted in a **successful login**, confirmed by viewing the rendered response directly inside Repeater — with **no need to go back to the actual website** at all.

This illustrates the core value of Repeater: **rapid, iterative testing of different input values against the same request, entirely within Burp Suite.**

---

## 10. Practical Use Case 2: Manual SQL Injection Testing

Repeater isn't limited to login forms — it can be used to test **any parameter** that accepts user input (login fields, search boxes, form fields, etc.) for vulnerabilities such as **SQL Injection**.

**Demonstrated approach:**

1. First, submit a normal-looking but incorrect username/password (e.g., username: `Rahul`, password: `Kumar`) to establish a baseline failed response.
2. Send this request to Repeater.
3. In the **password field** of the request, insert a classic SQL injection payload — the well-known pattern:
   ```
   ' OR 1=1 --
   ```
   (Referred to in the video as the famous `admin' = 1=1` style query — a very common, basic SQL injection test string used to check if a login form is vulnerable to authentication bypass.)
4. Copy this injected query and paste it into the relevant field (e.g., password parameter) within the Repeater request.
5. Click **Send**.
6. Check the response:
   - If the SQL injection payload is successfully interpreted by the vulnerable backend query, the server may respond with a **successful login** — bypassing authentication entirely, **without a valid username or password**.
7. Click on the **Render** view to confirm — if you see a logged-in dashboard/page, the SQL injection was successful and the field is vulnerable.

**Key takeaway:** Any input parameter that gets passed into a backend query — not just login fields, but also **search boxes** or other free-text inputs sent to the server — can potentially be tested this way for SQL injection vulnerabilities using Repeater.

> ⚠️ **Reminder:** This kind of testing should only ever be performed on systems you own or are explicitly authorized to test (e.g., your own deployed vulnerable lab application) — never on live/production systems without permission.

---

## 11. Why Repeater is Better Than Manually Retesting on the Website

| Without Repeater                                                       | With Repeater                                                              |
| ---------------------------------------------------------------------- | -------------------------------------------------------------------------- |
| Must revisit the website in the browser for every test                 | Everything happens inside Burp Suite                                       |
| Must re-trigger the action (e.g., re-click "Login") each time          | Just click **Send** to resend the modified request                         |
| Must manually check the resulting webpage each time                    | Response (including a rendered preview) shows instantly in Repeater        |
| Very repetitive and time-consuming ("hectic") for multiple test values | Fast, iterative testing of many different payloads/parameters in one place |

---

## 12. What's Coming Next: The Intruder Module

The **next module** to be covered in this series is **Intruder** — described as a very interesting and powerful Burp Suite module used specifically to perform **brute-force attacks** on websites (e.g., brute-forcing login passwords). This is the tool behind what's commonly referred to when people talk about "brute-forcing" a password on a web application.

---

## 13. Summary / Quick Revision Table

| Concept                      | One-Line Summary                                                                                                                   |
| ---------------------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| **Repeater**                 | A Burp Suite module for capturing a request once and resending/editing it repeatedly, without needing to reload the actual website |
| **FoxyProxy**                | Browser extension used to quickly toggle Burp's proxy on/off without manual settings changes each time                             |
| **Send to Repeater**         | Right-click an intercepted request in the Proxy tab → "Send to Repeater"                                                           |
| **Request panel**            | Shows the outgoing data from the client — fully editable before resending                                                          |
| **Response panel**           | Shows the server's reply, updated instantly on every "Send"                                                                        |
| **Render view**              | Shows how the response would visually appear as a rendered webpage                                                                 |
| **Raw / Pretty / Hex views** | Alternate ways to inspect the raw response data                                                                                    |
| **GET vs POST**              | The two most common HTTP request methods encountered during testing                                                                |
| **HTTP status codes**        | 2xx = success, 3xx = redirect, 4xx = client error, 5xx = server error                                                              |
| **Login testing use case**   | Edit username/password fields directly in Repeater and resend to test credential guesses                                           |
| **SQL Injection use case**   | Insert payloads like `' OR 1=1 --` into input fields via Repeater to test for authentication bypass vulnerabilities                |
| **Key advantage**            | Eliminates the need to repeatedly revisit the website in-browser for each test — everything is done from within Burp Suite         |
| **Next topic**               | **Intruder** module — used for automated brute-force attacks on web applications                                                   |

---

_End of Notes — Burp Suite Series: Repeater Module_
