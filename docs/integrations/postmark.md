<img src="https://assets.postmarkapp.com/packs/images/application/logo-84e5b009.svg" width="200" style="margin-bottom: 16px;">

# Integrating with Postmark

This guide outlines how you can use Quailrunner tasks to send transactional emails to your users, your employees, or anyone else you might want to email using the [Postmark](https://www.postmarkapp.com) API.

### Construct your Quailrunner SQL predicate

```sql
SELECT uuid AS entity_pk, email
FROM public.users
WHERE deleted_at IS NULL;
```

Make sure to select your primary key and rename it to `entity_pk`, as well as the field on your table that contains the email address of the person you want to send the email to. You can also select any additional columns you may want to use in the body of your email.

### Construct your Quailrunner action

#### Use the Postmark endpoint as your webhook URL

For sending [a plain text or plain HTML email](https://postmarkapp.com/developer/api/email-api#send-a-single-email):

```
https://api.postmarkapp.com/email
```

For [sending an email using a Postmark template](https://postmarkapp.com/developer/api/templates-api#email-with-template):

```
https://api.postmarkapp.com/email
```

#### Add the requisite custom headers

```
Accept: application/json
Content-Type: application/json
X-Postmark-Server-Token: <your-postmark-server-token>
```

#### Construct the appropriate JSON message body

For sending [a plain text or plain HTML email](https://postmarkapp.com/developer/api/email-api#send-a-single-email):

```js
{
  "From": "<your-email-address>",
  "To": "{{.email}}",
  "Subject": "Hello from Quailrunner!",
  "TextBody": "This is a transactional email we’re sending from Quailrunner and Postmark!",
  "HtmlBody": "<html><body>This is a transactional email we’re sending from Quailrunner and Postmark!</body></html>",
  "MessageStream": "outbound"
}
```

For [sending an email using a Postmark template](https://postmarkapp.com/developer/api/templates-api#email-with-template):

```js
{
  "From": "<your-email-address>",
  "To": "{{.email}}",
  "Subject": "Hello from Quailrunner!",
  "TemplateId": <your-numerical-postmark-template-id>, // no quotes, since this is a number
  "TemplateModel": {
     // replace using the appropriate names for your Postmark template model and
     // selected columns from your SQL predicate
    "field_1": "{{.colName1}}",
    "field_2": "{{.colName2}}"
  }
}
```
