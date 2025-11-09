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

---

## ⚙️ Table of Contents

1. [File Structure](#-file-structure)
2. [Website Structure](#-website-structure)
3. [Custom Code Locations](#-custom-code-placing-locations)
4. [Custom Code Explanation](#-custom-code-explanations)
5. [Popup Form Configuration](#-pop-up-form-where-to-edit-in-elementor)
6. [Form ID Integration](#-how-to-change-popup-form-ids-in-custom-code)
7. [Uploading Domain Products](#-how-to-upload-domain-products)
8. [Editing Homepage Sections](#-how-to-edit-homepage-section-in-elementor)
9. [Developer Notes](#-developer-notes)

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

Code Source: [`theme file editor------------------.txt`](theme%20file%20editor------------------.txt)

Key Functions:
- `buyourdomain_domain_grid_common()` → Generates domain product cards dynamically.
- `buyourdomain_featured_domain_grid_shortcode()` → Loads featured domains.
- `buyourdomain_portfolio_domain_grid_shortcode()` → Loads portfolio domains.
- **JavaScript + CSS (inside function)** → Handles “View More” logic and styling.

---

### 💡 Elementor Custom Code (Head)
File: `elementor custom code ----------- h.txt`  
Source: :contentReference[oaicite:0]{index=0}

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
