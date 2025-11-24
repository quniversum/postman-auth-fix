# Troubleshooting: Resolving Collection-Level Authorization Errors in Postman

**Author:** Q<br>
**Date:** November 2025<br>
**Tools:** Postman v11, REST API<br>
**Difficulty:** Beginner

## Overview

While validating the **Postman Library API v2** collection, the automated test suite failed with an assertion error regarding API Key authentication. This guide details the root cause—improper scope configuration—and the steps to resolve it using Collection-Level Authorization.

## The Error

**Scenario:**
Upon running the "Final Check" request, 42/44 tests passed. The specific failure occurred during the authorization validation step.

**Error Message:**

```text
[COLLECTION] Collection level Auth is set to API Key | AssertionError:
Fix: Set collection level auth api-key: expected false to equal true
```

![Screenshot of the Assertion Error in Postman](postman_error_Halfway_Check.png)

![Screenshot of the Assertion Error in Postman](postman_error_Final_Check.png)

## The Root Cause

The automated test script checks for credentials at the Collection Level (Parent), but the API Key was configured at the Request Level (Child). Furthermore, the Key Name was incorrectly labeled as `student-expert` instead of the required header name `api-key.`

## The Fix

To resolve the `AssertionError`, the authorization must be moved to the Collection level so all requests inherit it automatically.

1. Select the Postman Library API v2 folder in the sidebar.
2. Navigate to the Authorization tab.
3. Set the Type to `API Key.`
4. Configure the credentials:
   - Key: `api-key`
   - Value: `{{secret_key}}`

![Screenshot of the correct Collection Authorization settings](postman_auth_settings.png)

5. Save the changes and re-run the Final Check request.

![Screenshot of the succeeded Final Check](postman_succeeded_final_check.png)
