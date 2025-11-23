# Generating Clickable Links in Frappe

To generate a clickable link string pointing to any document in Frappe, you can use either of the following functions:
```python
frappe.utils.get_link_to_form("Customer", customer.name)
frappe.get_desk_link("Customer", customer.name)
```

Both functions return a formatted, clickable link string that can be used.
