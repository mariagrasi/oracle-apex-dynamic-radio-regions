# Oracle APEX – Dynamic Region Control with Radio Group

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
// Reference the APEX Radio Group item
var item = apex.item("P4_OPTION");

// Retrieve all radio button options for the Radio Group
var list_options = getRadioValues("P4_OPTION");

// If no option is currently selected
if (!item.getValue()) {

  // Get the value of the first radio button option
  var firstValue = list_options
    .first()
    .val();

  // Set the first radio option as the default value
  item.setValue(firstValue);
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

## 🧪 Use Cases

* Conditional forms
* Wizard-style pages
* Dynamic dashboards
* Feature toggles

---

## 📄 License

This example is provided for educational and project use within Oracle APEX applications.
