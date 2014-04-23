Easy Enterprise Mobile Development
by Corinne Krych

This is not meant to be an exact transcript of the talk but rather a sort of guide.

#### Slide: Easy Enterprise Mobile Development
- Welcome
- talk about enterprise mobile dev
- i'm going to talk about just web, or iOs app, my native paltform by heart
- take the whole view; from web to hybrid to native
- polyglot by essece, outside the JVM
- How to ease five-legged sheep syndrome
BUT let's start iwith a quick introduction
#### Slide: Corinne Presentation
With over 15 years of experience in IT.... 
- I am Athos for the 3musketeers dev Grails Mobile Solution within OS team
- I am cofounder of RGUG
- co-organizerr of JSSophia
- I work for RH
- part of AG mobile project, I’m iOS tech lead
- I’m also part of the Duchess team
#### Slide: Part1. What are the challenges of mobile app?
Let’s try to see what are the ingredients that made a classical web app transform into a mobile web app
#### Slide: Size
First of all, fLet’s face it… Size does matter
- you can not display the same amount of information in that device...
- information need to be displayed differently: one columns
which us to the topic of Responsive web design
=> Responsive web design
RWD can be achieved today with  together html5+CSS3
- For ex you always have to set the view port in html to fit your device width
- you can define css depending device size, device capabilities
- don’t use pixel use percentage/em => flexible image
#### Slide: Push
Today with mobile app you need to change your approach to design. Your app dont pull the server for information.
App dev push contextual information to the user in the format of push notification.
In native app, there is such service:
Google Cloud Messaging for Android and 
Apple Push Notification Service for iOS
where you push short version notification
With Firefox OS web based push notification is on its way.
Your own server can send push msg through websocket or equivalent protocol.
#### Slide: Surviving offline
Next challenge : we need to survive offline
- with the mobile nature of smart phone you can not assume you’ll be connected all the time.
you walk in the street …
- deal with offline mode using caching for reading data for static data
- you also need to deal with input data 
but you may go under a tunnel, go offline 
- offline mode required architectural change in your app: no more JSP and server side rendering
So application logic/flow in server
Moved some application logic and flow on the client
You should take advantage of localStorage, indexed db when you are in offline mode all  your read/write operation
#### Slide: Synchronise
When going back online your application need to synchronize operation with the server transparently
it’s a 2 steps approach:
1. send modified data from client to server. 
2. do the actual sync: retrieve modified data from server => math sync
#### Slide Security
When building Enterprise mobile app, you can't ignore security
- secure your data locally storage, encrypted cache
- auth/authz who you are, what data you can access
#### Slide: making money
Last but not least, we have a full mobile web app. But how to make money out of it? => How to put it on the stores?
and also how do you deal with your device capabilities not yet available in html5?
what about packaging your app for a Store?
=> Hybrid!
- phoneGap now called Apache cordova. fwk like cordova titanium will allow you to get access to your device through JS 
- the utimate goal is to cease to exist. mind the gap.
- the future firefox OS: full access to modible device through html5+JS
- develop your own plugin
#### Slide: PartII-AeroGear
#### Slide:100% mobile
AG is a 100% open source project. All src code is hosted on gh, if you look under AG organisation, you will see around
60 gh repo with more than 20 committers. Started in 2012/1013. it's sponsored by RH JBoss, it's multiplatform mobile project
Ag is about mobile: unified API - not just JS/Cordova it includes Native aPI.
AG is not a fwk
it’s rather a set of libraries
it provides all the plumbing you need for mobile
it’s not just focus on JS it supports all platforms: JS/Cordova but also native android  and iOS libs
#### Slide: web, native, hybrid
as I said why would you have to choose you can start with HTML5 and web dev
package with cordova, going hybrid, even use some of our cordova plugin to work as native
but also you can dev your client mobile with native - we’ll talk at themed on how to ease the transition -
thanks to your unified APIs
#### Slide features: Core/Push/Security/Sync
what our main features? we’re a set of libraries declined for Android/ios or JS
- libs for core: Pipe/Stores/auth - bare minimum for mobile app
- libs to support Push notification
- libs to Security: OTP, encryption
- future libs for sync, secure offline caching support
#### Slide Cookbook
Every time we release a new features we create a small demo app associated to it
Polyglot too, cookbook is declined into 3 gh repos: android, iOS and web
all along the presentation, i will illustrate my presentation with a concrete example taken from the cookbook
#### Slide Pipe
abstraction of connectivity
jax-rs compliant: not only jBoss, for ex last month I presented AG at Greach (work out of the box with grails rest
endpoint)
but also server agnostic
Here is how you create and read a pipe in JS
Async operations either use callbacks or jQuery promises
similar apis exists in Java and Obj-C
#### Slide Auth
simple digest auth support with PicketLink/soon Keycloak
OAuth2 support
seamless int with pipe once authenticated
#### Slide Store
very similar to Pipe API
Synchronous for most storage... but JS diversities: IndexedDB, webSQL are asynchronous return promise
encrypted version
#### Slide cookbook recipe store in JS
- demo read/save IndexDB(asynch)
select frame index.html
app.agStore.read().then(function(data){$.each(data, function(index, value) {console.log("title:"+value.title)});})
add: Bouillabaisse
Rascasse, rouget
app.agStore.save({title:"Bouillabaisse", id: "101"})
indexedDB.deleteDatabase("recipes")
- demo localStorage(synch)
app.agStore2.save({id:"101", title: "My own recipe"})
==> abstraction to your DB
#### Slide Push notif
Short Messages to ...notify!
No connection to maintain: save your batteries
Device registers to the UPS at start up of the app (ask permission)
One of big issue with push notification => UPs to unify different "propriatary" networks" GCM, APNs, simplePush
UPS admin to monitor, openshift cartridge to create/run a UPS server in a minute
Unified Client Libraries to send messages
#### Slide Push with without UPS
You can do without
But doing with, simplify your dev specially if you target multiple markets
#### Slide Push voc
Push Application</span>: A logical construct that represents an overall mobile application. </li>
Push Variant</span>: A variant of the Push Application. There can be multiple variants for a Push Application.
A Variant contains platform specific properties, such as a Google API key (Android) or a PushNetwork URL (SimplePush).
Installation</span>: Represents an actual installation on a mobile device / client.</li>
Push Notification Message</span>: A simple message to be send to a mobile application, to notify it of a data change.
Sender API: Is a component that sends Push Notification Messages to a Push Application or some of its different Variants
#### Slide OpenShift Demo
Let's see it in action: demo how to createUPServer on OS.
Not to dependant on network and wifi, i've done a recording of it.
- go to OS web site, login or register for free, up to 3 free app.
- create a new app with using AG cartridge
- give a name "pushme"
- go to the url and log in
- first time: admin/123, change password
- create your app from admin console
- name/short description: HelloApp
- Once application created you will have a Application ID and a Master SEcret. Take notes of this
there will be needed by mobile client and your backend app to send push notification and/or UPS admin console
- Variant creation: android one name, GCM
- in Google console developers you will have create an app: project number + API key
take note of those they will be required for mobile client
Create an app in GCM or Apple DEv. center is a pre-requisites. Required certificates-<Apple/Grants for GCM
 Well documented in ag.org
#### Slide Android client Demo
- login back to UPS admin console
- clone Helloworld app, part of cookbook
- in admin console copy paste the configuration information: variant Id and its secret, GCM_SENDER_ID == google project
number and UPS URL
- build: mvn build
- deploy: adb install apk
- first time app is launched it's going to register itself
- back to the console, you should be able to see a new installation
- Go to compose message (no filter)
- send notification when app is in back ground.
- next: write your own backend to send push notification (instead of compose msg feature in admin console)
- Helloworld is a simple demo with client registration sdk but no backend, use of admin console to send msg,
for a more complete ex see AeroDoc
#### Slide HelloWorld registration
Here is how to register in Android
Register method is asynch and takes 2 param success/error
#### Slide Sender API
SEveral ways od sending push notifications msgs:
- compose msg from admin console either from OP or locally deployed
- curl: login, send msg
- via sender api which is avail in Java and JS
#### Slide Security
Authentication/Authorization</strong> server side: Shiro/PicketLink adapter</li>
Soon to come KeyCloak adapter</li>
Encryption</strong>: key to enterprise mobile app
- symmetric encryption (private key)
- asymmetric encryption (private/public keys)
 encrypted Storage - encrypted offline mode
#### Slide Auth, authz
auth method: simple digest
OAuth2 compliant
integrate well with Keycloak: SSO server side ....
#### Slide OAuth2 with google
with GoogleDrive
The authorization code grant starts with the client, Shootnshare, redirecting the resource owner to the
Google authorization service.

After authenticating the resource owner and obtaining the resource owner's authorization, Google redirects back to
shootnshare mobile app with an authorization code that the client uses to request the access token.

With access token (limited intime, could be refrehed) shootnshare can access the protected resource owner's resource.
1. Hello, Mr GoogleAuth I'm Shoot'nShare app, could I get access to Google Drive services?</li>
(optional). Which account, please identify yourself?</li>
(optional). I am Bob, here's my login and password</li>
2 . Hello, Bob, do you want to grant access to GoogleDrive app?</li>
3. Yes I do. I trust this app.</li>
4. Ok fine, Let me use the client callback URL to get back to the app. Here is an access code for you GoogleDrive.</li>
5. Thanks. Mr Google may I exchange my auth code for an access token?</li>
6, 7. Here's your access token GoogleDrive Enjoy.</li>
8, 9. So Mr Google could I get access to Bob's list of files, here's my access token.</li>
10. Let me see if your access token is still valid, ok fine here's the list your requested.</li>
#### slide shoot'nshare
- use cocoapods for dep
- open xworkspace
- run on device, easier when taking pictures
- for ex i take a pic of the audiance and I share it on my google drive
- in AGShootViewcontroller: authz happens
instantiate it with config, lots of URLs
callback url should also be part of shoot-info.plist URL schema
on AGAuthorizer I call asyn method authz, with sucess/error
- See pic on google drive

#### Slide OAuth2 + KC
Keycloak: what is it?
-auth
-authz
-Oauth2
-SSO
-Role mgt
- admin console, JSON configuration
Keycloak is a new open source authentication server for cloud, mobile and html5.
With loads of features, including single-sign on, social login, account management console, account workflows,
fully featured admin console, OAuth2 and OpenShift cartridge to name a few.
The first alpha has recently been released, with loads more features planned for the future.
Keycloak also provides support for role based authorization and supports granting access to third party applications.
If you want to know  more about the project i recommand Stian recorded presentation
It's the new jboss open source project
Demo app:
Disclaimer: this recipe is still in a Pull Request
Replace GoogleDrive by our own Java backend</li>
Use Keycloak with for Auth/authz</li>
Configure OAuht2 client</li>
Keep the same iOS client code</li>
version 1.0-alpha4, integrate well with wildfly</li>
#### Slide encryption
=> Same unified approach to crypto:
unified crypto lib
unified crypto stores APIs
=> easy way of use For ex Crypto baox has 2 methods encrypt/decrypt
- For Java, built on top of javax.crypto and
Bouncy Castle
to support: AES-GCM (Advanced Encryption Standard (AES) in Galois/Counter Mode (GCM)) authenticated encryption,
password based key derivation and
elliptic curve cryptography.
- for JS, based upon Stanford Javascript Crypto Library (sjcl) uses CCM (counter mode) and OCB authenticated-encryption modes
- for iOS, based upon NaCI, (previous version we used default ios foundation fwk commom crypto using CBC mode) to allow
ECC (Elleptic curve)
#### Slide Part III Going hybrid with cordova
#### Slide What’s for
- go hybrid: present your app as a native app, by embedding a web view in your app
- bridge: cordova plugin to access your device contact etc…
- write once deploy everywhere attractive features
=> easy to write plugins for different platforms
#### Slide Cordova build
picture extracted from phone gap build (apache mbaas in the cloud)
#### Slide cordova+AG
part of our offering
aerogear-pushplugin-cordova: extend of the standard Cordova Push Plugin with AG Unified push server
aerogear-crypto-cordova: allows you to use the native aerogear crypto libs for your cordova apps. While staying close
to the aerogear-js api.
aerogear-geo-cordova:  collection of tools to make you live easier when using geo posistioning. It provides options
to easly create maps that are based openlayers
aerogear-otp-cordova: Cordova plugin for OTP depends on BarcodeScanner to be able to easly obtain the secret
#### Slide the magic in 4 commands
#### Slide CORDOVA DEMO
cordova create food
cd food
cp -rf ../food_recipes_data_manager/* www
cordova platform add ios
cordova build
cordova run
#### Slide the end
#### Slide Open source community
we need you
contributing is easy
start small and do more
here is some tips on how to help..
#### Slide hackengarten
#### Slide thanks+QA


