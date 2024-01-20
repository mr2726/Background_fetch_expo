# Background Fetch with Expo
> First, you need to install two libraries: `expo-background-fetch` and `expo-task-manager`. Then import them into your project:
### Install
```bash
npx expo install expo-background-fetch
npx expo install expo-task-manager
```
### Import

```javascript
import * as BackgroundFetch from 'expo-background-fetch';
import * as TaskManager from 'expo-task-manager';
```
>Next step, we need to set the task name outside the main component:
```javascript
const BACKGROUND_FETCH_TASK = 'background-fetch';
```

> Now, we need to register the background task outside your App component:
```javascript
async function registerBackgroundFetchAsync() {
  console.log('Register');
  return BackgroundFetch.registerTaskAsync(BACKGROUND_FETCH_TASK, {
    minimumInterval: 1,
  });
}
```
> Create the `TaskManager`:
```javascript
TaskManager.defineTask(BACKGROUND_FETCH_TASK, async () => {
  const now = Date.now();

  console.log(`Got background fetch call at date: ${new Date(now).toISOString()}`);

  return BackgroundFetch.BackgroundFetchResult.NewData;
});
```
## Full Code
```javascript
import React from 'react';
import { StyleSheet, Text, View, Button } from 'react-native';
import * as BackgroundFetch from 'expo-background-fetch';
import * as TaskManager from 'expo-task-manager';

const BACKGROUND_FETCH_TASK = 'background-fetch';

TaskManager.defineTask(BACKGROUND_FETCH_TASK, async () => {
  const now = Date.now();

  console.log(`Got background fetch call at date: ${new Date(now).toISOString()}`);

  return BackgroundFetch.BackgroundFetchResult.NewData;
});

async function registerBackgroundFetchAsync() {
  console.log('Register');
  return BackgroundFetch.registerTaskAsync(BACKGROUND_FETCH_TASK, {
    minimumInterval: 1,
  });
}

export default function BackgroundFetchScreen() {

  React.useEffect(() => {
    registerBackgroundFetchAsync();
  }, []);


  return (
    <View style={styles.screen}>
      <Text>Hello World!</Text>
    </View>
  );
}

const styles = StyleSheet.create({
  screen: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
  }
});

```