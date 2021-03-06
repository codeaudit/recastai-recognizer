recastai-recognizer
===================

An MS Bot Framework `IIntentRecognizer` implementation for [Recast.ai](https://recast.ai)

## Prereqs
* [Recast.ai node.js client, at least version 2](https://www.npmjs.com/package/recastai)
* [MS Bot Framework node.js SDK](https://www.npmjs.com/package/botbuilder) - though not a technical dependency of this module.

## Installation
```bash
npm install recastai-recognizer
```

## usage
The recognizer is used in basically the same way as the 
[LUIS recognizer plugin](https://docs.botframework.com/en-us/node/builder/guides/understanding-natural-language) 
packaged with the MS Bot Framework SDK. 
It uses the [Recast.ai node.js client module](https://www.npmjs.com/package/recastai) 
to communicate with your Recast.ai bot:
```javascript
/*
 * The Recast.ai-specific bit
 */
var rairec = require('recastai-recognizer')
var recognizer = new rairec.RecastRecognizer(YOUR_TOKEN, YOUR_LANGUAGE)

/*
 * Standard MS Bot Framework stuff
 */
var builder = require('botbuilder')
var dialog = new builder.IntentDialog({ recognizers: [recognizer] });
var bot = new builder.UniversalBot(MS_BOT_FMWK_CONNECTOR)
bot.dialog('/', intents)

/*
 * Matching against one Recast.ai intent
 */
dialog.matches('YOUR_INTENT', function(session, args) {
    ...
}

/*
 * Matching against one of a number of Recast.ai intents
 */
dialog.matchesAny(['YOUR_INTENT_1', 'YOUR_INTENT_2' ...], function(session, args) {
    ...
}
...
```

* `YOUR_TOKEN` should be the client token string from your Recast.aia bot
* `YOUR_LANGUAGE` should be the language in which the utterenaces are made (e.g. `"en"` or `"fr"`)

This allows the Recast.ai API to be used with the `botbuilder` intent 
matching facilities, making dialog and bot development with Recast.ai much 
more intuitive and closer to the recommended style of the `botbuilder` 
framework.

## Notes

* Entity information from Recast.ai far exceeds that defined for the `IEntity` interface.
  Consequently, for each returned entity, the `entity` (string) property of the `IEntity` structure is set to the `raw` property of the Recast entity object,
  while an additional property, `details`, is set to the full Recast entity object.
* The full Recast response structure can be found in the `recastResult` property of the returned `IIntentRecognizerResult` object.

## Contributing

In lieu of a formal styleguide, take care to maintain the existing coding style. That said, my own style is rather old-school, so any improvements that retain compatibility with the minimum platform requirements of the Recast.ai framework are more than welcome.

Add unit tests for any new or changed functionality. Lint and test your code - some of which I've been particularly careful to observe in these early releases! 

## Release History

* 0.0.4 Breaking change! "description" in IEntity object changed to "details"
* 0.0.3 Better Recast Entity support, plus tests!
* 0.0.2 Documentation fixes!
* 0.0.1 Initial release
