# 🛒 BuyOurDomain.com – WooCommerce + Custom Domain Grid Integration

This documentation explains how to **manage domain products**, **edit front-end features**, and **understand the custom code** used on the WordPress site [buyourdomain.com](https://buyourdomain.com/).  
It’s designed for clients or team members to easily make updates without breaking existing functionality.

---

## 📂 Project Overview

The website lists **domain names as products** and displays them dynamically on the homepage using **custom shortcodes** and **Elementor integrations**.

**Main customizations include:**
- Dynamic product grid display based on category (`featured`, `portfolio`, etc.)
- “Get This Domain” popup trigger integration via Elementor (custom code + Elementor Pro popup & form)
- LocalStorage-based domain name auto-fill in popup forms
- “View More” functionality for incremental domain loading
- Responsive and visually clean grid layout
- Automated mail responses to both the **site owner** and **user** on domain inquiry submission with custom mail templates

---

## ⚙️ Table of Contents

1. [Website Structure](#-website-structure)  
2. [Custom Code Locations](#-custom-code-placing-locations)  
3. [Custom Code Explanation](#-custom-code-explanations)  
4. [Popup Form in Elementor](#-pop-up-form-where-to-edit-in-elementor)  
5. [Changing Popup Form IDs](#-how-to-change-popup-form-ids-to-main-custom-code)  
6. [How to Upload Domain Products](#-how-to-upload-domain-products)  
7. [How to Edit Homepage Section](#-how-to-edit-homepage-section-in-elementor)  
8. [Developer Notes](#-developer-notes)

---

## 📁 File Structure

| File | Description |
|------|--------------|
| `functions.php` | Contains PHP shortcodes, grid logic, “Get This Domain” button, and popup trigger integration. |
| `elementor custom code.txt` | Handles popup auto-fill via LocalStorage. |
| `heading.txt` | Custom HTML code to dynamically display and store the clicked domain name in Elementor forms. Should be placed in an HTML widget and hidden on frontend for all devices. |
| `section shortcode on home page.txt` | Contains the shortcode used in Elementor to render the domain grid. |

---

## 🧩 Website Structure

- **Frontend:** Built using the **Astra Theme** and **Elementor Page Builder**.  
- **Custom Section:** Domain “Buy Now” section added via shortcode inside Elementor.  
- **Popup:** Created using Elementor’s Popup Builder.  
- **Buy Now Form:** Designed using Elementor Form widget inside the popup.

---

## 🗂️ Custom Code Placing Locations

| Location | File | Description |
|-----------|------|-------------|
| 1️⃣ | `Appearance → Theme File Editor → functions.php` | Contains main grid display logic and shortcode. (File: `theme file editor`) |
| 2️⃣ | `Elementor → Custom Code → Buy Domain` | JavaScript for popup and form auto-fill using LocalStorage. (File: `elementor custom code`) |
| 3️⃣ | `Elementor → Templates → Popups → Domain Enquiry Popup` | Hidden HTML widget containing dynamic JavaScript that updates the domain name inside the popup form. (File: `heading.txt`) |

*(You can add relevant screenshots for each location in this section.)*

---

## 🧠 Custom Code Explanations

### **1️⃣ functions.php (Main Grid Logic)**
**Purpose:**  
This file generates the product grid dynamically from WooCommerce categories and connects the “Get This Domain” button with the popup.

**Key Functions:**
- `buyourdomain_domain_grid_common($category_slug)` → Core logic for the domain grid  
- `[featured_domain_grid]` & `[portfolio_domain_grid]` → Shortcodes to render domain grids in Elementor

**Editable Lines:**
- **Popup ID:**  
  Located around this line:
  ```php
  onclick="elementorProFrontend.modules.popup.showPopup({id: 381});"
