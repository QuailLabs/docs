<img src="https://pages.twilio.com/rs/294-TKB-300/images/twilio-logo-red_2.png" alt="Twilio logo" width="200" style="margin-bottom: 16px;">

# Integrating with Twilio

This guide outlines how you can use Quailrunner tasks to send SMS or MMS messages to your users, your employees, or anyone else you might want to message using the [Twilio](https://www.twilio.com) API.

### Construct your Quailrunner SQL predicate

```sql
SELECT uuid AS entity_pk, phone_number
FROM public.users
WHERE deleted_at IS NULL;
```

Make sure to select your primary key and rename it to `entity_pk`, as well as the field on your table that contains the phone number of the person you want to send the message to. You can also select any additional columns you may want to use in the body of your message.

### Construct your Quailrunner action

#### Use the Twilio endpoint as your webhook URL

For sending SMS or MMS messages:

```
https://api.twilio.com/2010-04-01/Accounts/{AccountSid}/Messages.json
```

Replace `{AccountSid}` with your actual Twilio Account SID.

#### Generating Base64 Encoded Credentials

To generate your Base64 encoded credentials, follow these steps:

1. Combine your Twilio Account SID and Auth Token in the format `AccountSid:AuthToken`.
2. Encode this string using a terminal command.
   ```
   echo -n 'YourAccountSid:YourAuthToken' | base64
   ```

#### Add the requisite custom headers

```
Content-Type: application/x-www-form-urlencoded
Authorization: Basic {Base64EncodedCredentials}
```

Replace `{Base64EncodedCredentials}` with a Base64-encoded string of your Twilio Account SID and Auth Token in the format `AccountSid:AuthToken`.

#### Construct the appropriate form-urlencoded message body

For sending an SMS message:

```
From=<your-Twilio-phone-number>&To={{.phone_number}}&Body=This is a test message from Quailrunner and Twilio!
```

For sending an MMS message with media:

```
From=<your-Twilio-phone-number>&To={{.phone_number}}&Body=Check out this image!&MediaUrl=https://example.com/image.jpg
```

Note: This is not a JSON request body so Quailrunner’s request body validation tool will not think it is valid.

### Additional Configuration Options

- **StatusCallback**: You can add a `StatusCallback` parameter to receive webhooks about the message status.
- **MessagingServiceSid**: If you're using a Messaging Service, you can replace the `From` parameter with `MessagingServiceSid`.
- **MaxPrice**: Set a maximum price in USD that you are willing to pay for the message.
- **ValidityPeriod**: Set the length of time in seconds that the message can remain in a queue.

For a full list of available parameters, please refer to the [Twilio API documentation](https://www.twilio.com/docs/messaging/api/message-resource#create-a-message-resource).
