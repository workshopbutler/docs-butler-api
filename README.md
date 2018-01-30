FORMAT: 1A
HOST: https://api.workshopbutler.com

# Workshop Butler API

The Workshop Butler API provides a set of methods to integrate
accounts of knowledge brands and independent trainers with their websites
to show scheduled events and licensed trainers, accept registrations and
evaluations.

Our API has predictable, resource-oriented URLs, and uses HTTP response
codes to indicate API errors. We use built-in HTTP features,
like HTTP authentication and HTTP verbs, which are understood by
off-the-shelf HTTP clients.

JSON is returned by all API responses, including errors.
All input should be sent in JSON format.

> Workshop Butler API provides only a limited functionality, and cannot be
> considered as a full-scaled REST API for Workshop Butler platform.

## Authentication

API requests require an API key. This key is unique for your account. Do not share your secret API keys in publicly
accessible areas such GitHub, client-side code, and so forth.

All API requests must be made over HTTPS. API requests without authentication will also fail.

There are two ways you can pass API key. The first one is as
GET query parameter.

**GET Query parameter**

````http
curl 'https://api.workshopbutler.com/facilitators?api_key=YOUR_KEY'
````

The second option is HTTP header.

**HTTP Header**

````http
curl -H 'X-Api-Key: YOUR_KEY' 'https://api.workshopbutler.com/facilitators'
````

### API Keys
The API supports two types of accounts: knowledge brand and trainer. Each type of accounts
has its own key pattern.

API keys for knowledge brand accounts has `ppk_` prefix. API keys for trainers are shorter
and starts with `tpk_`.

To acquire the key:

- for a knowledge brand, open **API** tab in **Account Settings**
- for a trainer, open **Website Integration** tab in **Account Settings**.

## Errors
Workshop Butler uses HTTP response codes to report about an error. Any response with
4xx or 5xx is an error and should be handled appropriately.

All errors are in JSON format. Each error contains two required attributes: `code` and `message`.
`code` has an internal numeric identifier of the error. `message` gives you a better understanding
why the error occurred. Some errors may have additional attribute `info` which contains
a detailed description of the error.  

The example of an ordinary API error is below:

```json
{
  "code": 404,
  "message": "Not Found",
  "info": "The event with id=405 is not found"
}
```  

# Data Structures

<!-- include(data-structures.md) -->

# Group Registration Forms

<!-- include(forms.md) -->

# Group Attendees

<!-- include(attendees.md) -->

# Group Evaluations

## Evaluations [/evaluations]

An evaluation is a feedback provided by attendee to trainer. It consists of the answers
to 10 questions.

### Create New Evaluation [POST]

Adds a new evaluation to the system and informs a trainer (or trainers) about it
via email

::: warning
#### <i class="fa fa-warning"></i> Caution
this method is available only for knowledge brand's accounts. The owners of trainer
accounts will get `403 Forbidden` response.
:::

+ Attributes
    + event_id (number, required) - Event identifier, the evaluation belongs to
    + attendee_id (number, required) - Attendee ID
    + reason_to_register (string, required)
    + action_items (string, required)
    + changes_to_content (string, required)
    + facilitator_review (string, required)
    + changes_to_host (string, required)
    + facilitator_impression (number, required) - Value between [0, 10]
    + recommendation_score (number, required) - Value between [0, 10]
    + changes_to_event (string, required)
    + content_impression (number, required) - Value between [0, 10]
    + host_impression (number, required) - Value between [0, 10]

+ Response 201 (application/json)

    + Attributes
        + evaluation_id: 4302 (number) - The ID of a created evaluation

    + Body

            {
                "evaluation_id": 1234
            }

+ Response 403 (application/json)

    + Body

            {
                "code": 403,
                "message": "Forbidden Request"
            }

+ Response 409 (application/json)

    + Body

            {
                "code": 403,
                "message": "An evaluation for the attendee exists. Only one evaluation per attendee per event is allowed"
            }

+ Response 422 (application/json)

    + Body

            {
                "code": 422,
                "message": "Validation failed",
                "info": { "event_id": "This parameter is required" }
            }

### Get Evaluations [GET /evaluations{?sort}]

Returns the list of all your evaluations. The evaluations are returned sorted by creation date,
with the most recent evaluations appearing last.

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

    + Attributes (array[Evaluation])


# Group Events

<!-- include(events.md) -->

# Group Event Requests
## Event Requests [/event-requests]

### Create New Event Request [POST]

Adds a new event request to the system

+ Attributes
    + name (string, required) - Lead name
    + email (string, required) - Lead's email
    + country (string, required) - ISO 8166-2 country code
    + city: Orlando, London (array[string], optional) - List of cities
    + language: Russian, English (array[string], optional) - List of languages
    + start_date: "2017-09-17" (string, optional) - Start date of the period, YYYY-mm-DD
    + end_date: "2017-10-19" (string, optional) - End date of the period, YYYY-mm-DD
    + comment (string, optional)
    + number_of_attendees (number, required)

+ Response 201 (application/json)

+ Response 400 (application/json)

# Group Facilitators

<!-- include(facilitators.md) -->
