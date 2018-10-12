<a name="usage"></a>
# Usage

## Building an SMTP Email
Now that you’ve sent a sent a test SMTP email with Telnet, and integrated with SendGrid, it’s time to build content.

### Getting started building

 - Limitations Customizing Your Send Scheduling Your Send Substitution
 - Tags Section Tags Suppression Groups Categories Unique Arguments
 - Getting started building SMTP works by passing a JSON string with as
 - many SMTP objects as you want to SendGrid. To do this, add the JSON
 - string to your message under a header named “X-SMTPAPI” like this:

```
{
  "to": [
    "example@example.com",
    "example@example.com"
  ],
  "sub": {
    "%name%": [
      "Ben",
      "Joe"
    ],
    "%role%": [
      "%sellerSection%",
      "%buyerSection%"
    ]
  },
  "section": {
    "%sellerSection%": "Seller information for: %name%",
    "%buyerSection%": "Buyer information for: %name%"
  },
  "category": [
    "Orders"
  ],
  "unique_args": {
    "orderNumber": "12345",
    "eventID": "6789"
  },
  "filters": {
    "footer": {
      "settings": {
        "enable": 1,
        "text/plain": "Thank you for your business"
      }
    }
  },
  "send_at": 1409348513
}

```

## Customizing your send (filters)

You can customize the emails you send via SMTP by using different settings (also referred to as filters). Change these settings in the  **X-SMTPAPI header**.

For example, to send a blind carbon copy (BCC) of your email to the address example@example.com, include the following in your X-SMTPAPI header:
```
{ "filters" : 
	{ "bcc" : 
		{ "settings" : 
			{ "enable" : 1, "email" : "example@example.com" } 					 
		} 
	} 
}
```
The X-SMTPAPI header is a JSON-encoded associative array consisting of several sections, below are examples of JSON strings using each section. Add this header to any SMTP message sent to SendGrid and the instructions in the header will be interpreted and applied to that message’s transaction. You can enable these sections with the X-SMTPAPI header:

-   [Scheduling Your Send](https://sendgrid.com/docs/API_Reference/SMTP_API/building_an_smtp_email.html#-Scheduling-your-send)
-   [Substitution Tags](https://sendgrid.com/docs/API_Reference/SMTP_API/building_an_smtp_email.html#-Substitution-tags)
-   [Section Tags](https://sendgrid.com/docs/API_Reference/SMTP_API/building_an_smtp_email.html#-Section-tags)
-   [Suppression Groups](https://sendgrid.com/docs/API_Reference/SMTP_API/building_an_smtp_email.html#-Suppression-groups)
-   [Categories](https://sendgrid.com/docs/API_Reference/SMTP_API/building_an_smtp_email.html#-Categories)
-   [Unique Arguments](https://sendgrid.com/docs/API_Reference/SMTP_API/building_an_smtp_email.html#-Unique-arguments)

### Scheduling Your Send

Schedule your email send time using the  `send_at`  parameter within your X-SMTPAPI header. Set the value of  `send_at`  to the  [UNIX timestamp](https://en.wikipedia.org/wiki/Unix_time).
```
{ "send_at": 1409348513 }
```
For more information, see our  [scheduling parameters documentation](https://sendgrid.com/docs/API_Reference/SMTP_API/scheduling_parameters.html).

### [](https://sendgrid.com/docs/API_Reference/SMTP_API/building_an_smtp_email.html#-Substitution-Tags)Substitution Tags

Substitution tags allow you to dynamically insert specific content relevant to each of your recipients, such as their first and last names.

For example, to use a substitution tag to replace the first name of your recipient, insert the tag {{name}} in the HTML of your message:
```
<html> 
	<head></head> 
	<body> 
		<p>Hello {{name}},<br> The body of your email would go here... </p> 
	</body> 
</html>
```
To define the value that will replace the {{name}} tag, define the following in your X-SMTPAPI header:
```
{ 
	"to": [ 
	"john.doeexampexample@example.com", 
	"example@example.com" ], 
	"sub": 
		{ "{{name}}": 
			[ "John", 
			"Jane" ] 
		} 
}
```
For more information, see our  [substitution tags documentation](https://sendgrid.com/docs/API_Reference/SMTP_API/substitution_tags.html).

### [](https://sendgrid.com/docs/API_Reference/SMTP_API/building_an_smtp_email.html#-Section-Tags)Section Tags

Section tags are similar to substitution tags, but rather than replace tags with content for each recipient; section tags allow you to replace a tag with more generic content— like a salutation.

For more information, see our  [section tags documentation](https://sendgrid.com/docs/API_Reference/SMTP_API/section_tags.html).

### [](https://sendgrid.com/docs/API_Reference/SMTP_API/building_an_smtp_email.html#-Suppression-Groups)Suppression Groups

You can easily specify an unsubscribe group for an email sent via SMTP by including the  `asm_group_id`  parameter in your X-SMTPAPI header. Simply set the value of  `asm_group_id`to the numerical ID of the group you would like to use.
```
{ "asm_group_id": 1 }
```
For more information, see our  [suppression groups documentation](https://sendgrid.com/docs/API_Reference/SMTP_API/suppressions.html).

### [](https://sendgrid.com/docs/API_Reference/SMTP_API/building_an_smtp_email.html#-Categories)Categories

Categories allow you to track your emails according to broad topics that you define, like “Weekly Newsletter” or “Confirmation Email”. Simply define the category by using the  `category`  parameter within your X-SMTPAPI header:

```
{ "category": "Example Category" }
```

For more information, see our  [categories documentation](https://sendgrid.com/docs/API_Reference/SMTP_API/categories.html)

Categories should only be used for broad topics. To attach unique identifiers, please use  [unique arguments](https://sendgrid.com/docs/API_Reference/SMTP_API/unique_arguments.html).

### [](https://sendgrid.com/docs/API_Reference/SMTP_API/building_an_smtp_email.html#-Unique-Arguments)Unique Arguments

Use unique arguments to track your emails based on specific identifiers unique to individual messages. Unique arguments can be retrieved via SendGrid’s  [Event Webhook](https://sendgrid.com/docs/API_Reference/Webhooks/event.html)  or your  [email activity page](https://sendgrid.com/docs/User_Guide/email_activity.html).

For more information, see our  [unique arguments documentation](https://sendgrid.com/docs/API_Reference/SMTP_API/unique_arguments.html).

## [](https://sendgrid.com/docs/API_Reference/SMTP_API/building_an_smtp_email.html#-Additional-Resources)Additional Resources

-   [Getting Started with the UI](https://sendgrid.com/docs/User_Guide/Marketing_Campaigns/getting_started.html)
-   [Getting Started with the API](https://sendgrid.com/docs/API_Reference/api_v3.html)
-   [SMTP Service Crash Course](https://sendgrid.com/blog/smtp-service-crash-course/)
-   [Getting Started with the SMTP API](https://sendgrid.com/docs/API_Reference/SMTP_API/getting_started_smtp.html)
-   [Integrating with SMTP](https://sendgrid.com/docs/API_Reference/SMTP_API/integrating_with_the_smtp_api.html)