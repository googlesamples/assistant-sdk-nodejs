# Assistant SDK Sample for Node.JS

## Setup

1. Create or open a project in the [Actions Console](http://console.actions.google.com)
1. Follow the instructions to [register a device model](https://developers.google.com/assistant/sdk/guides/service/python/embed/register-device)
  1. Download `credentials.json`
1. Install the [`google-oauthlib-tool`][https://github.com/GoogleCloudPlatform/google-auth-library-python-oauthlib] in a [Python 3][https://www.python.org/downloads/] virtual environment:

```
python3 -m venv env
env/bin/python -m pip install --upgrade pip setuptools
env/bin/pip install --upgrade google-auth-oauthlib[tool]
```
1. Use the [`google-oauthlib-tool`][https://github.com/GoogleCloudPlatform/google-auth-library-python-oauthlib] to generate credentials:

```
env/bin/google-oauthlib-tool --client-secrets credentials.json \
                             --credentials devicecredentials.json \
                             --scope https://www.googleapis.com/auth/assistant-sdk-prototype \
                             --save
```

1. Run `cd google-assistant-grpc`
1. Run `npm install`
1. Run `node googleassistant.js`

```Javascript
const GoogleAssistant = require('./googleassistant');
const deviceCredentials = require('./devicecredentials.json');

const CREDENTIALS = {
  client_id: deviceCredentials.client_id,
  client_secret: deviceCredentials.client_secret,
  refresh_token: deviceCredentials.refresh_token,
  type: "authorized_user"
};

const assistant = new GoogleAssistant(CREDENTIALS);
```

## Usage

The `GoogleAssistant` class has several methods and helpers.

* `.assist(string)` - Sends a string request to the Assistant. Returns a promise with the text response or device action if applicable.
* `.setDeviceConfig(modelid, instanceid)` - Sets the device model and instance. This will need to be set with the model id you defined in the Actions Console to use device actions.
* `.startConversation([initialPrompt])` - Starts a conversation with "my test app". Returns a promise with the text response.
* `.startConversationWith(appName, [initialPrompt])` - Starts a conversation with an Assistant app. Returns a promise with the text response.
* `.endConversation()` - Ends a conversation with an Assistant app. Returns a promise.
* `.setLocale(locale)` - Changes the Assistant locale, in the format **en-US**.

```Javascript
assistant.assist('what time is it')
  .then(({ text }) => {
    console.log(text); // Will log "It's 12:30"
  });
```

You can chain together several requests and responses with Promises, as shown below.

```Javascript
assistant.assist('what is the weather')
  .then(({ text }) => {
    console.log(text);
    return assistant.assist('what about the weather tomorrow')
  })
  .then(({ text }) => {
    console.log(text);
  });
```

## License
See `LICENSE`.