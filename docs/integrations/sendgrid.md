<img src="https://assets.twilio.com/public_assets/ui-assets/0.0.26/img/auth0/Sendgrid.svg" width="200" style="margin-bottom: 16px;">

# Integrating with Twilio Sendgrid

This guide outlines how you can use Quailrunner tasks to send transactional emails to your users, your employees, or anyone else you might want to email using the [Sendgrid](https://sendgrid.com/en-us) API.

### Construct your Quailrunner SQL predicate

```sql
SELECT uuid AS entity_pk, email
FROM public.users
WHERE deleted_at IS NULL;
```

Make sure to select your primary key and rename it to `entity_pk`, as well as the field on your table that contains the email address of the person you want to send the email to. You can also select any additional columns you may want to use in the body of your email.

### Construct your Quailrunner action

#### Use the Sendgrid endpoint as your webhook URL

```
https://api.sendgrid.com/v3/mail/send
```

#### Add the requisite custom headers

```
Authorization: Bearer <your-twilio-api-key>
```

#### Construct the appropriate JSON message body

For sending a plain text or html email:

```js
{
  "personalizations": [
    {
      "to": [{"email": "{{.email}}"}],
    }
  ],
  "from": {"email": "<your-email-address>"},
  "subject": "Hello from Quailrunner",
  "content": [
    {
      "type": "text/plain",
      "value": "This is a transactional email we’re sending from Quailrunner and Sendgrid!"
    },
    {
      "type": "text/html",
      "value": "<html><body>This is a transactional email we’re sending from Quailrunner and Sendgrid!</body></html>"
    }
  ]
}
```

For sending an email using a Sendgrid template:

```js
{
  "personalizations": [
    {
      "to": [{"email": "{{.email}}"}],
      "dynamic_template_data": {
        "field_1": "{{.colName1}}",
        "field_2": "{{.colName2}}"
      }
    }
  ],
  "from": {"email": "<your-email-address>"},
  "template_id": "<your-sendgrid-email-template-id>",
}
```

There are lots of additional options you can send down to Sendgrid in the request body - for a complete list, check out their [API documentation](https://www.twilio.com/docs/sendgrid/api-reference/mail-send/mail-send#request-body).
