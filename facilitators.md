## Facilitators [/facilitators]

### Facilitator List [GET]

Get a list of facilitators/trainers

+ Attributes (array[Facilitator])

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
