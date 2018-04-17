## Events List [/events]

### Get Events [GET /events{?future}{&public}{&archived}{&countryCode}{&eventType}{&trainerId}{&widgetId}{&confirmed}{&fields}]

Get a list of events

+ Parameters
    + future: true (boolean, optional) - If true, only future events in the list. If false, only past events in the list.
    + public: false (boolean, optional) - If true, only public events in the list. If false, only private events int he list.
    + archived: true (boolean, optional) - If true, only archived events in the list. If false, only active events in the list
    + countryCode: "DE" (string, optional) - ISO 8166-2 country code. If set, only events from this country in the list.
    + eventType: 1 (number, optional) - Id of event type. If set, only events with this type in the list.
    + trainerId: 5 (number, optional) - If set, only the events ran by this trainer are returned
    + confirmed: false (boolean, optional) - If set, events are filtered by their confirmation state
    + widgetId: "xjl34" (string, optional) - DEPRECATED
    + fields: "title,city" (string, optional) - The output contains only the defined fields

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
                "country" : "CH",
                "rating": null
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
                "country" : "CA",
                "rating": null
              } ],
              "city" : "Montréal",
              "country" : "CA",
              "rating" : 5.65,
              "confirmed" : false,
              "free" : true,
              "online" : false
            }]

## Event [/events]

### Get Event [GET /events/{id}?{fields}]

Get a detailed info about event

+ Parameters
    + id (number|string) - It could be either a numerical or string (hashed) event id. The latter is preferable.
    + fields (string, optional) - Comma-separated list of event attributes to return. 'trainer.rating' adds a public trainer ratings for each trainer (brands only)

+ Attributes
    + id (number) - Numerical id of the event
    + hashed_id (string) - String id of the event
    + title (string)
    + spoken_languages: ["English", "German"] (array) - Languages of the event in English
    + materials_language: "Russian" (string, optional)
    + description (string, optional) - Can be in HTML format
    + additional_info (string, optional) - DEPRECATED. Will be removed in the next version without further notice.
    + start: "2017-05-19" (string) - Start date of the event
    + end: "2017-06-20" (string) - End date of the event
    + hours_per_day: 5 (number) - If this value equals 0, then a user didn't set a number of hours per day`
    + total_hours: 16 (number) - Total length of the event. If this value equals 0, then a user didn't set a total number of hours for the event
    + city: "London" (string)
    + country: "GB" (string) - ISO 8166-2 2-letters country code
    + website (string, optional)
    + registration_page (object, optional) - The url to a registration form
        + url (string) - The full url to a registration form
        + custom (boolean) - True if a trainer uses a third-party website to accept registration.
    + type (object) - Type of the event
        + id (number) - Id of the type
        + name (string) - Name of the type
        + url (string) - A related badge
    + facilitators (array[Person]) - Trainers who run the event
    + rating (number) - Rating of the event based on its evaluations
    + confirmed (boolean) - True if the event is confirmed
    + free (boolean) - True if the event is free
    + online (boolean) - True if the event is online
    + private (boolean, optional) - True if the event is private
    + state (string, optional) - "Canceled", "Archived" or "Active"
    + instructions (string, optional) - Text instructions for visitors on how to fill-in a registration form. They should be rendered before a registration form.
    + sold_out (boolean) - True if there is no more available seats for the event
    + free_ticket_type (object, optional) - If the event is free, you should use this parameter
        + unlimited (boolean) - True if there is no limitation for a number of seats
        + amount (number) - Number of seats for the event. Don't use this parameter, if 'unlimited' is true.
        + left (number) - Number of seats left. Don't use this parameter, if 'unlimited' is true.
        + start (string) - The day when a registration process starts. Before it, visitors cannot register for the event.
        + end (string) - The day when a registration process ends. After it, visitors cannot register for the event.
        + state (object)
            + sold_out (boolean) - True if there is no more available free tickets
    + paid_ticket_types (array[PaidTicketType]) - If the event is not free, you must use this parameter to render paid tickets



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
                    "country" : "ES",
                    "rating": 9.55
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
