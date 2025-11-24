# Troubleshooting: Resolving Collection-Level Authorization Errors in Postman

**Author:** Q<br>
**Date:** November 2025<br>
**Tools:** Postman v11, REST API<br>
**Difficulty:** Beginner

## Overview
During the validation of the **Postman Library API v2** collection, the automated test suite failed with an assertion error regarding API Key authentication. This guide documents the debugging process, the root cause analysis, and the resolution using Collection-Level Authorization.

## 1. The Error
Upon running the "Final Check" request in the test suite, 42 out of 44 tests passed. The specific failure occurred during the authorization validation step.

**Error Message:**

```text
[COLLECTION] Collection level Auth is set to API Key | AssertionError:
Fix: Set collection level auth api-key: expected false to equal true
```

![Screenshot of the Assertion Error in Postman](postman_error_halfway_check.png)

![Screenshot of the Assertion Error in Postman](postman_error_final_check.png)

## 2. The Root Cause

I inspected the request headers and the collection configuration to isolate the issue.

1. **Scope Mismatch:** The automated test script checks for credentials at the **Collection Level** (Parent). However, the API Key was configured at the **Request Level** (Child).
2. **Naming Convention:** The Key Name was incorrectly labeled as `student-expert`. The API specification explicitly requires the header name `api-key`.

## 3. The Fix

To resolve the `AssertionError`, I refactored the authorization to use **Parent Inheritance**. This ensures that any new request added to the collection automatically uses the correct credentials without manual input.

**Steps Taken:**

1. Select the **Postman Library API v2** folder in the sidebar.
2. Navigate to the **Authorization** tab.
3. Set the **Type** to `API Key`.
4. Configure the credentials:
   - **Key:** `api-key`
   - **Value:** `{{secret_key}}`
5. **Saved** the changes to the collection.

![Screenshot of the correct Collection Authorization settings](postman_auth_settings.png)

## 4. Verification

After applying the fix, I re-ran the "Final Check" request to verify the solution. The test suite returned a 200 OK status with 44/44 tests passed.

![Screenshot of the succeeded Final Check](postman_succeeded_final_check.png)

## Postman API Fundamentals Student Expert Badge

<blockquote
  class="badgr-badge"
  style="font-family: Helvetica, Roboto, 'Segoe UI', Calibri, sans-serif"
>
  <a
    href="https://badges.parchment.com/public/assertions/IZi0yw7eQ0GeDnybEIe8HA"
    ><img
      width="120px"
      height="120px"
      src="https://badges.parchment.com/public/assertions/IZi0yw7eQ0GeDnybEIe8HA/image"
  /></a>
  <p
    class="badgr-badge-name"
    style="
      hyphens: auto;
      overflow-wrap: break-word;
      word-wrap: break-word;
      margin: 0;
      font-size: 16px;
      font-weight: 600;
      font-style: normal;
      font-stretch: normal;
      line-height: 1.25;
      letter-spacing: normal;
      text-align: left;
      color: #05012c;
    "
  >
    Postman API Fundamentals Student Expert
  </p>
  <p
    class="badgr-badge-date"
    style="
      margin: 0;
      font-size: 12px;
      font-style: normal;
      font-stretch: normal;
      line-height: 1.67;
      letter-spacing: normal;
      text-align: left;
      color: #555555;
    "
  >
    <strong
      style="
        font-size: 12px;
        font-weight: bold;
        font-style: normal;
        font-stretch: normal;
        line-height: 1.67;
        letter-spacing: normal;
        text-align: left;
        color: #000;
      "
      >Awarded: </strong
    >November 2025
  </p>
  <p style="margin: 16px 0; padding: 0">
    <a
      class="badgr-badge-verify"
      target="_blank"
      href="https://badgecheck.io?url=https%3A%2F%2Fbadges.parchment.com%2Fpublic%2Fassertions%2FIZi0yw7eQ0GeDnybEIe8HA"
      style="
        box-sizing: content-box;
        display: flex;
        align-items: center;
        justify-content: center;
        margin: 0;
        font-size: 14px;
        font-weight: bold;
        width: 48px;
        height: 16px;
        border-radius: 4px;
        border: solid 1px black;
        text-decoration: none;
        padding: 6px 16px;
        margin: 16px 0;
        color: black;
      "
      aria-label="Verify (opens in new window)"
      >VERIFY</a
    >
  </p>
</blockquote>
