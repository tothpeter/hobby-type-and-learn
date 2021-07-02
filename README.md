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

### Web service

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

You can create new cards from the highlighted text on a website by following these steps:

- Sign in with your Type and Learn account.
- Highlight the text.
- Right click on it.
- Choose `add to Type and Learn` from the local menu.
- Provide the remaining details.
- Hit the save button.
- [Disco!](https://www.youtube.com/watch?v=UkSPUDpe0U8)

### Production

I turned my old laptop into a Linux server to host the project.
I put an NGINX HTTP server into the front line to route the requests to the corresponding services.
I used the puma app server to run the services.
I used S3 to store the static assets.
I created a deploy script for each project to be able to deploy and rollback with the press of a button.
