# GitHub Issues — FR-04 Personal Profile Management

## Bug 1: BUG-FR04-N-01 — Name whitespace is not normalized as expected

**Related TC ID:** FR04-N-TC03
**Severity:** Medium

**Steps to reproduce:**
1. Log in with a valid account.
2. Send `PUT /api/users/me` with `name = "  Nguyen Van An  "` (leading and trailing spaces).
3. Call `GET /api/users/me` to verify the stored name.

**Expected result:** Name is stored/displayed as trimmed value `Nguyen Van An`.

**Actual result:** Name was accepted but the stored/displayed value still contains leading/trailing spaces or was not trimmed as expected.

**Evidence:**
_([Screenshot to be added])_

---

## Bug 2: BUG-FR04-N-02 — Name required validation is missing or inconsistent in API

**Related TC IDs:** FR04-N-TC04, FR04-N-TC05, FR04-N-BVA-TC01
**Severity:** High

**Steps to reproduce:**
1. Log in with a valid account.
2. Send `PUT /api/users/me` with `name = ""` (empty string).
3. Send `PUT /api/users/me` with `name = "     "` (spaces only).
4. Observe the API response and call `GET /api/users/me` to check if name changed.

**Expected result:** API rejects the request with a validation error; old name remains unchanged.

**Actual result:** Although frontend shows validation error, testing via Postman/API shows the profile is updated with empty/spaces-only value.

**Evidence:**
_([Screenshot to be added])_

---

## Bug 3: BUG-FR04-N-03 — Name accepts invalid characters

**Related TC IDs:** FR04-N-TC06, FR04-N-TC07, FR04-N-TC08
**Severity:** Medium

**Steps to reproduce:**
1. Log in with a valid account.
2. Send `PUT /api/users/me` with `name = "Nguyen Van An123"` (contains digits).
3. Send `PUT /api/users/me` with `name = "Nguyen@Van#An"` (contains symbols).
4. Send `PUT /api/users/me` with `name = "Nguyen Van An😀"` (contains emoji).
5. Call `GET /api/users/me` to verify the stored name.

**Expected result:** All requests are rejected with validation errors; old name remains unchanged.

**Actual result:** All values were accepted and stored/displayed in the profile.

**Evidence:**
_([Screenshot to be added])_

---

## Bug 4: BUG-FR04-N-04 — Name accepts script-like payload

**Related TC ID:** FR04-N-TC09
**Severity:** High

**Steps to reproduce:**
1. Log in with a valid account.
2. Send `PUT /api/users/me` with `name = "<script>alert(1)</script>"`.
3. Call `GET /api/users/me` to verify the stored name.

**Expected result:** Request is rejected or the input is safely escaped; no script content is stored or executed.

**Actual result:** The script payload was accepted and stored/displayed as the name (though script was not executed).

**Evidence:**
_([Screenshot to be added])_

---

## Bug 5: BUG-FR04-N-05 — API accepts missing or non-string name

**Related TC IDs:** FR04-N-TC10, FR04-N-TC11
**Severity:** High

**Steps to reproduce:**
1. Log in with a valid account.
2. Send `PUT /api/users/me` without the `name` property in the request body.
3. Send `PUT /api/users/me` with `name = 12345` (numeric value).
4. Call `GET /api/users/me` to check profile status.

**Expected result:** API rejects both requests; old name remains unchanged.

**Actual result:** Both requests were accepted and the profile was updated successfully.

**Evidence:**
_([Screenshot to be added])_

---

## Bug 6: BUG-FR04-N-06 — Name maximum length is not enforced

**Related TC ID:** FR04-N-BVA-TC07
**Severity:** Medium

**Steps to reproduce:**
1. Log in with a valid account.
2. Send `PUT /api/users/me` with a 51-character name: `aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa`.
3. Call `GET /api/users/me` to verify the stored name.

**Expected result:** Request is rejected with a validation error for exceeding maximum length (50 characters); old name remains unchanged.

**Actual result:** The 51-character name was accepted and stored/displayed.

**Evidence:**
_([Screenshot to be added])_

---

## Bug 7: BUG-FR04-P-01 — Valid Vietnamese phone numbers are rejected

**Related TC IDs:** FR04-P-TC01, FR04-P-TC02, FR04-P-TC03, FR04-P-BVA-TC02, FR04-P-BVA-TC03, FR04-P-BVA-TC04, FR04-P-BVA-TC05, FR04-P-BVA-TC06
**Severity:** High

**Steps to reproduce:**
1. Log in with a valid account.
2. Send `PUT /api/users/me` with `phone = "0901234567"` (valid 10-digit local).
3. Send `PUT /api/users/me` with `phone = "01234567890"` (valid 11-digit local).
4. Send `PUT /api/users/me` with `phone = "+84901234567"` (valid international format).
5. Observe the API response.

**Expected result:** All valid Vietnamese phone numbers are accepted; profile stores/displays the phone number.

**Actual result:** All requests were rejected with `Invalid phone number format`.

**Evidence:**
_([Screenshot to be added])_

---

## Bug 8: BUG-FR04-P-02 — API accepts missing or non-string phone

**Related TC IDs:** FR04-P-TC06, FR04-P-TC12
**Severity:** High

**Steps to reproduce:**
1. Log in with a valid account.
2. Send `PUT /api/users/me` without the `phone` property in the request body.
3. Send `PUT /api/users/me` with `phone = 9012345678` (numeric value).
4. Call `GET /api/users/me` to check profile status.

**Expected result:** API rejects both requests; old phone remains unchanged.

**Actual result:** Both requests were accepted and the profile was updated successfully.

**Evidence:**
_([Screenshot to be added])_

---

## Bug 9: BUG-FR04-P-03 — Phone accepts invalid prefix or below-minimum value

**Related TC IDs:** FR04-P-TC09, FR04-P-BVA-TC01
**Severity:** High

**Steps to reproduce:**
1. Log in with a valid account.
2. Send `PUT /api/users/me` with `phone = "1901234567"` (invalid prefix, starts with `1`).
3. Send `PUT /api/users/me` with `phone = "911398029"` (9 digits, below minimum length).
4. Call `GET /api/users/me` to verify the stored phone.

**Expected result:** Both requests are rejected with validation errors; old phone remains unchanged.

**Actual result:** Both values were accepted and the profile was updated successfully.

**Evidence:**
_([Screenshot to be added])_

---

## Bug 10: BUG-FR04-P-04 — Phone validation error message is too generic

**Related TC IDs:** FR04-P-TC04, FR04-P-TC05, FR04-P-TC07, FR04-P-TC08, FR04-P-TC10, FR04-P-TC11, FR04-P-BVA-TC07
**Severity:** Low

**Steps to reproduce:**
1. Log in with a valid account.
2. Send `PUT /api/users/me` with various invalid phone values (empty, spaces, letters, separators, too short, too long).
3. Observe the error message returned by the API.

**Expected result:** Error message should be specific to the validation failure (e.g., "Phone is required", "Phone must be 10-11 digits").

**Actual result:** All invalid inputs return the same generic error: `Invalid phone number format`.

**Evidence:**
_([Screenshot to be added])_

---

## Bug 11: BUG-FR04-A-01 — Address whitespace is not trimmed as expected

**Related TC ID:** FR04-A-TC03
**Severity:** Medium

**Steps to reproduce:**
1. Log in with a valid account.
2. Send `PUT /api/users/me` with `address = "  15 Le Loi, District 1  "` (leading and trailing spaces).
3. Call `GET /api/users/me` to verify the stored address.

**Expected result:** Address is stored/displayed as trimmed value `15 Le Loi, District 1`.

**Actual result:** Address was accepted but the stored/displayed value still contains leading/trailing spaces (was not trimmed).

**Evidence:**
_([Screenshot to be added])_

---

## Bug 12: BUG-FR04-A-02 — Address required and minimum length validation are not enforced

**Related TC IDs:** FR04-A-TC04, FR04-A-TC05, FR04-A-TC06, FR04-A-BVA-TC01
**Severity:** High

**Steps to reproduce:**
1. Log in with a valid account.
2. Send `PUT /api/users/me` with `address = ""` (empty).
3. Send `PUT /api/users/me` with `address = "     "` (spaces only).
4. Send `PUT /api/users/me` with `address = "1234"` (4 characters, below minimum 5).
5. Send `PUT /api/users/me` with `address = "Addr"` (4 characters).
6. Call `GET /api/users/me` to verify.

**Expected result:** All requests are rejected with validation errors; old address remains unchanged.

**Actual result:** All values were accepted and stored/displayed.

**Evidence:**
_([Screenshot to be added])_

---

## Bug 13: BUG-FR04-A-03 — Address accepts script or HTML-like payload

**Related TC IDs:** FR04-A-TC07, FR04-A-TC08
**Severity:** High

**Steps to reproduce:**
1. Log in with a valid account.
2. Send `PUT /api/users/me` with `address = "<script>alert(1)</script>"`.
3. Send `PUT /api/users/me` with `address = "<b>Home</b>"`.
4. Call `GET /api/users/me` to verify the stored address.

**Expected result:** Requests are rejected or input is safely escaped; no markup content is stored.

**Actual result:** Both values were accepted and stored/displayed.

**Evidence:**
_([Screenshot to be added])_

---

## Bug 14: BUG-FR04-A-04 — Address accepts emoji and unsupported symbols

**Related TC IDs:** FR04-A-TC09, FR04-A-TC10
**Severity:** Medium

**Steps to reproduce:**
1. Log in with a valid account.
2. Send `PUT /api/users/me` with `address = "12 Le Loi 😀"` (contains emoji).
3. Send `PUT /api/users/me` with `address = "12 Le Loi @ District #1"` (contains `@` and `#`).
4. Call `GET /api/users/me` to verify.

**Expected result:** Both requests are rejected with validation errors; old address remains unchanged.

**Actual result:** Both values were accepted and stored/displayed.

**Evidence:**
_([Screenshot to be added])_

---

## Bug 15: BUG-FR04-A-05 — API accepts missing or non-string address

**Related TC IDs:** FR04-A-TC11, FR04-A-TC12
**Severity:** High

**Steps to reproduce:**
1. Log in with a valid account.
2. Send `PUT /api/users/me` without the `address` property in the request body.
3. Send `PUT /api/users/me` with `address = 12345` (numeric value).
4. Call `GET /api/users/me` to check profile status.

**Expected result:** API rejects both requests; old address remains unchanged.

**Actual result:** Both requests were accepted and the profile was updated successfully.

**Evidence:**
_([Screenshot to be added])_

---

## Bug 16: BUG-FR04-A-06 — Address maximum length is not enforced

**Related TC ID:** FR04-A-BVA-TC07
**Severity:** Medium

**Steps to reproduce:**
1. Log in with a valid account.
2. Send `PUT /api/users/me` with a 256-character address string.
3. Call `GET /api/users/me` to verify the stored address.

**Expected result:** Request is rejected with a validation error for exceeding maximum length (255 characters); old address remains unchanged.

**Actual result:** The 256-character address was accepted and stored/displayed.

**Evidence:**
_([Screenshot to be added])_
