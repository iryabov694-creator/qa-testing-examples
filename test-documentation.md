# Test Documentation Examples 

This repository contains samples of functional test checklists and verification suites designed for mobile (Android/iOS) and web applications. 

###  Full Interactive Checklist
The complete regression test suite consisting of **100+ verification points** (including Onboarding, Profile/Auth, AI Career Assistant, AI Interview Simulation, and Resume Builder modules) is fully maintained and accessible via Google Sheets:

 **[View Full Checklist in Google Sheets](https://docs.google.com/spreadsheets/d/17vvnIcuvp_c5qleqaId8fO2jCNT4baNH-l03y4QZeDw/edit?usp=sharing)**

---

##  Checklist Structure & Preview

Below is a brief structure preview of the tested components:

* **A. Splash Screen & Onboarding:** Animation timings, legal consents, sequentially managed step steps, skipping behaviors, and first-launch persistence.
* **B. Home Screen & Tab Navigation:** Deep links handling, visual placement, active icon states, and system localization syncing.
* **C. Profile & Authorization:** Google/Apple sign-in dialog flow integration, validation failure errors handling, secure logout cache wiping, and account deletion workflows.
* **D. AI Career Assistant & Chatbot:** Verification of LLM prompt constraints, boundary input resizing, right/left alignment grid bubbles, historical sessions clearing, and conversational contextual maintenance.
* **E. AI Interview Simulation:** Real-time speech streaming, dynamic mic level permissions toggles, audio recording animations, response generation fallbacks, and video-avatar speech synchronization.
* **F. Resume & CV Builder:** Multi-step draft percentage progression tracking, paid features dialog triggers, multi-select tag chips validation, and layout pagination inside PDF exports.
---

## 
Paywall & Subscription State Machine Matrix

This section demonstrates my ability to test complex, multi-tiered authorization and billing systems based on server-side flags (`is_client`, `is_partner`, `is_paid`, `partner_program_version`).

### 1. User Profiles & Dynamic UI Routing Matrix

| # | User Type Flags | Expected UI Behavior (Front-End Presentation) | Test Account (Mock Data) |
| :-: | :--- | :--- | :--- |
| **1** | `0, 0, 0, null` | **Blocking Paywall #1 (Account Type Selection):** Disallows any app usage. Forces user to pick either "Client" or "Partner". | `noclient@test.com` |
| **2** | `1, 0, 0, v2` | **Blocking Paywall #2 (Subscription):** Displays 3 pricing tiers ($30 / $59 / $99) tailored for the "Client" path. | `clientonly@test.com` |
| **3** | `1, 0, 1, v2` | **Full Access + Upgrade CTA:** Main app unlocked. Settings/Profile shows an "Upgrade" button leading to Partner Paywall #1. | `clientpaid@test.com` |
| **4** | `1, 0, 0, v1` | **Blocking Paywall #2 (Subscription):** Legacy client view. Displays 3 standard pricing tiers ($30 / $59 / $99). | `october44@test.com` |
| **5** | `1, 0, 1, v1` | **Full Access + Legacy Upgrade CTA:** Main app unlocked. "Upgrade" CTA offers full Partner feature transition. | `december02@test.com` |
| **6** | `0, 1, 0, v1` | **Blocking Paywall #2 (Subscription):** Partner path locked. Displays 2 dedicated pricing tiers ($59 / $99). | `october37@test.com` |
| **7** | `0, 1, 1, v1` | **Full Access + Cross-Sell CTA:** App unlocked. Offers "Upgrade" path to expand Partner tier boundaries. | `prodtest@test.com` |
| **8** | `0, 1, 0, v2` | **Blocking Paywall #2 (Subscription):** New Partner path locked. Displays 2 updated pricing tiers ($59 / $99). | `partneronly@test.com` |
| **9** | `0, 1, 1, v2` | **Unrestricted Maximum Tier Access:** Full app unlocked. "Upgrade" action items/buttons are hidden completely. | `unilevel01@test.com` |
| **10**| `0, 1, 1, v1` *(Discount)* | **Full Access + Paywall #3 (Discounted Partner Upgrade):** Specific condition triggered for users with the `account_partner_discount_v2` product flag active. | `discount01@test.com` |

---

### 2. End-to-End User Flow Scenarios

#### Scenario A: Seamless Onboarding for Fresh Registrations (`0,0,0` ➔ `1,0,0` ➔ `1,0,1`)
1. **Initial State:** New user lands on **Paywall #1** (Account Type Selector).
2. **Action:** User purchases the "Client" tier.
3. **State Mutation:** Database flags dynamically update to `is_client = true` (`1,0,0`).
4. **Expected Result:** App instantly intercepts the state and triggers **Paywall #2** (Subscription Selection).

#### Scenario B: Intermediate State Fallbacks (`1,0,0` or `0,1,0`)
* **Condition:** User closes the app or possesses a verified role but has no active paid balance.
* **Expected Result:** Upon app launch, the system must immediately serve **Paywall #2** (Subscription Selector). Deep links or standard app navigation routes must remain locked until validation passes.

#### Scenario C: Upgrading Account Roles (`1,0,1` ➔ `0,1,1`)
1. **Initial State:** Active paid Client (`is_client=true`, `is_paid=true`).
2. **Action:** User triggers the "Upgrade" CTA from the account dashboard and pays the difference.
3. **State Mutation:** `is_partner` updates to `true`.
4. **Expected Result:** App keeps the core access token active (`is_paid` remains `true`) but dynamically flips the UI container to display Partner features.

#### Scenario D: Legacy User Data Grace Period Handling (V1 Migration Exception)
* **Condition:** System detects a legacy V1 user returning with an expired product session database log.
* **Backend Mitigation:** Forced baseline rewrite: `is_client = true`, `paid = false`.
* **Expected Result:** User bypasses the Account Type Selector entirely and is redirected straight to **Step 2 (Paywall #2)** to re-verify payment credentials.
