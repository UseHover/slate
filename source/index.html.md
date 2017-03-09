---
title: Hover Documentation

<!-- language_tabs:
  - shell
  - ruby
  - python
  - javascript -->

toc_footers:
  - <a href='#'>Sign up to use Hover</a>

<!-- includes:
  - errors -->

search: true
---

# Introduction

Hover is an Android SDK which allows an Android app to use supported Mobile Money wallets to pay for goods and services in-app.

A developer can initiate a payment when a User presses a button or takes another action, or can create a subscription. Hover will deal with initiating the transaction, including getting the user’s permission, and returning the confirmation back to the app. This means that apps can treat a Mobile Money payment just as they would a credit card payment.

# Install Hover

> Add the Hover SDK to your app-level build.gradle dependencies

```gradle
buildscript {
  repositories {
    mavenCentral()
    maven { url 'http://maven.usehover.com/releases' }
  }

  dependencies {
    classpath 'com.android.tools.build:gradle:2.1.0'
  }
}

repositories {
  mavenCentral()
  maven { url 'http://maven.usehover.com/releases' }
}

dependencies {
  compile('com.hover:android-sdk:0.8.8@aar') { transitive = true; }
}
```

> Add required permissions and API key to your Android manifest

```xml
<uses-permission android:name="android.permission.CALL_PHONE"/>
<uses-permission android:name="android.permission.READ_PHONE_STATE"/>
<uses-permission android:name="android.permission.RECEIVE_SMS"/>
<uses-permission android:name="android.permission.READ_SMS"/>
<uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED"/>
<uses-permission android:name="android.permission.BIND_ACCESSIBILITY_SERVICE"/>
<uses-permission android:name="android.permission.SYSTEM_ALERT_WINDOW"/>
<uses-permission android:name="android.permission.INTERNET"/>

<uses-feature android:name="android.hardware.telephony"/>

<meta-data
   android:name="com.hover.ApiKey"  
   android:value="YOUR_API_KEY"/>
```

> Make sure you replace YOUR_API_KEY with an api key from an app you create

# Wave Money

Currency: <code>MMK</code>

Service id: <code>11</code>


## Check balance

```java
Intent i = new Hover.Builder(this).request("balance").from(11);
```

Retrieve the current balance of the user's Wave Money account.

## Send money (on-network)

```java
Intent i = new Hover.Builder(this).request("send", 
  "5000", "MMK", "+959555555555").from(11);
```

Send money from the users's Wave Money account to another Wave Money account.

Parameter | Description
--------- | -----------
who | Recipient phone number
amount | Amount to send in MMK

## Send money (off-network)

```java
Hover.Builder(context).request("send_money_off_network", amount, "MMK", who)
  .extra("recipient_nrc", "123456").extra("withdrawal_code", "654321").from(11);
```

Send money from the users's Wave Money account to a recipient on a different network.

Parameter | Description
--------- | -----------
who | Recipient phone number (can also be "*")
recipient_nrc | Recipient National Registration Card number
amount | Amount to send in MMK
withdrawal_code | Secret withdrawal code, must be provided by the sender

## Airtime topup

```java
Intent i = new Hover.Builder(this).request("buy_airtime_other", 
  "5000", "MMK", "+959555555555").from(11);
```

Buy airtime with the user's Wave Money account.

Parameter | Description
--------- | -----------
who | Recipient phone number, either the user's or another number
amount | Amount of airtime to buy in MKK
