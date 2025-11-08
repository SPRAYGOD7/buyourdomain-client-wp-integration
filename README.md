# 🛒 BuyOurDomain.com – WooCommerce + Custom Domain Grid Integration

This documentation explains how to **manage domain products**, **edit front-end features**, and **understand the custom code** used on the WordPress site [buyourdomain.com](https://buyourdomain.com/).  
It’s designed for clients or team members to easily make updates without breaking existing functionality.

---

## 📂 Project Overview

The website lists **domain names as WooCommerce products** and displays them dynamically on the homepage using **custom shortcodes** and **Elementor integrations**.

**Main customizations include:**
- Dynamic product grid display based on category (`featured`, `portfolio`, etc.)
- “Get This Domain” popup trigger integration via Elementor
- LocalStorage-based domain name auto-fill in popup forms
- “View More” functionality for incremental loading of domain cards
- Clean responsive styling and grid layout

---

## ⚙️ Table of Contents

1. [File Structure](#-file-structure)
2. [Custom Code Explained](#-custom-code-explained)
3. [How to Upload Domain Products](#-how-to-upload-domain-products)
4. [How to Edit Homepage Section](#-how-to-edit-homepage-section)
5. [Popup Form Integration](#-popup-form-integration)
6. [Screenshots](#-screenshots)
7. [Developer Notes](#-developer-notes)

---

## 📁 File Structure

| File | Description |
|------|--------------|
| `functions.php` | Contains all PHP shortcodes and grid logic. |
| `elementor custom code ----------- h.txt` | Handles popup auto-fill via LocalStorage. |
| `heading -----.txt` | Manages dynamic domain name injection into form. |
| `section shortcode on home page-----.txt` | Contains the shortcode added in Elementor section. |

---

## 🧩 Custom Code Explained

### **1️⃣ functions.php – Domain Grid System**
*(Source: [theme file editor------------------.txt](./theme%20file%20editor------------------.txt))*

#### 🔧 Function: `buyourdomain_domain_grid_common($category_slug)`
This is the core function responsible for:
- Fetching WooCommerce products from a specific category.
- Displaying them in a responsive grid.
- Adding “Get This Domain” buttons.
- Implementing the “View More” button logic.

**Shortcodes created:**
- `[featured_domain_grid]`
- `[portfolio_domain_grid]`

Add these to any Elementor “Shortcode” widget to display dynamic domain listings.

#### 🧠 Key Features:
- Displays first 8 products initially.
- Loads +4 domains with every “View More” click.
- Automatically hides “View More” when all products are visible.
- Popup (`ID: 381`) is triggered on button click.

---

### **2️⃣ Elementor Custom Code (Header Script)**
*(Source: [elementor custom code ----------- h.txt](./elementor%20custom%20code%20-----------%20h.txt))*

```js
function openDomainPopup(button) {
  const domainName = button.getAttribute("data-domain");
  localStorage.setItem("clickedDomain", domainName);
  elementorProFrontend.modules.popup.showPopup({ id: 233 });
}
