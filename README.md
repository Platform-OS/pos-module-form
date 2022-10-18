# Form

Form related operations.

## Usage

### Error registry

In your form handler you can register errors by calling `'modules/form/lib/commands/error/register'` function with the `form_name` and the `errors` object.

When you rendering your form, you can search for the errors by calling `modules/form/lib/queries/error/find` with the `form_name`. When you loading and error the error will be deleted after the search by default. You can keep the loaded errors by set `clear` argument to `false`.

### Field errors

In your validation, you can register the fields error by calling `modules/form/lib/commands/field_error/register` with the `field_name` and the `error` strings. This function will return with the error object. If you want, you can send `errors` object to the function and it will register to error into that.

### Form values storage

You can store you filed values by calling `modules/form/lib/commands/value/store` function with the `form_name` and the `values` object. You van load the form values by calling `modules/form/lib/queries/value/find` with the `form_name`

## Examples

### Register errors and store form values in the rest handler

```
function subscription = 'modules/subscribe/lib/commands/subscriber/create', first_name: params.first_name, last_name: params.last_name, email: params.email
unless subscription.valid
  function _ = 'modules/form/lib/commands/error/register', form_name: 'subscribe', errors: subscription.errors
  function _ = 'modules/form/lib/commands/value/store', form_name: 'subscribe', values: subscription
endunless
assign redirect_path = params.redirect_to | default: '/'
redirect_to redirect_path
```

### Searching for the errors and stored valued when rendering a form

```
function errors = 'modules/form/lib/queries/error/find', form_name: 'subscribe'
function values = 'modules/form/lib/queries/value/find', form_name: 'subscribe'
theme_render_rc 'subscribe/organisms/subscribe', errors: errors, values: values
```

### Registering field errors

```
assign errors = "{}" | parse_json
assign valid_email = object['email'] | is_email_valid
unless valid_email
  function errors = 'modules/form/lib/commands/field_error/register', errors: errors, field_name: 'email', error: 'Email is not valid'
endunless

function count = 'modules/subscribe/lib/queries/subscriber/count', email: object['email']
if count > 0
  function errors = 'modules/form/lib/commands/field_error/register', errors: errors, field_name: 'email', error: 'Email is  already subscribed'
endif
```
