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

## EvaluationEvent (object)
+ id: 123 (number) - The ID of the event
+ title (string) - The name of the event

## BriefAttendee (object)
+ id (number) - The ID of the attendee
+ first_name (string)
+ last_name (string)
+ email (string)

## FieldOption (object)
+ value: de (string) - Option's value
+ label: Germany (string) - Label

## Field (object)
+ name: first_name (string) - Name of the field, its unique identifier
+ type: text, checkbox, select, date, textarea, email, country, ticket (enum) - Type of the field
+ required (boolean) - True if the field is required
+ label: First Name (string) - Label, shown to visitors
+ options (array[FieldOption], optional) - Available options for `select` type

## QuestionResponse (object)
+ question (string) - Content of the question
+ answer (string) - Response to the question

## Evaluation (object)
+ event (EvaluationEvent)
+ attendee (BriefAttendee)
+ id (number) - The ID of the evaluation
+ question_1 (QuestionResponse)
+ question_2 (QuestionResponse)
+ question_3 (QuestionResponse)
+ question_4 (QuestionResponse)
+ question_5 (QuestionResponse)
+ question_6 (QuestionResponse)
+ question_7 (QuestionResponse)
+ question_8 (QuestionResponse)
+ question_9 (QuestionResponse)
+ question_10 (QuestionResponse)

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

## PaidTicketType (object)
+ id (string) - Unique ticket type ID. Must be passed during the registration of attendee
+ name (string) - Name of the ticket type. Ex: Regular, Early Bird
+ amount (number) - Total number of tickets of this type
+ left (number) - Number of available tickets of this type
+ start (string) - The first day when visitors can buy the tickets of this type
+ end (string) - The last day when visitors can buy the tickets of this type
+ with_vat (boolean) - True if the price doesn't include VAT and "+ VAT" must be shown alongside the price
+ price (object)
    + amount (number) - Ticket price
    + currency (string) - Ticket currency
    + sign (string) - Currency sign
+ state (object)
    + sold_out - True if there is no more tickets of this type available
    + ended - True if current date is more than 'end' attribute of this type
    + started - True if current date is more or equal than 'start' attribute of this type
    + in_future - True if current date is less than 'start' attribute of this type
    + valid - True visitors can buy the tickets of this type

## Person (object)

+ id (number) - Id of the person
+ first_name (string)
+ last_name (string)
+ photo (string) - Url of the person's photo
+ country (string) - ISO 8166-2 2-letters country code
+ rating (number) - Trainer's rating. May be 'null'

## Facilitator (object)
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
