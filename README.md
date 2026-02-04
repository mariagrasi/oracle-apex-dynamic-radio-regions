# Oracle APEX – Dynamic Region Control with Radio Group

<img width="1247" height="405" alt="image" src="https://github.com/user-attachments/assets/15a0a116-085a-4259-b459-f3900d7ff2b1" />

This page demonstrates a **client-side, upgrade-safe pattern** for controlling the visibility of multiple regions in **Oracle APEX (24+)** using a **Radio Group**, **Dynamic Actions**, and **native APEX JavaScript APIs**.

The solution is fully dynamic, scalable to multiple options, and avoids server-side processing or page reloads.

---

## 🎯 Objective

* Display different regions based on the selected **Radio Group** option
* Initialize a default option when the page loads
* Hide and show regions dynamically using **JavaScript nativo do APEX**
* Ensure compatibility with future APEX upgrades

---

## 🧩 Page Components

### 🔹 Page Item

* **Item Type:** Radio Group
* **Item Name:** `P4_OPTION`
* **Values:** Numeric or string values (e.g. `1`, `2`, `3`, ...)

### 🔹 Regions

Each region must have a **Static ID** following this convention:

| Radio Value | Region Static ID |
| ----------- | ---------------- |
| `1`         | `REG_1`          |
| `2`         | `REG_2`          |
| `3`         | `REG_3`          |

This naming convention enables a clean and scalable JavaScript implementation.

---

## ⚙️ Dynamic Actions

### 1️⃣ Initialize Default Option (Page Load)

**Event:** Page Load
**Action:** Execute JavaScript Code

```javascript
// Reference the Radio Group APEX item
var item = apex.item("P4_OPTION");

// If no option is selected, set the first radio option as default
if (!item.getValue()) {
  var firstValue = item.element
    .find("input[type=radio]")
    .first()
    .val();

  item.setValue(firstValue, true);
}
```

---

### 2️⃣ Control Region Visibility (Change Event)

**Event:** Change
**Selection Type:** Item(s)
**Item:** `P4_OPTION`
**Fire on Initialization:** Yes

```javascript
// Get the selected Radio Group value
var option = apex.item("P4_OPTION").getValue();

// Hide all regions associated with the Radio Group options
apex.item("P4_OPTION").element
  .find("input[type=radio]")
  .each(function () {
    var value = this.value;
    var region = apex.region("REG_" + value);
    if (region) {
      region.element.hide();
    }
  });

// Show the region that matches the selected option
if (option) {
  var selectedRegion = apex.region("REG_" + option);
  if (selectedRegion) {
    selectedRegion.element.show();
  }
}
```

---

## 🧠 Helper Function (Optional)

To encapsulate DOM access and improve maintainability:

```javascript
// Returns all radio button options for the specified APEX Radio Group item
function getRadioValues(itemName) {
  return apex.item(itemName).element.find("input[type=radio]");
}
```

---

## 🛡️ Upgrade Safety

This implementation uses **public and supported APEX APIs**:

* `apex.item().getValue()`
* `apex.item().setValue()`
* `apex.region().element`

Direct DOM access is limited and encapsulated, minimizing upgrade risk.

Tested and validated on:

* Oracle APEX 24.x

---

## ✅ Benefits

* No page submit
* No PL/SQL processes required
* Fully client-side
* Scales to any number of Radio Group options
* Clean, readable, and maintainable code

---

## 📌 Notes

* Always define **Static IDs** for regions
* Keep the Radio Group values aligned with region IDs
* Prefer Dynamic Actions over custom JavaScript events

---

## 📥 Importing the Page into Oracle APEX

This repository includes an exported Oracle APEX page that can be imported into an existing application for validation and testing.

### 🔹 Prerequisites

* Oracle APEX **24.x or higher**
* An existing APEX application (any schema)
* Developer access to App Builder

### 🔹 Steps to Import the Page

1. Download the exported page file from this repository (e.g. `f210224_page_4.sql`).
2. Log in to **Oracle APEX**.
3. Open **App Builder** and select your target application.
4. In the application home page, click **Create Page**.
5. Choose **From a File**.
6. Upload the exported page SQL file.
7. Review the import summary and click **Install Page**.

The page will be added to your application with all regions, items, and Dynamic Actions.

---

### 🔹 Post-Import Checklist

After importing the page, verify the following:

* The Radio Group item name is `P4_OPTION`
* All regions have the correct **Static IDs** (`REG_1`, `REG_2`, `REG_3`, ...)
* Dynamic Actions are enabled
* JavaScript code is present in the Dynamic Actions

Once validated, run the page and interact with the Radio Group to confirm the regions are displayed correctly.

---

## 🧪 Use Cases

* Conditional forms
* Wizard-style pages
* Dynamic dashboards
* Feature toggles

---

## 📄 License

This example is provided for educational and project use within Oracle APEX applications.
