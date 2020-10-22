# Zent Cash Mobile - A mobile, native Zent Cash wallet

### Initial Setup

Install Yarn

```bash
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
sudo apt update && sudo apt install yarn
```

* `cd Zent-Mobile-Wallet`
* `yarn install`

### Borrar carpeta .git 
eliminar la carpeta .git --> Zent-Mobile-Wallet\node_modules\react-native-push-notification\.git

* `npm i jetifier`
* `npx jetify`
* Crear fichero sentry.properties dentro de la carpeta **Android**

```text
defaults.project=zentcash
defaults.org=zentcash
auth.token=<TOKEN>
```

### Install Android SDK
```bash
sudo snap install androidsdk
androidsdk "platform-tools" "platforms;android-28"
export ANDROID_SDK_ROOT=/root/snap/androidsdk/30/AndroidSDK
```

### Running

* `node --max-old-space-size=8192 node_modules/react-native/local-cli/cli.js start` (Just need to run this once to start the server, leave it running)
* `react-native run-android`

### Logging

`react-native log-android`

### Creating a release

You need to bump the version number in:

* `src/Config.js` - `appVersion`
* `android/app/build.gradle` - `versionCode` and `versionName`
* `package.json` - `version` - Not strictly required

Then
`cd android`
`chmod +x gradlew`
`./gradlew bundleRelease`
Optionally
`./gradlew installRelease`

or `yarn deploy-android`

### Integrating QR Codes or URIs

Zent Cash Mobile supports two kinds of QR codes.

* Standard addresses / integrated addresses - This is simply the address encoded as a QR code.

* zentcash:// URI encoded as a QR code.

Your uri must being with `zentcash://` followed by the address to send to, for example, `zentcash://Ze4qSTYgPABXiyh3Qak2gLRUXXCZzNaCC7qDDCdBqpLVjVduz6e6SFC16Eh9bd1cju9JDPhBDFRGz8HaAym5qvYV2fvQkkxtW`

There are a few optional parameters.

* `name` - This is used to add you to the users address book, and identify you on the 'Confirm' screen. A name can contain spaces, and should be URI encoded.
* `amount` - This is the amount to send you. This should be specified in atomic units.
* `paymentid` - If not using integrated address, you can specify a payment ID. Specifying an integrated address and a payment ID is illegal.

An example of a URI containing all of the above parameters:

```
zentcash://Ze4qSTYgPABXiyh3Qak2gLRUXXCZzNaCC7qDDCdBqpLVjVduz6e6SFC16Eh9bd1cju9JDPhBDFRGz8HaAym5qvYV2fvQkkxtW?amount=10000&name=Starbucks%20Coffee&paymentid=e1b0bb215fb4b7af6f41f0a325b59252f2836ec831c528d75703ffbf8966fe0d
```

This would send `100 ZTC` (10000 in atomic units) to the address `Ze4qSTYgPABXiyh3Qak2gLRUXXCZzNaCC7qDDCdBqpLVjVduz6e6SFC16Eh9bd1cju9JDPhBDFRGz8HaAym5qvYV2fvQkkxtW`, using the name `Spanish Coffee` (Note the URI encoding), and using a payment ID of `e1b0bb215fb4b7af6f41f0a325b59252f2836ec831c528d75703ffbf8966fe0d`

You can also just display the URI as a hyperlink. If a user clicks the link, it will open the app, and jump to the confirm screen, just as a QR code would function. (Provided all the fields are given)
