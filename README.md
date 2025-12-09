# Website-Username-Dump-Data-Bridge
WordPress Authentication &amp; Enumeration Analysis This project documents an authorized university (https://goedu.ac/) cybersecurity assessment of a WordPress-based application. Testing included username enumeration analysis, password-wordlist testing using WPScan, and evaluation of authentication weaknesses.

*Testing Scope

*The primary focus areas included:

Username enumeration behavior

Authentication testing using WPScan in a safe, rate-limited environment

Analysis of how WordPress exposes usernames by default

Defensive measures to stop automated username disclosure

* Tools Used
WPScan (Password Wordlist Testing + Enumeration)

WPScan was used to evaluate authentication handling.
During testing, WPScan automatically enumerated usernames through:

WordPress author archive redirects

REST API user endpoint

Public post metadata

This behavior is normal for WordPress and highlights a common hardening issue.

Additional Tools

Burp Suite (for enumeration and HTTP behavior analysis)

Standard Linux CLI utilities

WordPress administrative dashboard (for applying defensive fixes)

* Key Findings
1. Username Enumeration

* WordPress exposed valid usernames through:

/wp-json/wp/v2/users

/author/<ID> pages

Metadata in posts and comments

This allowed WPScan to automatically discover valid usernames even when performing password-only wordlist testing.

2. Authentication Weaknesses

No rate-limiting on login attempts

Clear distinction between “invalid username” vs. “incorrect password”

No multi-factor authentication (MFA)

Predictable username patterns

These issues significantly reduce the time required to perform password-guessing attacks in a controlled environment.

* Defensive Hardening Measures Implemented

* To prevent WPScan and similar tools from automatically dumping usernames, the following protections were added:

1. Disable REST API User Endpoint

Removed /wp-json/wp/v2/users to stop API-based enumeration.

2. Block Author Archive Enumeration

Stopped username leakage through /author/1/ and similar endpoints.

3. Harden Login Responses

Standardized error messages to avoid revealing whether the username or password was incorrect.

4. Change Display Names

Set author display names to values that do not match login usernames.

5. Optional: .htaccess Filters

Blocked ?author= queries to further prevent enumeration.

These measures were tested to confirm that WPScan could no longer retrieve usernames.
