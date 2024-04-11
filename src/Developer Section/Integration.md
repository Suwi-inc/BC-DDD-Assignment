# Integration Guide 
## Google Calendar API
The integration with the Google Calendar API enables LeadsCalendar to fetch and display users' existing events, as well as create new events seamlessly within the application.
To set up the evironement to use the Google calender API you need to enable the API , Configure the OAuth consent screen and Authorize credentials for a web application [Here] (https://developers.google.com/calendar/api/quickstart/js) 
LeadsCalendar uses the following parts of the Google calender API
 - Event - An event on a calendar containing information such as the title, start and end times, and attendees
  The following snippet demonstartes how an event is created in leadsCalender
```
var event = {
  'summary': 'Google I/O 2015',
  'location': '800 Howard St., San Francisco, CA 94103',
  'description': 'A chance to hear more about Google\'s developer products.',
  'start': {
    'dateTime': '2015-05-28T09:00:00-07:00',
    'timeZone': 'America/Los_Angeles'
  },
  'end': {
    'dateTime': '2015-05-28T17:00:00-07:00',
    'timeZone': 'America/Los_Angeles'
  },
  'recurrence': [
    'RRULE:FREQ=DAILY;COUNT=2'
  ],
  'attendees': [
    {'email': 'lpage@example.com'},
    {'email': 'sbrin@example.com'}
  ],
  'reminders': {
    'useDefault': false,
    'overrides': [
      {'method': 'email', 'minutes': 24 * 60},
      {'method': 'popup', 'minutes': 10}
    ]
  }
};

var request = gapi.client.calendar.events.insert({
  'calendarId': 'primary',
  'resource': event
});

request.execute(function(event) {
  appendPre('Event created: ' + event.htmlLink);
});
```
Detailed documentation for Google calender API Event can be found [here](https://developers.google.com/calendar/api/v3/reference/events)
 - Calender - A collection of events. Each calendar has associated metadata, such as calendar description or default calendar time zone
## PayPal REST API
LeadsCalendar utilizes the PayPal REST API for payment processing. This integration facilitates secure transactions, allowing users to make payments using their PayPal accounts. 
To set up the environment to work with Paypal you need to Get client ID and client secret and get the access token from [Here](https://developer.paypal.com/api/rest/)
- Code Snippet of how LeadsCalendar send an invoice to PayPal
The Inovice is sent by 
```
var fetch = require('node-fetch');

fetch('https://api-m.sandbox.paypal.com/v2/invoicing/invoices/INV2-EHNV-LJ5S-A7DZ-V6NJ/send', {
    method: 'POST',
    headers: {
        'Authorization': 'Bearer zekwhYgsYYI0zDg0p_Nf5v78VelCfYR0',
        'Content-Type': 'application/json',
        'PayPal-Request-Id': 'b1d1f06c7246c'
    },
    body: JSON.stringify({ "send_to_invoicer": true })
});
```
The invoice is sent and created if all the paramters are correct
Detailed documentation for integrating with the PayPal REST API can be found [here](https://developer.paypal.com/api/rest/).

## Binance Pay API
The Binance Pay API integration enables LeadsCalendar to offer cryptocurrency payment options to users. By integrating with Binance Pay, users can make payments using various cryptocurrencies supported by the platform. 
The following snippet shows how to create a payment reqquest
- Creating a Request Parameter
```
// Crypto payments(C2B) parameters
public struct OrderInitParameters {
    let merchantId: String
    let prepayId: String
    let timestamp: Int64
    let noncestr: String
    let certSn: String
    let sign: String
    let redirectScheme: String
}

// Transfer or send cryptos(C2C) parameters
public struct C2CInitParameters {
    let id: String
    let type: String
    let redirectScheme: String
}
```
- Calling the API to make the payment
```
BinancePay.shared.pay(with: parameters, containerView: self.view) { (result) in
   switch result {
   case .success:
    print("success")
   case .failure(let error):
    print("failure \(error)")
   }
}
```


Detailed documentation for integrating with the Binance Pay API can be found [Here](https://developers.binance.com/docs/binance-pay/app-integration).
