# Twilio-Slack-Notifier

Twilio-Slack-Notifier is a simple app that allows you to answer phone calls in the web browser using Twilio programmable voice service and Slack notifications. It's, more or less, working in the following scenario:

1. Someone is calling you
2. Twilio-Slack-Notifier is posting a notification on your Slack channel with a link to the web browser client where you can answer the call
3. You click the link included in Slack notification
4. When you click the `call` button in the browser, another notification is posted on your Slack to let the rest of your team know that somebody has already answered the call
5. Call is performed and recorded
6. When one of the sides finishes the call, Slack is notifying about that fact and the line is ready to use again

This app may be useful when:
* you don't want to care about a physical phone in your office, just a web browser, no additional devices needed
* you have problems with clients that can't reach your number - thanks to Twilio you can pick a number from dozens of countries
* you're tired of being sticked to your chair - anyone on your Slack channel can answer the call
* you want to have a full history of your phone calls with a recorded copy of each one. If you missed something, you still have a note on Slack that somebody was trying to reach you

## How does it work?

Twilio-Slack-Notifier is an extremely lightweight app. It uses [Sinatra]((https://github.com/sinatra/sinatra)) to communicate with [Slack](https://slack.com) and [Twilio](https://twilio.com) APIs, simple web client with some JavaScript to handle the voice calls in the browser and a few webhooks to make it all running smoothly.

## Setup, requirements, dependencies
### What do you need to start using Twilio-Slack-Notifier
#### Twilio account with number

To start using Twilio-Slack-Notifier you have to sign up to [Twilio](https://www.twilio.com) and pick up a number you want to use. It's free and provides full functionality for testing purposes, there's only a short message that you're running a trial account.

#### Twilio incoming connection webhook

After registering on Twilio and picking up a number, you have to configure your first webhook. To do that, please paste the address of your Twilio-Slack-Notifier app under `Phone numbers -> Configure -> Voice -> A call comes in`


#### Twilio webhook for connection finished

In the same place you're pasting the Twilio-Slack-Notifier incoming call webhook you can type in the address of the endpoint for notifying users that the call is finished. It's under the `call status changes` input field.

#### Twilio Application

When you're done with the number, go to `Programmable voice -> Tools -> TwiML apps` and create an application. In a few seconds your Twilio app will be ready (we'll need its SID later), then you can configure a webhook there to handle the voice calls by simply typing the address of your Twilio-Slack-Notifier app into Voice's `requested URL`.

#### Slack application

When we're done with Twilio, it's time to create a simple Slack application. Go to https://api.slack.com/apps and create an app (we'll need its token as well). Remember to setup proper permissions of this app to make it able to post the messages on your channel. The only permission we need is `chat:write:bot`.

#### Twilio-Slack-Notifier setup

Clone the repo, rename `config/config.yml.sample` file to `config/config.yml`, fill it with proper data, bundle install and voila! We're done!

### Requirements

To make Twilio-Slack-Notifier running you need an environment ready to handle Rack-based application based on [Sinatra](https://github.com/sinatra/sinatra).

### Dependencies

To communicate with external services we use the following gems:
* [Twilio-ruby](https://github.com/twilio/twilio-ruby)
* [Slack-api](https://github.com/aki017/slack-ruby-gem)

### I want to test it locally...
No problem! You can clone the Twilio-Slack-Notifier repo locally and put your app out to the Internets through [ngrok.io](https://ngrok.com/). Remember to update all the webhooks addresses on Twilio and in your `config.yml` file in order to make it working.

## Configuration
You have to provide some secret data to the `config.yml` file. A sample config file should look similar to this:

``` yml
slack:
  client_token: 'xoxp-1234-your0slack-99secret4token-nomnomnom3'
  channel: '#awesome-slack-channel-name'
twilio:
  account_sid: 'ACand0some1random9chars'
  auth_token: '1234rtg42fg234rfgsbh45uknow'
  app_sid: 'APand8your9app3sid9from1twilio'
  client_name: 'your-client-name'
  caller: '+485555555'
web_client_link: 'https://my_web_client_endpoint.com/call'

```
### What is that? Where do I get all these data?

* `Slack client token` - it's a token for authorizing your Slack account when you use the application. First of all, you have to create a Slack application (just give it a name and permissions as above) and you'll get the token when you choose your app under https://api.slack.com/apps
* `Slack channel` - it's just the channel name you want the notifications to be posted, can be actually anything as long as the channel exists
* `Twilio account sid` - Get it in your Twilio dashboard after login: https://www.twilio.com/console
* `Twilio auth token` - Just like above: https://www.twilio.com/console
* `Twilio app sid` - You have to create a Twilio app first, go to Console -> Programmable Voice -> Tools -> TwiML apps. After creating a new app you'll get the Application SID there.
* `Twilio client name` - it's a name you have to type in your Twilio dashboard and Twilio-Slack-Notifier config. It authorizes the browser to connect to your Twilio and perform the phone calls
* `Caller` - your Twilio phone number
* `Web client link` - the endpoint address for your web client - this link is posted on the Slack channel.

## Contributing

If you'd like to contribute to Twilio-Slack-Notifier, please feel free to do so! I'll be happy to help in case of any questions. For more information please check the [contributing guidelines](CONTRIBUTING.md)

## License

This app is available as open source under the terms of the [MIT License](https://opensource.org/licenses/MIT).
