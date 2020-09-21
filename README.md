# Exploring Appflow Exercise 3

This part -3 of the exploring Appflow exercise consists of building a development build for iOS

1. Building for native (advanced)
    1. generating signing credentials and a provisioning profile
    2. Create a security profile in Appflow
    
    
Development type iOS builds require [generating signing credentials and a provisioning profile. ](https://ionicframework.com/docs/appflow/package/credentials#android-credentials)Code Signing with a development type builds allows your app to run on device through Xcode. <br> You can create a secuirty profile using:
* iOS development provisioning profile 
* iOS development signing certificate.

Once you have these you can create a security profile in Appflow Dashboard.

### generating signing credentials and a provisioning profile

## iOS Credentials
You'll need an Apple Developer account (Individual or Organization). See
[comparing
memberships](https://developer.apple.com/support/compare-memberships/).

### Registering your App ID

Every app must register an ID with Apple.

* Navigate to [Certificates, Identifiers & Profiles › Identifiers › App IDs](https://developer.apple.com/account/ios/identifier/bundle) in the [Apple Developer Center](https://developer.apple.com/account) and register a new App ID.

![iOS App id](/docs/assets/img/appflow/ss-profiles-ios-app-id.png)

* Under Explicit App ID, set the Bundle ID to the ID you've specified in your app's `config.xml` file. iOS Bundle IDs are represented as a reversed address, such as `com.ionicframework.MyIonicApp`.

### Device Registration

Devices must be explicitly registered with Apple for sending push notifications during development.

* Navigate to [Certificates, Identifiers & Profiles › Devices › All](https://developer.apple.com/account/ios/device/) in the [Apple Developer Center](https://developer.apple.com/account) and register a new Device.

![iOS Register Device](/docs/assets/img/appflow/ss-profiles-ios-device-register-1.png)

* Give your device a name and enter the device's UDID. [Find your UDID](ios-udid).

### Certificate Signing Request

Before you can get a certificate from Apple, you'll need to generate a certificate signing request file. It can be created on Mac OSX by using Keychain Access and on other platforms by using OpenSSL.

#### Using Keychain Access

* Navigate to Keychain Access › Certificate Assistant › Request a Certificate From a Certificate Authority on your Mac.

![iOS CSR](/docs/assets/img/appflow/ss-profiles-ios-csr.png)

* Enter your name and email address. Leave the CA Email blank.
* Select Saved to disk and hit continue. This will generate your `.certSigningRequest` file.

#### Using OpenSSL

* Generate a private RSA key file.

```bash
$ openssl genrsa -out keyname.key 2048
```

* Create the certificate signing request file by filling out the interactive form.

```bash
$ openssl req -new -key keyname.key -out CertificateSigningRequest.certSigningRequest
```

### iOS App Certificate & Provisioning Profile

Before you can generate App Certificates & Provisioning Profiles, you'll need to [register your app and any devices, and obtain a `.certSigningRequest`](#ios-setup).

App Certificates & Provisioning Profiles are for signing your app and giving it access to certain devices.

#### Certificate

There are two types of Apple certificates: development and production. We'll guide you through generating credentials with a development certificate.

* Navigate to [Certificates, Identifiers & Profiles › Certificates › Development](https://developer.apple.com/account/ios/certificate/development) in the [Apple Developer Center](https://developer.apple.com/account) and create a new certificate. Under Development, select iOS App Development.

![iOS Cert](/docs/assets/img/appflow/ss-profiles-ios-cert-1.png)

* Step through the steps. Upload the `.certSigningRequest` you created to generate a certificate. Then, download your certificate. It should be a `.cer` file.

Next, we'll need to convert the certificate from a `.cer` file to a `.p12` file. It can be converted on Mac OSX by using Keychain Access and on other platforms by using OpenSSL.

#### Using Keychain Access

* Drag your `.cer` file into your login keychain.
* Right click on your imported certificate and click Export.

![iOS Cert](/docs/assets/img/appflow/ss-profiles-ios-cert-2.png)

* Select Personal Information Exchange (.p12) for File Format.
* Save the certificate, giving it a strong password.

#### Using OpenSSL

* Download your iOS certificate to the same directory as your private RSA key.
* Change the format of the iOS certificate to PEM.

```bash
$ openssl x509 -inform DER -outform PEM -in ios_development.cer -out ios_development.cer.pem
```

* Export the certificate as a `.p12` file, giving it a strong password.

```bash
$ openssl pkcs12 -export -inkey keyname.key -in ios_development.cer.pem -out Certificates.p12
```

### Provisioning Profile

Provisioning profiles give your app access to be installed, or *provisioned*, on specific devices. For iOS App Development provisioning profiles, devices are selected manually.

* Navigate to [Certificates, Identifiers & Profiles › Provisioning Profiles › Development](https://developer.apple.com/account/ios/profile/limited) in the [Apple Developer Center](https://developer.apple.com/account) and create a new provisioning profile.

![iOS Provisioning Profile](/docs/assets/img/appflow/ss-profiles-ios-pp-1.png)

* Under **Development**, select **iOS App Development**.
* Select the correct App ID (with the Bundle ID matching your app's `config.xml` file)
* Select the certificate you generated.
* Select any and all development devices.
* Download the `.mobileprovision` file, which is your provisioning profile file.
