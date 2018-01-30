Workshop Butler allows users to configure what information they want to collect
during the registration. The users can add new fields of several different types,
make fields optional or required, and add fill-in instructions for the forms.

As a result, a set of attendee attributes, passed to the system through API for
successful registration, can differ from event to event. You can hard-code
a predefined set of fields on a registration form but you will limit a user's
ability to tweak the system for their needs.

This section explains how to implement registration forms correctly.

### Rendering a Form

When you request an [event](#events-events-get), the response contains two attributes,
related to the look of the form:

- `instructions`
- `registration_form`

The former one is a text with some instructions from the user to visitors on how
to fill-in the form. For example,

- *Add your food preferences to the comment*
- *Please, describe what your main goal for joining a workshop is*

The latter one is an array with sections and fields, the user expects visitors to fill in.
Each section has a name and one or several fields it contains.

The sections and the fields are ordered the way a user wants. Try not to change
the order if there are no other requirements from your users.

For example, this is a simplified form structure:

````json
[{
   "name": "Personal Info",
   "fields": [
     ...
   ]
 },{
   "name": "Billing Address",
   "fields": [
     ...
   ]
 }]
````

Based on it, you can create a form like:

````HTML
<form>
  <fieldset>
    <legend>Personal Info</legend>
    <p>
      <input ....>
    </p>
  </fieldset>
  <fieldset>
    <legend>Billing Address</legend>
    <p>
      <input ....>
    </p>
  </fieldset>
</form>
````

### Rendering Fields
Each field has four attributes: `name`, `type`, `required` and `label`. `select`
field contains one more attribute with the list of options.

````json
{
  "name": "first_name",
  "type": "text",
  "required": true,
  "label": "First Name"
}
````

From this JSON you may create a code similar to:

````HTML
<label for="first_name" class="required">
  First Name
  <input type="text" name="first_name"/>
</label>
````

`name` is an unique field name which is passed to [through API](#attendees-attendee-list-post-1)
along with a field's value. Using the `name`, Workshop Butler maps the passed
parameters to relevant attendee's attributes.

::: warning
It is up to you to check if a visitor fills in the fields with `required=true`.
The system returns an error if one or several required fields are empty.  
:::

There are 8 field types:

Type | Description
----:| ----
text | Basic input field, ````HTML <input type="text" ..>````
checkbox | Checkbox with true/false value, ````HTML <input type="checkbox" ...>````
select | Select field, ````HTML <select name=""><option>... </select>````
date | Date in format YYYY-MM-DD, ````HTML <input type="date" ...>````
textarea | Large text input, ````HTML <textarea ....>````
email | Email, ````HTML <input type="email" ...>````
country | List of countries to select from. A passed value for this field should be an ISO 8166-2 country code (**de**, **RU**).
ticket | A selector with available ticket types. The ticket types are in `paid_ticket_types` attribute.

Workshop Butler gives you plenty of information to render a beautiful yet
fully functional dynamical registration form. 
