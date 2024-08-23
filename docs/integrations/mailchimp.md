<img src="https://iad1.qualtrics.com/WRQualtricsSiteIntercept/Graphic.php?IM=IM_50ZFUIvJXfl9Iq2" width="200" style="margin-bottom: 16px;">

# Integrating with Intuit Mailchimp

This guide outlines how you can use Quailrunner tasks to send transactional emails to your users using the [Mailchimp](https://mailchimp.com/) API.

### Construct your Quailrunner SQL predicate

```sql
SELECT uuid AS entity_pk, email
FROM public.users
WHERE deleted_at IS NULL;
```

Make sure to select your primary key and rename it to `entity_pk`, as well as the field on your table that contains the email address of the person you want to send the email to. You can also select any additional columns you may want to use in the body of your email.

### Construct your Quailrunner action

#### Use the Mailchimp endpoint as your webhook URL

For sending [a plain text or plain HTML email](https://mailchimp.com/developer/transactional/api/messages/send-new-message/):

```
https://mandrillapp.com/api/1.0/messages/send
```

For sending [an email using a Mailchimp template](https://mailchimp.com/developer/transactional/api/messages/send-using-message-template/):

```
https://mandrillapp.com/api/1.0/messages/send-template
```

#### Add the requisite custom headers

```
Content-Type: application/json
```

#### Construct the appropriate JSON message body

For sending a plain text or html email:

```js
{
  "key": "<your-mailchimp-api-key>",
  "message": {
    "from_email": "<your-email-address>",
    "to": [
      {
        "email": "{{.email}}",
        "name": "{{.name}}",
      }
    ],
    "subject": "Hello from Quailrunner!",
    "text": "This is a transactional email we’re sending from Quailrunner and Mailchimp!",
    "html": "<html><body>This is a transactional email we’re sending from Quailrunner and Mailchimp!</body></html>",
  }
}
```

For sending an email using a Mailchimp template:

```js
{
  "key": "<your-mailchimp-api-key>",
  "template_name": "<your-mailchimp-template-name>",
  "template_content": [
    {
      "name": "field_1",
      "content": "{{.colName1}}"
    },
    {
      "name": "field_2",
      "content": "{{.colName2}}"
    }
  ],
  "message": {
    "from_email": "<your-email-address>",
    "to": [
      {
        "email": "{{.email}}",
        "name": "{{.name}}",
      }
    ],
  }
}
```

There are lots of additional options you can send down to Mailchimp in the request body - for a complete list, check out their [API documentation](https://mailchimp.com/developer/transactional/api/messages/).
