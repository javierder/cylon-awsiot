# Cylon.js For AWS IoT

Cylon.js (http://cylonjs.com) is a JavaScript framework for robotics, physical computing, and the Internet of Things (IoT).

This repository contains the Cylon.js adaptor/driver for the AWS IoT libraries. It uses the aws-iot-device-sdk, provided by amazon.

This is forked from the cylon-mqtt adaptor/driver, very basic fork.

## How to Install

Install `cylon-awsiot` through NPM:

    $ npm install cylon cylon-awsiot

Before using `cylon-awsiot`, you'll need to setup your AWS IoT connection, and download the certificates and keys.

## How to Use

There's two different ways to use the `cylon-mqtt` module.

You can use the connection object only, in which case you have pub/sub access to all available MQTT channels:

```javascript
Cylon.robot({
  connections: {
    server: { adaptor: 'awsiot', 
              host: '_amazon_provided_hsot',
              port : 8883,
              thingName : "Thing Name",
              clientId : "Client ID 00",
              caCert : "root-CA.crt",
              clientCert : "certificate.pem.crt",
              privateKey : "private.pem.key"
           }
  },

  work: function(my) {
    my.server.subscribe('hello');

    my.server.on('message', function (topic, data) {
      console.log(topic + ": " + data);
    });

    every((1).seconds(), function() {
      my.server.publish('hello', 'hi there');
    });
  }
});
```

Or, you can use the device object, which restricts your interaction to a single MQTT channel.
This can make it easier to keep track of different channels.

```javascript
Cylon.robot({
  connections: {
    //sames as above
  },

  devices: {
    hello: { driver: 'awsiot', topic: 'hello' }
  },

  work: function(my) {
    my.hello.on('message', function (data) {
      console.log("hello: " + data);
    });

    every((1).seconds(), function() {
      my.hello.publish('hi there');
    });
  }
})
```

## Documentation
This is just the initial fork.

Thank you!

## Contributing

For our contribution guidelines, please go to [https://github.com/hybridgroup/cylon/blob/master/CONTRIBUTING.md
](https://github.com/hybridgroup/cylon/blob/master/CONTRIBUTING.md
).

## Release History

For the release history, please go to [https://github.com/hybridgroup/cylon-mqtt/blob/master/RELEASES.md
](https://github.com/hybridgroup/cylon-mqtt/blob/master/RELEASES.md
).

## License

Copyright (c) 2014-2016 The Hybrid Group. Licensed under the Apache 2.0 license.
