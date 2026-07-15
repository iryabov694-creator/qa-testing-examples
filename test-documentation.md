# Test Documentation Examples 

This repository contains samples of functional test checklists and verification suites designed for mobile (Android/iOS) and web applications. 

### 🟢 Full Interactive Checklist
The complete regression test suite consisting of **100+ verification points** (including Onboarding, Profile/Auth, AI Career Assistant, AI Interview Simulation, and Resume Builder modules) is fully maintained and accessible via Google Sheets:

👉 **[View Full Checklist in Google Sheets (Кликните для просмотра полной таблицы)](https://docs.google.com/spreadsheets/d/17vvnIcuvp_c5qleqaId8fO2jCNT4baNH-l03y4QZeDw/edit?usp=sharing)**

---

## 📋 Checklist Structure & Preview

Below is a brief structure preview of the tested components:

* **A. Splash Screen & Onboarding:** Animation timings, legal consents, sequentially managed step steps, skipping behaviors, and first-launch persistence.
* **B. Home Screen & Tab Navigation:** Deep links handling, visual placement, active icon states, and system localization syncing.
* **C. Profile & Authorization:** Google/Apple sign-in dialog flow integration, validation failure errors handling, secure logout cache wiping, and account deletion workflows.
* **D. AI Career Assistant & Chatbot:** Verification of LLM prompt constraints, boundary input resizing, right/left alignment grid bubbles, historical sessions clearing, and conversational contextual maintenance.
* **E. AI Interview Simulation:** Real-time speech streaming, dynamic mic level permissions toggles, audio recording animations, response generation fallbacks, and video-avatar speech synchronization.
* **F. Resume & CV Builder:** Multi-step draft percentage progression tracking, paid features dialog triggers, multi-select tag chips validation, and layout pagination inside PDF exports.
