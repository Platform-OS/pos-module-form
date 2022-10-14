# Form

Form related operations.

## Usage

### Error registry

In your form handler you can register errors by calling `'modules/form/lib/commands/error/register'` function with the `form_name` and the `errors` object.

When you rendering your form, you can search for the errors by calling `modules/form/lib/queries/error/find` with the `form_name`

## Examples

### Register and error in the rest handler

```
function subscription = 'modules/subscribe/lib/commands/subscriber/create', first_name: params.first_name, last_name: params.last_name, email: params.email
unless subscription.valid
  function res = 'modules/form/lib/commands/error/register', form_name: 'subscribe', errors: subscription.errors
endunless
assign redirect_path = params.redirect_to | default: '/'
redirect_to redirect_path
```

### Searching for the errors when rendering a form

```
function errors = 'modules/form/lib/queries/error/find', form_name: 'subscribe'
theme_render_rc 'subscribe/organisms/subscribe', errors: errors
```
