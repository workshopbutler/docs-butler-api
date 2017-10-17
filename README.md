FORMAT: 1A
HOST: http://api.workshopbutler.com/

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

`curl 'https://api.workshopbutler.com/facilitators?api_key=YOUR_KEY'`

The second option is HTTP header.

**HTTP Header**

`curl -H 'X-Api-Key: YOUR_KEY' 'https://api.workshopbutler.com/facilitators'`

### API Keys
The API supports two types of accounts: knowledge brand and trainer. Each type of accounts
has its own key pattern.

API keys for knowledge brand accounts has `ppk_` prefix. API keys for trainers are shorter
and starts with `tpk_`.

To acquire the key:

- for a knowledge brand, open **API** tab in **Settings**
- for a trainer, open **Website Integration** tab in **Profile**.

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

## Address (object)
+ country (string) - Name of country
+ city (string) 
+ street_1 (string)
+ street_2 (string)
+ postcode (string)
+ province (string)

## TaxInfo (object)
+ vat_name: IVA (string) - Name of VAT
+ vat_number (string)

## Location (object)
+ country (string) - Name of country
+ country_code (string) - ISO 8166-2 2-letters country code
+ city (string)

## BriefEvent (object)
+ id: 43AB243 (string) - The ID of the event 
+ title (string) - The name of the event
+ start (string) - Start date in YYYY-mm-DD format
+ end (string) - End date in YYYY-mm-DD format
+ location (Location)

## Attendee (object)
+ id (number) - The ID of the attendee
+ first_name (string) 
+ last_name (string)
+ date_of_birth (string) - Date of birth in YYYY-mm-DD format
+ email (string)
+ country (string) - Name of country
+ city (string) 
+ street_1 (string)
+ street_2 (string)
+ postcode (string)
+ province (string)
+ organisation: Bug Cleaner Ltd. (string) - Name of organization where the attendee works at
+ work_address (Address, optional)
+ billing_address (Address, optional)
+ tax_info (TaxInfo, optional)
+ phone (string)
+ comment (string)
+ blog (string)
+ website (string)
+ role (string) - Job Title
+ price: EUR 100 (string) - The amount with currency which the attendee paid for entering the event
+ event (BriefEvent)

## Error (object)
+ code (number) - Internal code number
+ message (string) - Human-readable error description
+ info (string, optional) - Additional information about the error

## Badge (object)
+ name (string) - Name of the badge
+ url (string) - Badge image

## Endorsement (object)
+ content (string)
+ name (string) - Author's name
+ company (string) - Name of the author's company and/or a job title
+ rating (number, optional)

## Material (object)
+ type (string) - article, casestudy or video
+ link (string)

## Organisation (object)
+ name (string)
+ country (string) - ISO 8166-2 2-letters country code
+ city (string)
+ website (string)

## Person (object)

+ id (number) - Id of the person
+ first_name (string)
+ last_name (string)
+ photo (string) - Url of the person's photo
+ country (string) - ISO 8166-2 2-letters country code

## Attendees [/attendees]

An attendee object represents a person participating in an event. 

+ Model (application/json)

    + Attributes
        + id (number) - The ID of the attendee
        + first_name (string) 
        + last_name (string)
        + date_of_birth (string) - Date of birth in YYYY-mm-DD format
        + email (string)
        + country (string) - Name of country
        + city (string) 
        + street_1 (string)
        + street_2 (string)
        + postcode (string)
        + province (string)
        + organisation: Bug Cleaner Ltd. (string) - Name of organization where the attendee works at
        + work_address (Address, optional)
        + billing_address (Address, optional)
        + tax_info (TaxInfo, optional)
        + phone (string)
        + comment (string)
        + blog (string)
        + website (string)
        + role (string) - Job Title
        + price: EUR 100 (string) - The amount with currency which the attendee paid for entering the event
        + event (BriefEvent)


### Create an attendee [POST]

There are two ways to create an attendee. The difference is in how Workshop Butler
reacts for a new attendee. 

When you create a new attendee, the system:
 
1. adds a new attendee object
2. sets the attendee's `participated` attributes to `true` 

When you register a new attendee, the system:

1. checks if a selected ticket is available 
1. adds a new attendee object
1. sends a welcome email to the attendee's email address.

While integrating your website with Workshop Butler, prefer to use `register` action 
for a registration form. Use `create` action only on an evaluation form, when you know
for sure the attendee has participated.

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
    + organization: Bug Cleaner Ltd. (string) - Name of organization where the attendee works at
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
            "organization": "Lost Beauty BV",
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

    + Attributes (Error)
    + Body
    
            {
                "code": 403,
                "message": "Forbidden Request"
            }

+ Response 422 (application/json)

    + Attributes (Error)
    + Body
    
            {
                "code": 422,
                "message": "Validation failed",
                "info": { "event_id": "This parameter is required" }
            }
  

### Register an attendee [POST /attendees/register]

When you register a new attendee, the system:

1. checks if a selected ticket is available 
1. adds a new attendee object
1. sends a welcome email to the attendee's email address.

You should prefer this action to `create`. For more details, check **Create an attendee** action.

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
    + organization: Bug Cleaner Ltd. (string) - Name of organization where the attendee works at
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

    + Attributes (Error)
    + Body
    
            {
                "code": 403,
                "message": "Forbidden Request"
            }

+ Response 422 (application/json)

    + Attributes (Error)
    + Body
    
            {
                "code": 422,
                "message": "Validation failed",
                "info": { "event_id": "This parameter is required" }
            }

### List all attendees [GET /attendees{?sort}]

Returns the list of all your attendees. The attendees are returned sorted by creation date,
with the most recent attendees appearing last.

**IMPORTANT:** this method is available only for trainer's accounts. The owners of knowledge
brand accounts will get `403 Forbidden` response.

+ Parameters
    + sort (enum[string], optional) - Sort field and order
  
        + Default: `+created`
        
        + Members
            + `+created`
            + `-created`
                                
+ Response 200 (application/json)

    + Attributes (array[Attendee])
    


## Evaluations [/evaluations]

### Create an evaluation [POST]

Add a new evaluation to the system in <code>Pending</code> state

+ Attributes
    + event_id (number, required) - Event identifier, the evaluation belongs to
    + attendee_id (number, required) - Attendee identifier
    + reason_to_register (string, required)
    + action_items (string, required)
    + changes_to_content (string, required)
    + facilitator_review (string, required)
    + changes_to_host (string, required)
    + facilitator_impression (number, required) - Value between [0, 10]
    + recommendation_score (number, required) - Value between [0, 10]
    + changes_to_event (string, required)
    + content_impression (string, required)
    + host_impression (string, required)

+ Response 201 (application/json)
    + Body

            {
                "evaluation_id": 1234
            }

+ Response 400 (application/json)

## Event Collection [/events]

### Event [GET /events/{id}]

Get a detailed info about an event

+ Parameters
    + id (number) - Id of the event

+ Attributes
    + id (number) - Id of the event
    + title (string)
    + spoken_languages: ["English", "German"] (array) - Languages of the event in English
    + materials_language: "Russian" (string, optional)
    + description (string, optional)
    + additional_info (string, optional) - DEPRECATED
    + start: "2017-05-19" (string) - Start date of the event
    + end: "2017-06-20" (string) - End date of the event
    + hours_per_day: 5 (number)
    + total_hours: 16 (number) - Total length of the event
    + city: "London" (string)
    + country: "GB" (string) - ISO 8166-2 2-letters country code
    + website (string, optional)
    + registration_page (string)
    + type (object) - Type of the event
        + id (number) - Id of the type
        + name (string) - Name of the type
    + facilitators (array[Person]) - Trainers who run the event
    + rating (number) - Rating of the event based on its evaluations
    + confirmed (boolean) - True if the event is confirmed
    + free (boolean) - True if the event is free
    + online (boolean) - True if the event is online


+ Response 200 (application/json)
    + Body

            {
                "id": 1234,
                "title": "Management 3.0 - Curso de 2 días",
                "type" : {
                    "id": 1,
                    "name": "Standard 2-days Course"
                },
                "description" : "Curso Oficial Management 3.0 en Castellano",
                "spoken_languages" : [ "Spanish" ],
                "materials_language" : "English",
                "additional_info" : "precios Early Bird hasta el 31 de ....",
                "start" : "2013-02-04",
                "end" : "2013-02-05",
                "hours_per_day" : 8,
                "total_hours" : 16,
                "facilitators" : [ {
                    "id" : 76,
                    "first_name" : "Angel",
                    "last_name" : "Medinilla",
                    "photo" : "https://secure.gravatar.com/avatar/bfe39d10318bb80b9efe49c72065c548?s=300",
                    "country" : "ES"
                } ],
                "city" : "Barcelona",
                "country" : "ES",
                "website" : "http://www.proyectalis.com/2012/12/10/semana-de-formacion-barcelona-2013/",
                "registration_page" : "http://www.proyectalis.com/2012/12/10/semana-de-formacion-barcelona-2013/",
                "rating" : 5.5,
                "confirmed" : true,
                "free" : true,
                "online": false
            }

+ Response 400 (application/json)

### Event List [GET /events?future={future}&public={public}&achived={archived}&countryCode={country}&eventType={type}]

Get a list of events

+ Parameters
    + future (boolean, optional) - If true, only future events in the list. If false, only past events in the list.
    + public (boolean, optional) - If true, only public events in the list. If false, only private events int he list.
    + archived (boolean, optional) - If true, only archived events in the list. If false, only active events in the list
    + country: "DE" (string, optional) - ISO 8166-2 country code. If set, only events from this country in the list.
    + type: 1 (number, optional) - Id of event type. If set, only events with this type in the list.

+ Attributes
    + id (number) - Id of the event
    + title (string)
    + spoken_languages: ["English", "German"] (array) - Languages of the event in English
    + start: "2017-05-19" (string) - Start date of the event
    + end: "2017-06-20" (string) - End date of the event
    + hours_per_day: 5 (number)
    + total_hours: 16 (number) - Total length of the event
    + city: "London" (string)
    + country: "GB" (string) - ISO 8166-2 2-letters country code
    + type (object) - Type of the event
        + id (number) - Id of the type
        + name (string) - Name of the type
    + facilitators (array[Person]) - Trainers who run the event
    + rating (number) - Rating of the event based on its evaluations
    + confirmed (boolean) - True if the event is confirmed
    + free (boolean) - True if the event is free
    + online (boolean) - True if the event is online


+ Response 200 (application/json)
    + Body

            [{
              "id" : 193,
              "title" : "Management 3.0 two day course",
              "type" : {
                "id": 1,
                "name": "Standard 2-days Course"
              },
              "spoken_languages" : [ "German" ],
              "start" : "2011-11-24",
              "end" : "2011-11-25",
              "hours_per_day" : 8,
              "total_hours" : 16,
              "facilitators" : [ {
                "id" : 31,
                "first_name" : "Mischa",
                "last_name" : "Ramseyer",
                "photo" : null,
                "country" : "CH"
              } ],
              "city" : "Zürich",
              "country" : "CH",
              "rating" : 8.0,
              "confirmed" : true,
              "free" : false,
              "online" : false
            }, {
              "id" : 270,
              "title" : "Management 3.0 two day course",
              "type" : {
                "id": 1,
                "name": "Standard 2-days Course"
              },
              "spoken_languages" : [ "French" ],
              "start" : "2012-03-22",
              "end" : "2012-03-23",
              "hours_per_day" : 8,
              "total_hours" : 16,
              "facilitators" : [ {
                "id" : 36,
                "first_name" : "François",
                "last_name" : "Beauregard",
                "photo" : null,
                "country" : "CA"
              } ],
              "city" : "Montréal",
              "country" : "CA",
              "rating" : 5.65,
              "confirmed" : false,
              "free" : true,
              "online" : false
            }]

## Event Requests [/event-requests]

### Create a New Event Request [POST]

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

## Facilitators [/facilitators]

### Facilitator [GET /facilitators/{id}]

Get a detailed info about a facilitator

+ Parameters
    + id (number) - Id of the facilitator

+ Attributes
    + id (number) - Id of the facilitator
    + first_name (string)
    + last_name (string)
    + email_address (string)
    + image (string)
    + address (object) - Home Address
        + street_1 (string)
        + street_2 (string)
        + city: "London" (string)
        + post_code (string)
        + province (string)
        + country: "GB" (string) - ISO 8166-2 2-letters country code
    + bio (string) - Biography in HTML format
    + interests (string) - Interests in HTML format
    + twitter_handle (string) - Twitter name
    + facebook_url (string) - Facebook profile
    + linkedin_url (string) - LinkedIn profile
    + google_plus_url (string) - G+ profile
    + website (string) - Website
    + blog (string) - Blog
    + active (boolean) - True if this facilitator has an active account
    + years_of_experience (number) - For how many year this facilitator holds a license of the brand
    + number_of_events (number)
    + rating (number)
    + organisations (array[Organisation])
    + endorsements (array[Endorsement])
    + materials (array[Material])
    + badges (array[Badge])
    + statistics (object) - Facilitator statistics
        + public_rating (number)
        + private_rating (number)
        + public_median (number)
        + private_median (number)
        + public_nps (number)
        + private_nps (number)
        + number_of_public_evaluations
        + number_of_private_evaluations

+ Response 200 (application/json)
    + Body

            {
                "id" : 25,
                "first_name" : "Sergey",
                "last_name" : "Kotlov",
                "email_address" : "skotlov@gmail.com",
                "image" : "https://secure.gravatar.com/avatar/996e4a0e5366080a444e43a4b446fcf6?s=300",
                "address" : {
                    "street_1" : "3 Kotelnikova alley 75",
                    "street_2" : null,
                    "city" : "Saint-Petersburg",
                    "post_code" : "197341",
                    "province" : "----",
                    "country" : "RU"
                },
                "bio" : "blabla",
                "interests" : null,
                "twitter_handle" : "skotlov",
                "facebook_url" : null,
                "linkedin_url" : null,
                "google_plus_url" : null,
                "website" : "http://changegeek.ru",
                "blog" : "http://changegeek.ru",
                "active" : true,
                "organisations" : [ {
                    "id" : "24",
                    "name" : "Happy Melly One BV",
                    "city" : "Rotterdam",
                    "country" : "NL",
                    "website" : null
                } ],
                "endorsements" : [ {
                    "content" : "I was impressed by his passion",
                    "name" : "Sergey Kotlov",
                    "company" : "Happy Melly"
                } ],
                "materials" : [ {
                    "type" : "article",
                    "link" : "http://mail.ru"
                }, {
                    "type" : "video",
                    "link" : "http://hithat.com"
                }, {
                    "type" : "casestudy",
                    "link" : "http://hithat.com"
                } ],
                "years_of_experience" -> 3,
                "number_of_events" -> 65,
                "rating": 0.0,
                "statistics": {
                  "public_rating": 4.56,
                  "private_rating": 4.56,
                  "public_median": 7,
                  "private_rating": 6,
                  "public_nps": 45.5,
                  "private_nps": 56.7,
                  "number_of_public_evaluations": 57,
                  "number_of_private_evaluations": 156
                },
                "badges" : [ {
                    "name" : "Yay4Monday",
                    "url" : "https://d2ttjh3tmuazph.cloudfront.net/badges/aae9390445ddb5e3935f8a2f0cd1eb5388f21619.jpg"
                }, {
                    "name" : "Conference Talk",
                    "url" : "https://d2ttjh3tmuazph.cloudfront.net/badges/7281fe689afb695f0c1f84baaf0af18e1460514a.jpg"
                } ]
            }

+ Response 404 (application/json)
