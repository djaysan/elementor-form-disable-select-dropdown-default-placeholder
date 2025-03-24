# elementor-form-disable-select-dropdown-default-placeholder
Disable the placeholder for select dropdown on elementor forms

By default, Elementor’s Pro Form’s select fields lack placeholders, unlike other input fields. This isn’t a problem specific to Elementor but is related to the nature of group fields in HTML.

**Step-by-step solution**
**1. Creating a Required Select Field**
To set up a required select field, you simply need to toggle the “Required” option. But, the trouble is that it selects the first option by default which defeats the purpose of a required input.

**2. Toggle Required Option**
To prevent a default selected option, we add an invalid option that prompts users to make a selection to avoid form errors. This is achieved by adding a blank space as the first option of the select field.

**3. Add a blank first option to the select field**
Adding a placeholder
To further improve user experience, we can replace the blank space with a key-value pair, where the key is our placeholder text and the value is blank. In Elementor form terms, the option should be as “placeholder text | .”

**4. Add placeholder text with blank value**
Addressing accessibility concerns
While the above method works, the invalid option remains selectable and is read by screen readers as one of the options. To prevent this, we need to add two attributes, “hidden” and “disabled” to the option. Unfortunately, Elementor currently does not natively support adding custom attributes, so we turn to Custom JS.

**Implementing custom JS for accessibility**
To ensure the placeholder option cannot be selected, follow these steps:

Under the CSS Classes in the Advanced tab of the Elementor form widget, add a class name of **“dis-ele-form“**.
Insert the following JS Snippet using your preferred method such as a Code Snippets plugin, an HTML widget or enqueuing the script in your child theme.
Screenshot of the advanced tab of the Elementor form showing the CSS Classes set to dis-ele-form

```
<script>
document.addEventListener('DOMContentLoaded', function () {
    // Find all forms with class name "dis-ele-form"
    var forms = document.querySelectorAll('.dis-ele-form');

    // Iterate through each form
    forms.forEach(function (form) {
        // Find all select fields with the required attribute within the form
        var selects = form.querySelectorAll('select[required]');

        // Iterate through each select field
        selects.forEach(function (select) {
            // Check if the first option has a blank value before disabling and hiding it
            if (select.options.length > 0 && select.options[0].value.trim() === '') {
                select.options[0].disabled = true;
                select.options[0].hidden = true;
            }
        });
    });
});
</script>
```

**Using Modern JS Syntax:**

```
<script>
document.addEventListener('DOMContentLoaded', () => {
    // Find all forms with class name "dis-ele-form"
    const forms = document.querySelectorAll('.dis-ele-form');

    forms.forEach(form => {
        const selects = form.querySelectorAll('select[required]');

        selects.forEach(select => {
            if (select.options.length > 0 && select.options[0].value.trim() === '') {
                select.options[0].disabled = true;
                // Optionally add CSS for better UX:
                select.options[0].style.display = 'none';
            }
        });
    });
});
</script>
```
