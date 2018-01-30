An attendee represents a person participating in an event and has three required
attributes: first name, last name and email. Other attributes may be either optional
or required, depending on user settings.

## Important Info

There are two ways to create an attendee. The difference is in how Workshop Butler
reacts for a new attendee.

When you [create a new attendee](#attendees-attendee-list-post), the system:

1. adds a new attendee object
2. sets the attendee's `participated` attributes to `true`

When you [register a new attendee](#attendees-attendee-list-post-1), the system:

1. checks if a selected ticket is available
1. adds a new attendee object
1. triggers `New Attendee` event.

Getting `New Attendee` event the system:

- sends a welcome email to the attendee's email address.
- sends a notification to team members about new registration.

While integrating your website with Workshop Butler, prefer to use [`register`](#attendees-attendee-list-post-1) action
for a registration form. Use [`create`](#attendees-attendee-list-post) action only on an evaluation form, when you know
for sure the attendee has participated.

## Attendee List [/attendees]

### Create New Attendee [POST]

Adds a new attendee to the system without additional checks and without triggering
`New Attendee` event.

On success, the system returns `201` response code. On failure, the system returns:

- `403` response code when the account does not have an access to the event and cannot add a new attendee to it;
- `422` response code when the request has invalid data and cannot be processed


+ Attributes
    + event_id (number, required) The ID of the event the attendee participated at
    + first_name (string, required) The first name of the attendee
    + last_name (string, required) The last name of the attendee
    + email: first.last@workshopbutler.com (string, required) The email address is an identifier of the attendee.
    Each event can have only one attendee with an unique email address.
    If a new attendee and an existing attendee have identical email addresses, then their
    attributes are merged.
    + date_of_birth (string) - Date of birth in YYYY-mm-DD format
    + country: DE - Country of origin, ISO 8166-2 2-letters code
    + city: Berlin (string)
    + street_1 (string)
    + street_2 (string)
    + province (string)
    + post_code (string)
    + organisation: Bug Cleaner Ltd. (string) - Name of organisation where the attendee works at
    + comment (string)
    + role: Manager (string) - Job title
    + opt_out (boolean) - True if you don't want to include the attendee to third-party mailing list
        + default: false


+ Request (application/json)

        {
            "event_id": 123,
            "first_name": "Carl",
            "last_name": "Maddow",
            "email": "carl.maddow@test.com",
            "date_of_birth": "1951-09-17",
            "country": "NL",
            "city": "Amsterdam",
            "province": "Holland",
            "organisation": "Lost Beauty BV",
            "role": "Developer"
        }

+ Response 201 (application/json)

    + Attributes (object)
        + attendee_id (number) - The ID of a created attendee

    + Body

            {
                "attendee_id": 1234
            }

+ Response 403 (application/json)

    + Body

            {
                "code": 403,
                "message": "Forbidden Request",
                "info": { "event_id": "This parameter is required" }
            }

+ Response 422 (application/json)

    + Body

            {
                "code": 422,
                "message": "Validation failed",
                "info": { "event_id": "This parameter is required" }
            }


### Register New Attendee [POST /attendees/register]

Adds a new attendee to the system after a ticket check, and triggers `New Attendee`
event.

::: note
You should prefer this action to [`create`](#attendees-attendee-list-post). For more details, read [Important info](#header-important-info).
:::

The POST parameters for this request vary and are based on the configuration of
registration form. For more information, read [Registration Forms](#registraiton-forms).

On success, the system returns `201` response code. On failure, the system returns:

- `403` response code when the account does not have an access to the event and cannot add a new attendee to it;
- `422` response code when the request has invalid data and cannot be processed


+ Attributes
    + event_id (number, required) The ID of the event the attendee participated at
    + first_name (string, required) The first name of the attendee
    + last_name (string, required) The last name of the attendee
    + email: first.last@workshopbutler.com (string, required) The email address is an identifier of the attendee.
    Each event can have only one attendee with an unique email address.
    If a new attendee and an existing attendee have identical email addresses, then their
    attributes are merged.
    + date_of_birth (string) - Date of birth in YYYY-mm-DD format
    + country: DE - Country of origin, ISO 8166-2 2-letters code
    + city: Berlin (string)
    + street_1 (string)
    + street_2 (string)
    + province (string)
    + post_code (string)
    + organisation: Bug Cleaner Ltd. (string) - Name of organisation where the attendee works at
    + comment (string)
    + role: Manager (string) - Job title
    + opt_out (boolean) - True if you don't want to include the attendee to third-party mailing list
        + default: false

+ Response 201 (application/json)
    + Body

            {
                "attendee_id": 1234
            }

+ Response 403 (application/json)

    + Body

            {
                "code": 403,
                "message": "Forbidden Request"
            }

+ Response 422 (application/json)

    + Body

            {
                "code": 422,
                "message": "Validation failed",
                "info": { "event_id": "This parameter is required" }
            }

### Get Attendees [GET /attendees{?sort}]

Returns the list of all attendees for the account. The attendees are returned sorted by creation date,
with the most recent attendees appearing last.

::: warning
#### <i class="fa fa-warning"></i> Caution
this method is available only for trainer's accounts. The owners of knowledge
brand accounts will get `403 Forbidden` response.
:::

+ Parameters
    + sort (enum[string], optional) - Sort field and order

        + Default: `+created`

        + Members
            + `+created`
            + `-created`

+ Response 200 (application/json)

    + Attributes (array[Attendee])
