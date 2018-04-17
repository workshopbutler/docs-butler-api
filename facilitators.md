## Facilitators [/facilitators{?fields}]

### Facilitator List [GET]

Returns the list of all trainers for the account. The trainers are returned sorted by name.

The content of the list depends on the type of account:

- for knowledge brands, it contains all active trainers/facilitators
- for training companies, it contains all active trainers, both employees and external trainers
- for trainers, it contains the only record of the trainer himself

+ Parameters
    + fields: "first_name,email_address" (string, optional) - The output contains only the defined fields

+ Attributes (array[Facilitator])

+ Response 200 (application/json)
    + Body

            [ {
              "id" : 12,
              "first_name" : "Nick",
              "last_name" : "Doom",
              "photo" : null,
              "country" : "GB",
              "languages" : [ "English" ],
              "countries" : [ "GB" ],
              "rating" : 6.416666984558105,
              "badges" : [ {
                "name" : "Certificate of Attendance",
                "url" : "https://d2ttjh3tmuazph.cloudfront.net/badges/3761a40b388780d62d73abb88896cef671c981c1.jpg"
              }, {
                "name" : "Certificate of Knowledge",
                "url" : "https://d2ttjh3tmuazph.cloudfront.net/badges/e473d0a30a730c33a2f41a47d178c57c9963e8b5.jpg"
              }, {
                "name" : "Change For Happiness",
                "url" : "https://d2ttjh3tmuazph.cloudfront.net/badges/ca751785ba194f43d7b19afa8226008e332f1976.jpg"
              } ]
            }, {
              "id" : 14,
              "first_name" : "Alejandra",
              "last_name" : "Dojo",
              "photo" : null,
              "country" : "ES",
              "languages" : [ "Spanish" ],
              "countries" : [ "ES", "CO" ],
              "rating" : 8.321,
              "badges" : [ ]
            }]

### Get Facilitator [GET /facilitators/{id}]

Get a detailed info about facilitator

+ Parameters
    + id (number) - Id of the facilitator

+ Attributes (Facilitator)

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
