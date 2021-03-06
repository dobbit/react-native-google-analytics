# react-native-google-analytics

Google Analytics for React Native!

## Getting started

1. `npm install react-native-google-analytics@latest --save`
2. [AdSupport](https://github.com/facebook/react-native/tree/master/Libraries/AdSupport)
 * You can find `RCTAdSupport.m` in `node_modules/react-native/Libraries/AdSupport`.
 * In xcode: Add `RCTAdSupport.m` in `Your Project > Libraries > React.xcodeproj > React > Base`.

## Usage

Below is an example that utilizes this library along with `react-native-ab` for A/B testing.

```javascript
'use strict';

var React = require('react-native');
var {
  AppRegistry,
  StyleSheet,
  Text,
  View,
  TouchableHighlight
} = React;
var { Experiment, Variant } = require('react-native-ab');
var {
  Analytics,
  Tracking: GATracking,
  Hits: GAHits,
  Experiment: GAExperiment
} = require('react-native-google-analytics');

var ga = this.ga = null;

var rnabtest = React.createClass({

  getInitialState() {
    return {
      experiments: {}
    };
  },

  componentWillMount() {
    GATracking.getClientId(function(err, clientId) {
      ga = new Analytics('UA-XXXXXXXX-X', clientId);
      var screenView = new GAHits.ScreenView('GA Test', '1', 'com.example.app');
      ga.send(screenView);
    });
  },

  render() {
    return (
      <View style={styles.container}>
        <TouchableHighlight onPress={this._resetExperiment}>
          <View>
            <Experiment
              ref="welcomeMessageTest"
              name="welcome-message"
              onChoice={this._onChoice}
            >
              <Variant name="standard">
                <Text style={styles.welcome}>
                  Welcome to React Native!
                </Text>
              </Variant>
              <Variant name="friendly">
                <Text style={styles.welcome}>
                  Hey there! Welcome to React Native!
                </Text>
              </Variant>
              <Variant name="western">
                <Text style={styles.welcome}>
                  Howdy, partner! This here is React Native!
                </Text>
              </Variant>
            </Experiment>
          </View>
        </TouchableHighlight>
        <TouchableHighlight onPress={this._sendEvent}>
          <Text style={styles.sendEventTest}>
            Send GA Event
          </Text>
        </TouchableHighlight>
        <Text style={styles.instructions}>
          To get started, edit index.ios.js
        </Text>
        <Text style={styles.instructions}>
          Press Cmd+R to reload,{'\n'}
          Cmd+Control+Z for dev menu
        </Text>
      </View>
    );
  },

  _onChoice(testName, variantName) {

    var experiment = new GAExperiment(testName, variantName);

    var state = this.state;
    state.experiments[testName] = experiment;
    this.setState(state);
  },

  _resetExperiment() {
    this.refs.welcomeMessageTest.reset();
  },

  _sendEvent() {
    var experiment = this.state.experiments['welcome-message'];
    var gaEvent = new GAHits.Event('Demos', 'send', 'React Native', 100, experiment);
    ga.send(gaEvent);
  }
});

var styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    backgroundColor: '#F5FCFF',
  },
  welcome: {
    fontSize: 20,
    textAlign: 'center',
    margin: 10,
  },
  instructions: {
    textAlign: 'center',
    color: '#333333',
    marginBottom: 5,
  },
  sendEventTest: {
    color: 'blue',
    fontSize: 16,
    textAlign: 'center'
  }
});

AppRegistry.registerComponent('rnabtest', () => rnabtest);

```

*TODO: App example that doesn't use react-native-ab*

## API

*Coming soon. For now, refer to the usage section.*
