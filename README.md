# Type and Learn

The purpose of this project is to provide a tool for learning foreign languages. It uses the broadly known card technique, when you have a card and you can write for instance an English word on one side, and the equivalent Spanish word on the other side. Of course you can write anything on the cards.

It is in [production](http://type-and-learn.kalina.tech) already (ship early, ship often). Test account: E-mail: test@kalina.tech, password: 12345678

A broader description is on its way...

## The components of the project

### Front-end

This is an Ember CLI application.

#### Deployment process:

- Build and fingerprint all assets
- Upload the modified assets to S3
- Insert the content of the index html into Redis in the production server
  - At this point we can test out the new version separately from the production here: http://type-and-learn.kalina.tech/?version=canary
- If everything works fine, then activate the new release in the Redis store

[Github](https://github.com/tothpeter/type_and_learn_client)

### Web

This is a lightweight Sinatra app, which serves the index HTML to the client.

It serves the incoming requests:

- fetches the activated release index HTML from Redis
- serialize the logged in user (if there's any) and put it into a meta tag
- sends the whole HTML back to the client

The client uses this HTML to boot up the app.

Extra features:

- It is possible to check out new or any release before publish it to the public
  - Last release (Canary): http://type-and-learn.kalina.tech/?version=canary
  - Specific release: http://type-and-learn.kalina.tech/?version=release&release=RELEASE

[Github](https://github.com/tothpeter/type_and_learn_web)

### API

This is a stateless Rails API that serves the client side Ember app.

It uses the [JSON API specification](http://jsonapi.org/) to communicate with the outside world.

[Github](https://github.com/tothpeter/type_and_learn_api)

### Socket service

This is a rack based micro service, running on puma. Its duty is sending events to the connected browsers. The events are coming from concurrent processes on the server, like the background jobs.

A basic scenario could be this:

- the browser connects using web socket and it subscribes to an event using message passing with a simple dialect
- any other process in the server can trigger an event by connecting to the service via unix socket using message passing with a simple dialect as well
- when the event is triggered, the service will send the event to the subscribed browser(s)

[Github](https://github.com/tothpeter/type_and_learn_websockets)

### Chrome Extension

After you sign in with your Type and Learn account, you can add any text from any page to your cards by following these steps:

- highlight the text
- right click on it
- choose `add to Type and Learn` from the local menu
- fill up the pop-up form
- click on the save button

[Github](https://github.com/tothpeter/type_and_learn_chrome_extension)

### Production server

Currently I run the project on my old laptop in my living room. I built up an Ubuntu server for this purpose.