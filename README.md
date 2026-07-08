# Elementor form: disable select dropdown default placeholder

Disables the default placeholder option in Elementor Pro Form select dropdowns, so a required select field does not pre-select its first option and the placeholder cannot be chosen or announced as a real option by screen readers.

Special thanks: David Denedo (https://daveden.co.uk/tutorials/add-placeholders-to-select-fields-in-elementor-pro-form)

## Background

By default, Elementor Pro Form select fields lack placeholders, unlike other input fields. This is not a problem specific to Elementor but is related to the nature of group fields in HTML.

## Step-by-step solution

### 1. Create a required select field

To set up a required select field, toggle the "Required" option. The trouble is that it selects the first option by default, which defeats the purpose of a required input.

### 2. Add a blank first option

To prevent a default selected option, add an invalid option that prompts users to make a selection to avoid form errors. This is achieved by adding a blank space as the first option of the select field.

### 3. Add placeholder text with a blank value

To further improve the user experience, replace the blank space with a key-value pair, where the key is your placeholder text and the value is blank. In Elementor form terms, write the option as `placeholder text | ` (with a space after the pipe).

### 4. Hide the placeholder option with JS (accessibility)

While the above method works, the invalid option remains selectable and is read by screen readers as one of the options. To prevent this, the option needs the `hidden` and `disabled` attributes. Elementor does not natively support adding custom attributes, so we use a small JS snippet:

1. Under CSS Classes in the Advanced tab of the Elementor form widget, add the class name `dis-ele-form`.
2. Insert the following JS snippet using your preferred method, such as the Code Snippets plugin, an HTML widget, or enqueuing the script in your child theme.

```html
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

Using modern JS syntax:

```html
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

## Notes

- You can reuse the same class name (`dis-ele-form`) on multiple forms on the page and it will work just fine. You do not have to rewrite the JavaScript.
- If you want it to work on all Elementor forms on your page, replace `.dis-ele-form` with `.elementor-form` in the script.

## Support

If this saved you time, you can support my work:

[<img src="https://storage.ko-fi.com/cdn/kofi2.png?v=6" alt="Buy Me a Coffee at ko-fi.com" height="36">](https://ko-fi.com/H2H81I6YY1)
