---
title: Validator
id: validator
prev: grid-model-collection.html
next: filter-adapter.html
---

UIKernel provides a way to validate data, so that the user can be notified of invalid input before submitting a form or changes in an editable grid.

### createValidator

{% highlight javascript %}
UIKernel.createValidator()
{% endhighlight %}

Returns a builder that allows to define validation rules.

---

## Validator methods

### field

{% highlight javascript %}
Validator field(string field, [string|null function validator(value), ...])
{% endhighlight %}

Add synchronous field validation.


### fields

{% highlight javascript %}
Validator fields(string[] fields, function validatorFunction(Object record, ValidationErrors errors))
{% endhighlight %}

Specify multiple synchronous validators for a group of fields. If errors occur, function complements errors object.

---

### asyncDependence

{% highlight javascript %}
Validator asyncDependence(string[] fields)
{% endhighlight %}

Specifies server validation dependencies in a client validator.

---

### asyncField

{% highlight javascript %}
Validator asyncField(string field, function validator)
{% endhighlight %}

Add an asynchronous validator to a field.

---

### asyncFields

{% highlight javascript %}
Validator asyncFields(string[] fields, function validator)
{% endhighlight %}

Add an asynchronous validator to fields. If any errors occur, the callback with the parameter being  an `ValidationErrors` object will be invoked.

> `err` contains the error that occurred during the asynchronous function execution.
The `errors` parameter contains validation errors.

---

## Built-in Validation Rules

A set of basic validation rules is provided:

{% highlight javascript %}
// Check if value is boolean
UIKernel.Validators.boolean(string errorMessage)

// Check if date matches min-to-max range
UIKernel.Validators.date(Date min, Date max, string errorMessage)

// Check if variants contain the value
UIKernel.Validators.enum(Array variants, string errorMessage)

// Check if value is float
UIKernel.Validators.float(number min, number max, string errorMessage)

// Check if value matches [id, string label] form
UIKernel.Validators.listElement(string errorMessage)

// Check if value matches [id, string label] form and id is not null
UIKernel.Validators.listElement.isRequired(string errorMessage)

// Check if value is not empty
UIKernel.Validators.notNull(string errorMessage)

// Check if value is integer
UIKernel.Validators.number(number min, number max, string errorMessage)

// Check if value matches regular expression
UIKernel.Validators.regExp(RegExp regExp, string errorMessage)
{% endhighlight %}
[Sources]({{ site.github }})

---

## Custom Validation Rules

You can also create your own validation rules. For example:
{% highlight javascript %}
function (value) {
  if (value % 2 === 0) {
    return 'Numbers can be odd only';
  }
}
{% endhighlight %}

---

## Validation Example

{% highlight javascript %}
var validator = UIKernel.createValidator()
  // Check if country's present
  .field('country', Validators.notNull('Invalid country.'))
  // Check if email is valid using regular expressin
  .field('email', Validators.regExp(
    /^.*@.*$/,
    'Your email is invalid'
  ))
  // Check data isn't less than '2014-10-01'
  .field('timestamp', Validators.date(
    '2014-10-01', null,
    'Timestamp must exceed 2014-10-01'
  ))
  // Check if the age matches 15-90 range
  .field('age', Validators.number(15, 90,
    'You do not have the right age'
  ))
  // Check if credit isn't less than 30.5
  .field('credit', Validators.float(30.5, null,
    'You can not take the credit is less than 30.5'
  ))
  // Check if user agrees with terms of use
  .field('agree', Validators.boolean(true,
    'Select that you agree with the rules.'
  ))
  // Check if a number is odd
  .field('someField', function (value) {
    if (value % 2 === 0) {
      return 'Numbers can be odd only';
    }
   })

{% endhighlight %}

---

## Building result

### getValidationDependency

{% highlight javascript %}
 string[] getValidationDependency(string[] fields)
{% endhighlight %}

Get all dependent fields validation needs

---

### isValidRecord

{% highlight javascript %}
ValidationErrors|null isValidRecord(Object record, function callback)
{% endhighlight %}

Check record validity

