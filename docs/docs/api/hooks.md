---
sidebar_position: 3
---

# Hooks

React v16.8 introduced [hooks](https://reactjs.org/docs/hooks-intro.html). If you are using a version of React Native that is before [v0.59.0](https://facebook.github.io/react-native/blog/2019/03/12/releasing-react-native-059), your React Native version does not support hooks.

## `useTrackPlayerEvents`

Register an event listener for one or more of the [events](./events.md) emitted by the TrackPlayer. The subscription is removed when the component unmounts.

Check out the [events section](./events.md) for a full list of supported events.

```jsx
import React, { useState } from 'react';
import { Text, View } from 'react-native';
import { useTrackPlayerEvents, TrackPlayerEvents, STATE_PLAYING } from 'react-native-track-player';

// Subscribing to the following events inside MyComponent
const events = [
  TrackPlayerEvents.PLAYBACK_STATE,
  TrackPlayerEvents.PLAYBACK_ERROR
];

const MyComponent = () => {
  const [playerState, setPlayerState] = useState(null)

  useTrackPlayerEvents(events, (event) => {
    if (event.type === TrackPlayerEvents.PLAYBACK_ERROR) {
      console.warn('An error occured while playing the current track.');
    }
    if (event.type === TrackPlayerEvents.PLAYBACK_STATE) {
      setPlayerState(event.state);
    }
  });

  const isPlaying = playerState === STATE_PLAYING;

  return (
    <View>
      <Text>The TrackPlayer is {isPlaying ? 'playing' : 'not playing'}</Text>
    </View>
  );
};
```

## `useProgress`

| State            | Type     | Description                      |
| ---------------- | -------- | -------------------------------- |
| position         | `number` | The current position in seconds  |
| buffered         | `number` | The buffered position in seconds |
| duration         | `number` | The duration in seconds          |

`useProgress` accepts an interval to set the rate (in miliseconds) to poll the track player's progress. The default value is `1000` or every second.

```jsx
import React from 'react';
import { Text, View } from 'react-native';
import { useProgress } from 'react-native-track-player';

const MyComponent = () => {
  const { position, buffered, duration } = useProgress()

  return (
    <View>
      <Text>Track progress: {position} seconds out of {duration} total</Text>
      <Text>Buffered progress: {buffered} seconds buffered out of {duration} total</Text>
    </View>
  )
}
```
