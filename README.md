# Type and Learn

The purpose of this project is to provide a tool for learning foreign languages. It uses the broadly known flash card technique.

It used to be in production (type-and-learn.kalina.tech) when I used to have my home made laptop server in my living room.

<!--
Demo account:

- __e-mail:__ test@kalina.tech
- __password:__ 12345678 -->

## The cornerstones of the project

### Front end

[GitHub](https://github.com/tothpeter/type_and_learn_client)

This is an Ember JS application using Ember CLI.

#### Testing

I used QUnit for automated tests to cover the core functionalities and to reach a given level of confidence.

#### Deployment

With the press of a button, or better put, with an instruction in the command line.

__The automated process:__
- Build and fingerprint all assets.
- Upload the modified assets to S3.
- Insert the new content of the index.html into Redis in production.

__The manual process:__
- At this point the new version is available in production using the `version` url param: type-and-learn.kalina.tech/?version=canary
- If everything works fine, then activate the new release in the Redis store.

### The web service

[GitHub](https://github.com/tothpeter/type_and_learn_web)

This is a lightweight Sinatra app to serve the content of the index HTML to the client to boot up the app.

It serves the incoming requests like:

- Fetches the activated release index HTML from Redis.
- Serialises the logged in user (if there's any) and put it into a meta tag to speed up the front end boot.
- Sends the whole HTML back to the client.

#### Deployment

This app makes it possible to check out different releases in production before publish them.
  - Last release (Canary): type-and-learn.kalina.tech/?version=canary
  - Specific release: type-and-learn.kalina.tech/?version=release&release=RELEASE

It uses Capistrano and leverages the app server capability of phased restart to provide zero downtime deploys.

### Back end API

[GitHub](https://github.com/tothpeter/type_and_learn_api)

This is a stateless JSON API powered by Ruby on Rails to feed the client.

It uses the [JSON API specification](http://jsonapi.org/) to unify the communication with the outside world.

#### Deployment

It uses Capistrano and leverages the app server capability of phased restart to provide zero downtime deploys.

#### Testing

I used RSpec for automated tests to cover the core functionalities and to reach a given level of confidence.

### WebSocket service

[GitHub](https://github.com/tothpeter/type_and_learn_websockets)

This is a rack based micro service, running on puma. Its duty is sending events to the connected browsers. The events are coming from the concurrent processes on the server, like a background job.

A basic scenario:

- A browser connects using web socket and it subscribes to an event using message passing with a simple dialect.
- Any other process in the server can trigger an event by connecting to the service via unix socket using message passing with a simple dialect.
- When an event is triggered, the service will send it to the subscribed browsers.

#### Testing

The asynchronous nature of this service makes it a bit tricky to validate the expected behaviour, but hey, what is a hobby project for?

I used RSpec for automated tests to cover the core functionality and to reach a given level of confidence.

### Chrome Extension

[GitHub](https://github.com/tothpeter/type_and_learn_chrome_extension)

After you sign in with your Type and Learn account, you can add any text from any page to your cards by following these steps:

- Highlight the text.
- Right click on it.
- Choose `add to Type and Learn` from the local menu.
- Provide the remaining details.
- Hit the save button.
- [Disco!](https://www.youtube.com/watch?v=UkSPUDpe0U8)

### Production server

I used to run the project on my old laptop in my living room, which I built up from scratch using Ubuntu.

There was an NGINX HTTP server in the front line that routed the requests to the corresponding application server.

The API and the socket service used the puma app server, the web project was running on unicorn.
