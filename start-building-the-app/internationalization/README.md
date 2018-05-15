# Internationalization 🇮🇳🇺🇸🇷🇺

For application developers, internationalizing an application means abstracting all of the strings and other locale-specific things like date, currency etc.

We will create a file called `en.js` and `hi.js` containing all the strings in a flat JSON format. Our presentational components will import the strings from one of these files depending on the current language. Both the language files will contain the same keys at any point in time.

## Framework for Internationalization

We will be using [react-native-i18n](https://www.npmjs.com/package/react-native-i18n). It integrates [i18n.js](https://github.com/fnando/i18n-js) with React Native and uses the user preferred locale as default. `I18n.js` is a very famous vanilla JS library which supports features like date/time localization, number localization, locale fallback, etc. You can find more info about i18n.js [here](https://github.com/fnando/i18n-js).

Note that the only feature this module gives us is setting the device locale as default app locale. If you do not need this feature, you may skip installing the native module and use the [i18n-js](https://www.npmjs.com/package/i18n-js) module instead.

## Setting up the framework

1. Let's start by installing the npm module.

   `yarn add react-native-i18n`

2. After this is done, we would need to link it to our app using

   `react-native link react-native-i18n`

3. Create language js files containing language strings in flat JSON format. We will follow the convention of `<PAGENAME>_contentTypeInCameCase`. The reason for this convention is that it will be easier for the testers/devs to know if a translation is missing considering we will be having `guess` mode enabled \(explained later\).

   > Example: config/language/en.js

   ```javascript
    export default {
      HOME_noteTitle: 'Note Title',
      HOME_pleaseTypeYourNote: 'Please type your note below',
      HOME_startTakingNotes: 'Start taking notes',
      HOME_save: 'Save',
      HOME_characters: 'chacters',
      ABOUT_us: 'About Us',
      ABOUT_theApp: 'About the app',
      ABOUT_theCreators: 'About the Creators',
      ABOUT_theAppDesc: 'About the app',
      ABOUT_theCreatorsDesc: 'About the Creators',
    };
   ```

4. Create a utility file which will export a translate function and some other utility functions.

   _Refer to the comments to know what each line/function does_

   > utils/language.utils.js

   ```javascript
    import I18n from 'react-native-i18n'; // You can import i18n-js as well if you don't want the app to set default locale from the device locale.
    import en from '../config/language/en';
    import hi from '../config/language/hi';

    I18n.fallbacks = true; // If an English translation is not available in en.js, it will look inside hi.js
    I18n.missingBehaviour = 'guess'; // It will convert HOME_noteTitle to "HOME note title" if the value of HOME_noteTitle doesn't exist in any of the translation files.
    I18n.defaultLocale = 'en'; // If the current locale in device is not en or hi
    I18n.locale = 'en'; // If we do not want the framework to use the phone's locale by default

    I18n.translations = {
      hi,
      en
    };

    export const setLocale = (locale) => {
      I18n.locale = locale;
    };

    export const getCurrentLocale = () => I18n.locale; // It will be used to define intial language state in reducer.

    /* translateHeaderText:
     screenProps => coming from react-navigation (defined in app.container.js)
     langKey => will be passed from the routes file depending on the screen.(We will explain the usage later int the coming topics)
    */
    export const translateHeaderText = (langKey) => ({screenProps}) => {
      const title = I18n.translate(langKey, screenProps.language);
      return {title};
    };

    export default I18n.translate.bind(I18n);
   ```

5. Setup is done. Now let's import the translations from the utility file instead of hardcoding it in the components.

   Instead of

   ```javascript
    <Text style={styles.characterCount}>{text.length} characters</Text>
   ```

   we will write

   ```javascript
    <Text style={styles.characterCount}>{text.length} {translate('HOME_characters')}</Text>
   ```

After following the above 5 steps, we will have an internationalization framework setup and all our strings coming from a single language config file.

Let's change the defaultLocale of the app from `en` to `hi` in `language.utils.js` and see if our framework setup works.

![](../../.gitbook/assets/13.1.png)

TADA! Our setup works like a charm and we can see everything in `Hindi`

The code till here can be found on the **branch** [chapter/13/13.1](https://github.com/react-made-native-easy/note-taker/tree/chapter/13/13.1)

