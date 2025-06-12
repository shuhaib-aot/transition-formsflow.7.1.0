
![image](https://github.com/user-attachments/assets/c9397b8e-d6ed-4ab6-bb54-505ce7e5a894)

# 📦 Form Bundle Feature

## Overview

The **Form Bundle** is a feature that allows grouping one or more forms into a bundle. It enables:

- Collecting data from multiple forms into a **single bundled submission**.
- Rendering forms **conditionally** based on rules or logic.
- Reordering the **form views** dynamically.

---

## ⚙️ How Form Bundle Works

Each time a form is rendered as part of a bundle, the system calls the `Execute Rule API`. This API evaluates the logic and determines which form should be shown next.

- The **bundled submission data** is sent to the `Execute Rule API` for rule evaluation.
- This ensures dynamic and condition-based rendering of forms within the bundle.

---

## 🛠️ Bundled Submission in Formio

Formio supports bundled submissions with a few custom headers:

- To allow bundled submission data to be stored, you must include the following header:

```http
skip-sanitize: true
```
This bypasses the default sanitization check during submission and allows storing the full structure of the bundle.

🔐 How Bundle Submission Works
Bundled submissions are secured — the bundleId is not directly accessible.

Instead, you must pass a formId to retrieve the bundle submission.

When fetching data using a formId, only fields defined in that specific form are returned from the full bundle data.

Example:
If a bundle has over 100 fields, but the form linked to a formId has only 10 fields, only those 10 fields will be returned.

🔁 How Bundle Resubmit Works
When a user resubmits a form within the bundle:

The backend fetches the full bundle submission using the form’s formId.

It merges the current form's new data with the existing bundle.

Then, it executes the rules again to evaluate what should be shown or triggered next.
