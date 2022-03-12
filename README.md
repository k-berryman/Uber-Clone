# Uber Clone

Followed the Uber Clone tutorial by Sonny Sanga ([click here](https://www.youtube.com/watch?v=bvn_HYpix6s&ab_channel=SonnySangha))

React Native, iOS, Android, Redux, Tailwind CSS, Google's Directions API, Google's Places API, Google's Distance Matrix API, Google Autocomplete, React Native Elements, React Native Navigation, React Native Maps

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
    </View>
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

### Build NavOptions Component
Create a folder `components` for components we'll reuse
In that, create `NavOptions.js` and add a boilerplate

```
import React from 'react'
import { StyleSheet, Text, View } from 'react-native'

const NavOptions = () => {
  return (
    <View>
      <Text></Text>
    </View>
  )
}

export default NavOptions

const styles = StyleSheet.create({})
```

Add the NavOptions component into HomeScreen component
In HomeScreen.js, `import NavOptions from '../components/NavOptions';` and `<NavOptions />`

In `NavOptions.js`, add the following
```
const data = [
  {
    id: '123',
    title: 'Get a ride',
    image: 'https://links.papareact.com/3pn',
    screen: 'MapScreen',
  },
  {
    id: '456',
    title: 'Order food',
    image: 'https://links.papareact.com/28w',
    screen: 'EatsScreen',
  },
];
```

We're going to use a `FlatList` which is one component that takes some data & renders each item. By default it's vertical, but we're going to make it horizontal.

To make an element look touchable in react native, add 'Touchable Opacity'

`resizeMode` keeps the aspect ratio

Use React Native Elements for icons
`yarn add react-native-elements`
`yarn add react-native-vector-icons`
`yarn add react-native-safe-area-context` (for icons)

Go to `App.js` and
`import { SafeAreaProvider } from 'react-native-safe-area-context';`
and then wrap our app
```
export default function App() {
  return (
    <Provider store={store}>
      <SafeAreaProvider>
        <HomeScreen />
      </SafeAreaProvider>
    </Provider>
  );
}
```

Now add in logo from react native elements
https://reactnativeelements.com/docs/icon
`import { Icon } from 'react-native-elements';` and `<Icon />`


### React Native Navigation
Create `screens/MapScreen.js` and add in a boilerplate

```
import React from 'react'
import { StyleSheet, Text, View } from 'react-native'

const MapScreen = () => {
  return (
    <View>
      <Text></Text>
    </View>
  )
}

export default MapScreen
```

Do the same for `EatsScreen`

`yarn add @react-navigation/native`
`expo install react-native-gesture-handler react-native-reanimated react-native-screens react-native-safe-area-context @react-native-community/masked-view`

In `App.js`,
`import 'react-native-gesture-handler'` (for swiping screens) and `import { NavigationContainer } from '@react-navigation/native';`

Wrap our app
```
    <Provider store={store}>
      <NavigationContainer>
        <SafeAreaProvider>
          <HomeScreen />
        </SafeAreaProvider>
      </NavigationContainer>
    </Provider>
```

`yarn add @react-navigation/stack` (stack screens up, then swipe back)

----

Make a Stack Navigator (In `App.js`)
`import { createStackNavigator } from '@react-navigation/stack';`

`const Stack = createStackNavigator();`

Wrap our app again
```
    <Provider store={store}>
      <NavigationContainer>
        <SafeAreaProvider>
          <Stack.Navigator>
            <HomeScreen />
          </Stack.Navigator>
        </SafeAreaProvider>
      </NavigationContainer>
```

Include all screens we can navigate to
```
<Stack.Navigator>
  <Stack.Screen name='HomeScreen' component={HomeScreen} />
</Stack.Navigator>
```

Instead of putting component directly

Now just make a `Stack.Screen` for MapScreen and EatsScreen too

----

Since screens are hierarchachly underneath our Navigator, we can get a navigation prop or useNavigation hook

Go to `NavOptions`
`import { useNavigation } from '@react-navigation/native';`
`const navigation = useNavigation();`

`onPress={() => navigation.navigate('')}`
Specifically plug in names passed in to Stack.Screens

Yay! And swiping works


### Google Places AutoComplete Package and Google Places API and Google Directions API and Distance-Matrix API
`yarn add react-native-google-places-autocomplete`

Go to `cloud.google.com`, go to console, make a new project, enable a billing account, go to APIs & Services -> Dashboard, click enable APIs & services

Search 'Directions API', click it, click enable
Same for 'Places API'
Same for 'Distance Matrix API'

Go to credentials tab to get a key
Click create credential, click API key, copy it & KEEP IT A SECRETTTT

---

In main dir, create `.env` and put the key in there
Add `.env` to `.ignore`

`yarn add react-native-dotenv`
Go into `babel.config.js`
and add
```
plugins: [
 [
   "module:react-native-dotenv",
    {
      moduleName: "@env",
      path: ".env",
    },
  ],
],
```

Restart server
`expo start`
if stuck, try out `expo r -c`

------

### Google Places Autocomplete
Go back to `HomeScreen.js`
`import { GooglePlacesAutoComplete } from 'react-native-google-places-autocomplete';`
`import { GOOGLE_MAPS_APIKEY } from '@env';`

```
<GooglePlacesAutocomplete
  placeholder="Where from?"
  nearbyPlaceAPI="GooglePlacesSearch" // Google Places API
  debounce={400} // after you stop typing, wait for 4 seconds, and then search
/>
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
