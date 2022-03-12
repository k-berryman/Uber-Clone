# Uber Clone

Followed the Uber Clone tutorial by Sonny Sanga ([click here](https://www.youtube.com/watch?v=bvn_HYpix6s&ab_channel=SonnySangha))

React Native, iOS, Android, Redux, Tailwind CSS, Google's Directions API, Google's Places API, Google's Distance Matrix API, Google Autocomplete, React Native Elements, React Navigation, React Native Maps

### Set Up Expo
`sudo npm install -g expo-cli`
Close & re-open terminal

`expo init uber-clone`
Select blank template

Open up `App.js`
`expo start`
`i` to launch our iOS sim
`r` to refresh expo

### Set Up Redux Toolkit
`yarn add @reduxjs/toolkit`
`yarn add react-redux`

Set up provider, which will wrap redux around our app
In `App.js`,
`import { Provider } from 'react-redux';`
`import { store } from './store';`

```
export default function App() {
  return (
    <Provider store={store}>
      <View style={styles.container}>
        <Text>UBBBERRR</Text>
      </View>
    </Provider>
  );
}
```

We need a store to set up our global data layer
Create `store.js`, which is where our data will live
This code config and following instructions are mostly from the Redux website

We're going to have one primary layer in the data layer — navSlice, which is where the user will store their nav info such as their destination
```
import { configureStore } from '@reduxjs/toolkit';
import navReducer from './slices/navSlice';

export const store = configureStore({
  reducer: {
    nav: navReducer,
  },
});
```

Create folder `slices/` and create `navSlice.js`
We need to be able to dispatch an action into the data layer.
Our 3 dispatch funcs today will be `setOrigin`, `setDestination`, and `setTravelTimeInformation`. This all goes inside a **reducer**. The info inside the action is called the payload.

In `navSlice.js`,
```
import { createSlice } from '@reduxjs/toolkit';

const initialState = {
  origin: null, // Current location
  destination: null,
  travelTimeInformation: null
}

export const navSlice = createSlice({
  name: 'nav',
  initialState,
  reducer: {
    setOrigin: (state, action) => {
      state.origin = action.payload;
    },
    setDestination: (state, action) => {
      state.origin = action.payload;
    },
    setTravelTimeInformation: (state, action) => {
      state.origin = action.payload;
    },
  }
})
```

Now we need to expose the rest of our app to our brand new data layer. In `navSlice.js`,
`export const { setOrigin, setDestination, setTravelTimeInformation } = navSlice.actions;`

Now we know how to push data to the data layer,
but what about grabbing info out from the data layer?
Now we need Selectors.

Create one selector for each item in initial state
```
// Selectors
export const selectOrigin = (state) => state.nav.origin;
export const selectDestination = (state) => state.nav.destination;
export const selectTravelTimeInformation = (state) => state.nav.travelTimeInformation;
```

Finally, `export default navSlice.reducer;` which will then be imported in `store.js`

### Build HomeScreen Component
Create `screens` folder. In that, create `HomeScreen.js`. Add a boilerplate.

```
import React from 'react'
import { StyleSheet, Text, View } from 'react-native'

const HomeScreen = () => {
  return (
    <View>
      <Text></Text>
    <View>
  )
}

export default HomeScreen

const styles = StyleSheet.create({})
```

Go to `App.js`, `import HomeScreen from './screens/HomeScreen';` and `<HomeScreen />`

We'll use a mix of normal styles and Tailwind

### Set Up Tailwind React Native
`yarn add tailwind-react-native-classnames`

In `HomeScreen.js`,
```
import tw from 'tailwind-react-native-classnames;
<Text style={tw`text-red-500`}>im home!!!</Text>
```

### Notes
React Native uses `<View>`s instead of `<div>`s
Views get compiled into either iOS or Android components

Instead of `className=''` it's `style={}`

From metro bundler on http://localhost:19002, scan the QR code from your phone. Open with Expo Go App.

iOS requires XCode and Android requires Android Studio — or — use a real device

With our iOS Sim, we have fast refresh (because of Next.js)

In React Native, flexbox defaults to a vertical column, which is the opposite of React

We have `yarn.lock`, so let's use yarn.
If we had `package.lock`, we'd use npm.

Redux is a data layer for our app. Share data between any component. Push or pull data whenever.

Use safe area view

Use `<Image />` instead of `<img />`
