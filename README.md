# Type and Learn

The purpose of this project is to provide a tool for learning foreign languages. It uses the broadly known card technique, when you have a card and you can write for instance an English word on one side, and the equivalent Spanish word on the other side. Of course you can write anything on the cards.

## The components of the project

### Front end

This is an Ember CLI application.

[Github](https://github.com/tothpeter/type_and_learn_client)

### Web
[Github](https://github.com/tothpeter/type_and_learn_web)

### API
[Github](https://github.com/tothpeter/type_and_learn_api)

### Socket service

This is a rack based micro service, running on puma. Its duty is sending events to the connected browsers. The events are coming from the concurrent processes on the server.

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