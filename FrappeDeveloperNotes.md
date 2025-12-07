# Frappe Developer Notes

## Table of Contents

1. [Override Standard Reports in Installed Apps](#1-override-standard-reports-in-installed-apps)
2. [Generating Clickable Links in Frappe](#2-generating-clickable-links-in-frappe)
3. [Setting Default Values When Creating a New Document from a Link Field](#3-setting-default-values-when-creating-a-new-document-from-a-link-field)

### 1. Override Standard Reports in Installed Apps

You can override standard reports from other installed apps directly through your custom app.
For a full working example, visit the following repository:

[Override Standard Reports](https://github.com/niyazrazak/frappe_report_override)

Use the following hooks in your `hooks.py` to override report behaviour, scripts, and templates:
```python
# Override Report DocType class
override_doctype_class = {
    "Report": "frappe_report_override.overrides.CustomReport"
}

# Override the method that returns report scripts
override_whitelisted_methods = {
    "frappe.desk.query_report.get_script": "frappe_report_override.overrides.get_script"
}

# Override report JS
report_override_js = {
	"Job Card Summary": "reports/js/custom_job_card_summary.js",
}

# Override report Python execute function
report_override = {
 	"Job Card Summary": "frappe_report_override.reports.custom_job_card_summary.execute"
}

# Override report HTML template (if required)
report_override_html = {
	"Job Card Summary": "reports/overrides/html/custom_job_card_summary.html"
}
```

### 2. Generating Clickable Links in Frappe

To generate a clickable link string pointing to any document in Frappe, you can use either of the following functions:
```python
frappe.utils.get_link_to_form("Customer", customer.name)
frappe.get_desk_link("Customer", customer.name)
```

Both functions return a formatted, clickable link string that can be used.

### 3. Setting Default Values When Creating a New Document from a Link Field

When a user clicks “Create New” from a Link field, Frappe allows you to pre-fill fields in the new document by using:
```javascript
var df = frappe.meta.get_docfield("Opportunity Item", "item_code", frm.doc.name);
// OR: var df = frm.get_docfield("items", "item_code");
// OR: var df = frm.get_docfield("item_code");
df.get_route_options_for_new_doc = function (field) {
	return {
		is_sales_item: 1,
		customer_items: [{
			customer_name: frm.doc.customer_name,
			customer_group: frm.doc.customer_group,
		}]
	}
}
```

