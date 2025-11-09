# 🛒 BuyOurDomain.com – WooCommerce + Custom Domain Grid Integration

This documentation explains how to **manage domain products**, **edit front-end features**, and **understand the custom code** used on the WordPress site [buyourdomain.com](https://buyourdomain.com/).  
It’s designed for developers, editors, or team members to easily maintain and update the website without breaking existing functionality.

---

## 📘 Project Overview

The website lists **domain names as products** and dynamically displays them on the homepage using **custom PHP shortcodes**, **Elementor integrations**, and **custom JavaScript** for popup and form interactions.

### 🧠 Core Custom Features
- Dynamic product grid display by category (`featured`, `portfolio`, etc.)
- “Get This Domain” popup trigger integrated with Elementor
- LocalStorage-based domain name auto-fill in popup form
- Hidden HTML element in popup for domain name injection
- Incremental “View More” loading of domains
- Responsive grid layout with gradient buttons and hover effects
- Automated email responses on enquiry submission (to user & admin)
![Homepage Grid Example](assets/buyourdomain.com.png)
---

## ⚙️ Table of Contents

1. [File Structure](#-file-structure)
2. [Website Structure](#-website-structure)
3. [Custom Code Locations](#-custom-code-placing-locations)
4. [Custom Code Explanation](#-custom-code-explanations)
5. [Popup Form – Where to Edit in Elementor](#-popup-form--where-to-edit-in-elementor)
6. [Form ID Integration](#-how-to-change-popup-form-ids-in-custom-code)
7. [Uploading Domain Products](#-how-to-upload-domain-products)
8. [Editing Homepage Sections](#-how-to-edit-homepage-section-in-elementor)
9. [Developer Notes](#%E2%80%8D-developer-notes)

---

## 📁 File Structure

| File | Location | Description |
|------|-----------|-------------|
| `functions.php` | `Appearance > Theme File Editor > functions.php` | Contains PHP shortcodes, grid logic, and popup trigger integration. |
| `elementor custom code (head)` | `Elementor > Custom Code > buy domain` | JS for domain auto-fill via LocalStorage and popup triggering. |
| `heading (HTML widget)` | `Elementor > Templates > Popups > Domain Enquiry Popup` | Hidden HTML + JS for domain injection into form fields. |
| `section shortcode` | `Elementor > Home Page > Custom Section` | Adds `[domain_grid]` shortcode for displaying domain list. |

---

## 🧩 Website Structure

- **Theme:** Astra  
- **Builder:** Elementor  
- **Custom Section:** Added via `[domain_grid]` shortcode  
- **Popup:** Elementor popup named `Domain Enquiry Popup`  
- **Form:** Elementor form widget with hidden domain field (`domain_name`)

---

## 📍 Custom Code Placing Locations

1. **Buy Section (PHP Shortcode):**  
   `Appearance > Theme File Editor > functions.php`  
   Handles product grid, button trigger, and View More logic.

2. **Elementor Custom Code (Popup Autofill):**  
   `Elementor > Custom Code > buy domain`  
   Controls popup trigger and domain data sync using LocalStorage.

3. **Popup Form Hidden HTML (Domain Sync):**  
   `Elementor > Templates > Popups > Domain Enquiry Popup`  
   Add HTML widget (hidden) with script below the form for domain name injection.

---

## 🧠 Custom Code Explanations

### 🧩 PHP (functions.php)
File: `functions.php`  
Purpose: Generate dynamic domain grid and handle “View More” functionality.

Code Source: [`theme file editor.txt`](theme%20file%20editor.txt)

Key Functions:
- `buyourdomain_domain_grid_common()` → Generates domain product cards dynamically.
- `buyourdomain_featured_domain_grid_shortcode()` → Loads featured domains.
- `buyourdomain_portfolio_domain_grid_shortcode()` → Loads portfolio domains.
- **JavaScript + CSS (inside function)** → Handles “View More” logic and styling.

---

### 💡 Elementor Custom Code (Head)
File: `elementor custom code ----------- h.txt`  
Source: [`elementor custom code.txt`](elementor%20custom%20code.txt)

```html
<script>
function openDomainPopup(button) {
  const domainName = button.getAttribute("data-domain");
  localStorage.setItem("clickedDomain", domainName);
  elementorProFrontend.modules.popup.showPopup({ id: 233 });
}

document.addEventListener("DOMContentLoaded", function () {
  const domainField = document.querySelector('input[name="form_fields[domain_name]"]');
  if (domainField) {
    const savedDomain = localStorage.getItem("clickedDomain");
    if (savedDomain) domainField.value = savedDomain;
  }
});
</script>
```
👉 This code saves the clicked domain name to LocalStorage and pre-fills it when the popup opens.

### 🧱 Popup Form Hidden HTML

File: heading -----.txt
Source: [`heading.txt`](heading.txt)
Purpose: Dynamically show and send the domain name inside the enquiry form.

```html
<script>
document.addEventListener('DOMContentLoaded', function() {
  document.querySelectorAll('.get-domain-btn').forEach(function(btn) {
    btn.addEventListener('click', function() {
      const domain = this.getAttribute('data-domain');
      const target = document.querySelector('#domain-name');
      if (target) target.textContent = domain;
    });
  });
});
</script>

<script>
document.addEventListener('DOMContentLoaded', function() {
  document.querySelectorAll('.get-domain-btn').forEach(function(btn) {
    btn.addEventListener('click', function() {
      const domain = this.getAttribute('data-domain');
      const target = document.querySelector('#domain-name');
      const hiddenInput = document.querySelector('input[name="form_fields[domain_name]"]');
      if (target) target.textContent = domain;
      if (hiddenInput) hiddenInput.value = domain;
    });
  });
});
</script>
```
## 🧩 Section Shortcode on Home Page

File: section shortcode on home page-----.txt
Source: [`section shortcode on home page.txt`](section%20shortcode%20on%20home%20page.txt)
```html
[portfolio_domain_grid]
[featured_domain_grid]
```
➡ Add this shortcode inside Elementor where you want the grid to appear (typically in a custom HTML widget or section).

## 🧾 Popup Form – Where to Edit in Elementor
Path: Elementor > Templates > Popups > Domain Enquiry Popup 
Edit the popup form using Elementor form widget.
Ensure the field ID for domain name = domain_name.
Add the hidden HTML widget (code above) under the form.
Hide this widget from all frontend devices.

## 🔧 How to Change Popup Form IDs in Custom Code
Open functions.php
Find the
```html
onclick="elementorProFrontend.modules.popup.showPopup({id: 381});" line.
```
Replace 381 with the new Popup ID (from Elementor popup settings).

## 🛍 How to Upload Domain Products
Path: WordPress Dashboard > Products > Add New

Steps: 
1. Enter domain name in Product Title (e.g., buyourdomain.com).
2. Leave price empty (for enquiry-based sales).
3. Select Category:
4. featured → Displays in Featured Grid.
5. portfolio → Displays in Portfolio Grid.
6. Click Publish.

## 🏗 How to Edit Homepage Section in Elementor

Path: Pages > Home > Edit with Elementor
- Locate the section with shortcode `[featured_domain_grid]` , `[portfolio_domain_grid]`.
- You can duplicate or move this section as needed.
- For styling, edit section padding/margins via Elementor; functionality is handled via PHP.

## 🧑‍💻 Developer Notes

- All shortcodes and frontend logic are located inside functions.php.
- Popup scripts depend on Elementor Pro.
- The hidden HTML widget ensures correct domain name injection into forms and email templates.
- Each “Get This Domain” button triggers the popup and auto-fills the clicked domain.
- Mail templates and automation are handled by Elementor Form Actions (Email, Admin Notification).

## ✅ Summary

This project integrates WooCommerce with a custom domain listing and enquiry system using PHP, JavaScript, and Elementor Pro.
It allows for dynamic domain grids, popup-triggered enquiries, and automated email flows — all managed through the WordPress dashboard.

## Maintainer: Lokendra Singh
Tech Stack: WordPress · WooCommerce · PHP · JavaScript · Elementor Pro
Project URL: buyourdomain.com
