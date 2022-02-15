[angularjs-crypto](http://ngmodules.org/modules/angularjs-crypto)  
==================

AngularJS Module that integrate cryptography functionality offers from the [crypto-js](https://code.google.com/p/crypto-js/) project for all http requests and response.

Inprogress update to the new angularjs version but before protractor migration.

## Versions

Actual there are two versions one for angularjs 1.2 and one for angularjs 1.3.
- angularjs 1.2 use the 1.2 branch it is tested with the latest 1.2.28 version
- angularjs 1.3 use the master branch it is tested with 1.3.13 version

## Feature Requests

- support public private key encryption/decryption maybe by integrate this javascript [library](https://github.com/travist/jsencrypt)
- encrypt / decrypt binary data like images (setup a working example and maybe support it directly with angularjs-crypto) [example image encryption](http://alicebobandmallory.com/articles/2010/10/14/encrypt-images-in-javascript)

## Code
[anuglarjs-crypto.js](https://github.com/pussinboots/angularjs-crypto/blob/master/public/js/lib/angularjs-crypto.js)

Dependencies
------------
- [AngularJS 1.1.4 + ](http://angularjs.org/) (tested with 1.1.4, 1.2.16, 1.2.28, 1.3.13)
- [Crypto-js 3.1.2 AES modul](https://raw.githubusercontent.com/pussinboots/angularjs-crypto/master/public/js/cryptojs/rollups/aes.js)
- [Crypto-js 3.1.2 ecb mode](https://raw.githubusercontent.com/pussinboots/angularjs-crypto/master/public/js/cryptojs/components/mode-ecb.js)

## Development

To simplify the checkout and the setup of a full development environment with all needed dependencies the  [vagrant-git](https://github.com/pussinboots/vagrant-git) project is used. 

But before we can use the [vagrant-git](https://github.com/pussinboots/vagrant-git) project first install all [reequieremnts](https://github.com/pussinboots/vagrant-git#requirements). It is implemented as nodejs application. Than you can install the [vagrant-git](https://github.com/pussinboots/vagrant-git) project follow this [instruction](https://github.com/pussinboots/vagrant-git#install). Usage turorial can be found [here](https://github.com/pussinboots/vagrant-git#usage).

To setup a development vagrant box for this project execute the command below.
```bash
vgit --repo pussinboots/angularjs-crypto 
```
It will checkpout the vagrant runtime repo and this project itslef.

On Windows without ssh client
```bash
vgit --g https --repo pussinboots/angularjs-crypto 
```
That use https instead of ssh protocol. The ssh protocol is the default used protocol. So the first execution will take a while to download the vagrant [base box](https://vagrantcloud.com/pussinboots/boxes/ubuntu-truly) defined in the Vagrantfile. Than it install the defined dependencies see below. When the login screen appear login with vagrant/vagrant than you have a ready to use development environment for that project up and running. The project will be checkout to the vagrant shared folder so that the Host and Guest operating systems can access the git clone.

The vagrant-git configuration is defined in the [.vagrant.yml](.vagrant.yml) file. The configuration will be explained on the next line the [general explanetion](https://github.com/pussinboots/vagrant-git#project-configuration).
```yml
repo: 
    - pussinboots/vagrant-devel
folder: /vagrant/project/angularjs-crypto
deps: 
    - sublime3
    - nodejs
```

repo: defined the following github repo like https://github.com/pussinboots/vagrant-devel as the vagrant runtime repo where the [Vagrant configuration](https://github.com/pussinboots/vagrant-devel/blob/master/Vagrantfile) is placed that will be used for that project. 

folder: is only information for the Developer that will pe display before vagrant startup so that he or sher knows where the angularjs-crypto project root folder can be found.

deps: defined the dependencies to be installed during vagrant provision.

## Install (manual)

* download [js file](https://github.com/pussinboots/angularjs-crypto/blob/master/public/js/lib/angularjs-crypto.js)
* download [js plugin file](https://github.com/pussinboots/angularjs-crypto/blob/master/public/js/lib/plugins/CryptoJSCipher.js)
* download [cryptojs aes](https://raw.githubusercontent.com/pussinboots/angularjs-crypto/master/public/js/cryptojs/rollups/aes.js)
* download [crypto mode-ecb](https://raw.githubusercontent.com/pussinboots/angularjs-crypto/master/public/js/cryptojs/components/mode-ecb.js)
* added javascript file to your app html file
```html
<script type='text/javascript' src="CryptoJSCipher.js"></script>
<script type='text/javascript' src="angularjs-crypto.js"></script>
<script type='text/javascript' src="aes.js"></script>
<script type='text/javascript' src="mode-ecb.js"></script>
```

## Usage

* add modul dependency ('angularjs-crypto') to angular
```js
var demoApp = angular.module('demoApp', ['services', 'angularjs-crypto']);
```

Example Service Definition

* configure the http request for automatic decryption/encryption detection by setting property crypt:true

```js
'use strict';

angular.module('services', ['ngResource'], function ($provide) {
    $provide.factory('Data', function ($resource) {
        return $resource('/assets/config', {}, {
            query: {method: 'GET', isArray: false, crypt: true},
            queryNoCrypt: {method: 'GET'},
            save: {method: 'POST', params: {}, crypt: true},
            saveNoCrypt: {method: 'POST', params: {}}
        });
    });
});
```

* configure base64Key aes key for decryption/encryption

```js
demoApp.run(['cfCryptoHttpInterceptor', function(cfCryptoHttpInterceptor) {
    cfCryptoHttpInterceptor.base64Key = "16rdKQfqN3L4TY7YktgxBw==";
}])
```

That's done now all json fields that end with the pattern (default: '_enc') will be encoded in requests and decoded in responses.

Issues
-------------
- Report at the github [issue tracker](https://github.com/pussinboots/angularjs-crypto/issues)

Todos
-------------
* configurable error handling strict or elegant mode 
* support for http ajax calls missing only ng resource calls are supported (under investigation)
* add support of encrypted cookies that are decrypted on server side

Features
-------------
* implements ciphers offered by the [crypto-js](https://code.google.com/p/crypto-js/) project
 * AES
 * DES
 * Triple DES
 * Rabbit
 * RC4, RC4Drop
* configuration the cipher algorithm to use (aes is configured as default) (done [see](https://github.com/pussinboots/angularjs-crypto/blob/master/README.md#set-own-plugin-implementation-for-encoding-and-decoding)))
* configuration a function that return the aes secret key to use for encryption/decryption
* encoding of complete query and body for requests
* encoding of query parameter fields that end with the pattern
* configuration of encode/decode function so that you can plugin in your own implementation
* configuration of the aes secret key to use for encryption/decryption
* configuration of the field name pattern which determinate which fields will be encrypted/decrypted
* aes encryption/decryption of http json requests and responses
  * only with mode [ECB](http://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Electronic_codebook_.28ECB.29)
  * only with padding [Pkcs7](http://en.wikipedia.org/wiki/Padding_(cryptography)#PKCS7)
* automatic detection of encryption/decryption for json fields based on the naming rule all fields end with pattern (default: :enc) as it comes 
  * reponse then decrypt 
  * request then encrypt
* only requests / responses of Content-Type: 'application/json;charset=utf-8' will be processed other types will skip crypt processing include auto detection

Configuration
-------------

#### Set the base64Key for aes encryption/decryption

```js
var demoApp = angular.module('demoApp', ['angularjs-crypto']);
demoApp.run(['cfCryptoHttpInterceptor', function(cfCryptoHttpInterceptor) {
 cfCryptoHttpInterceptor.base64Key = "16rdKQfqN3L4TY7YktgxBw==";
}])
```

#### Set the field name pattern

```js
var demoApp = angular.module('demoApp', ['angularjs-crypto']);
demoApp.run(['cfCryptoHttpInterceptor', function(cfCryptoHttpInterceptor) {
 cfCryptoHttpInterceptor.pattern = "_enc"; //that is the default value
}])
```

#### Set own plugin implementation for encoding and decoding

This make it now possible to simple add other CryptoJs cipher implementations like DES or even other crypto libraries as well. If i find the time than i will add at least the supported cipher from CryptoJs. An example implementation that use Crypto AES can be found [here](https://github.com/pussinboots/angularjs-crypto/blob/master/public/js/lib/CryptoJSPlugin.js)
```js
var demoApp = angular.module('demoApp', ['angularjs-crypto']);
demoApp.run(['cfCryptoHttpInterceptor', function(cfCryptoHttpInterceptor) {
 cfCryptoHttpInterceptor.plugin = {
        encode: function(plainValue, base64Key) {
		    return plainValue;
                },
        decode : function(encryptedValue, base64Key) {
                    return encryptedValue;
                }
    };
}])
```

#### configure the cipher algorithm to use

```js
demoApp.run(['cfCryptoHttpInterceptor', function(cfCryptoHttpInterceptor) {
    cfCryptoHttpInterceptor.plugin = new CryptoJSCipher(CryptoJS.mode.ECB, CryptoJS.pad.Pkcs7, CryptoJS.DES)
})
```

- availabe ciphers that are supported by [CryptoJS](https://code.google.com/p/crypto-js/#Ciphers)
```js
demoApp.run(['cfCryptoHttpInterceptor', function(cfCryptoHttpInterceptor) {
    cfCryptoHttpInterceptor.plugin = new CryptoJSCipher(CryptoJS.mode.ECB, CryptoJS.pad.Pkcs7, CryptoJS.AES)
    cfCryptoHttpInterceptor.plugin = new CryptoJSCipher(CryptoJS.mode.ECB, CryptoJS.pad.Pkcs7, CryptoJS.DES)
    cfCryptoHttpInterceptor.plugin = new CryptoJSCipher(CryptoJS.mode.ECB, CryptoJS.pad.Pkcs7, CryptoJS.TripleDES)
    cfCryptoHttpInterceptor.plugin = new CryptoJSCipher(CryptoJS.mode.ECB, CryptoJS.pad.Pkcs7, CryptoJS.Rabbit)
    cfCryptoHttpInterceptor.plugin = new CryptoJSCipher(CryptoJS.mode.ECB, CryptoJS.pad.Pkcs7, CryptoJS.RC4)
    cfCryptoHttpInterceptor.plugin = new CryptoJSCipher(CryptoJS.mode.ECB, CryptoJS.pad.Pkcs7, CryptoJS.RC4Drop)
})
```
#### Configure the Content-Type header for encryption/decryption
```
demoApp.run(['cfCryptoHttpInterceptor', function(cfCryptoHttpInterceptor) {
	cfCryptoHttpInterceptor.contentHeaderCheck = new ContentHeaderCheck(['application/json', 'application/json_enc']);
}
```
The default configured Content-Type is shown above. That means only for request's and responses 
that have one of the aboved Content-Type will be enrypted/decrypted. For example if you perform a request with the Content-Type : 'text/plain'. This request will be skipped for encryption even if shouldCrypt is set to true.

There is also the possibilities to implement your own ContentHeaderCheck that for example always return 
true. Like that own below.

```
function ContentTypeDoesntMatterCheck() {
    return {
        check: function(contentType) {
            return true;
        }
    }
}
```
To use the new implemented ContentHeaderCheck apply following configuration code.
```
demoApp.run(['cfCryptoHttpInterceptor', function(cfCryptoHttpInterceptor) {
	cfCryptoHttpInterceptor.contentHeaderCheck = new ContentTypeDoesntMatterCheck();
}
```

#### Activate console logging

```js
var demoApp = angular.module('demoApp', ['angularjs-crypto']);
demoApp.run(['cfCryptoHttpInterceptor', function(cfCryptoHttpInterceptor) {
 cfCryptoHttpInterceptor.logging = true; //the default value is false. True will log the decrypted and the encrypted values to the console.
}])
```

#### Complete encoding of query parameter

```js
$provide.factory('Data', function ($resource) {
        return $resource('/assets/config', {}, {
	        queryFullCrypt: {method: 'GET', isArray: false, fullcryptquery:true}
        });
    });
```

#### Complete encoding of body

```js
$provide.factory('Data', function ($resource) {
        return $resource('/assets/config', {}, {
            saveFullCrypt: {method: 'POST',  fullcryptbody:true}
        });
    });
```

##Example

#### Key Example

Change the base64Key locally by read it from the rootScope.

```js
var demoApp = angular.module('demoApp', ['angularjs-crypto']);
demoApp.run(function(cfCryptoHttpInterceptor, $rootScope) {
    cfCryptoHttpInterceptor.base64Key = $rootScope.base64Key;
    cfCryptoHttpInterceptor.pattern = "_enc"; //default value but for a better understanding it is also defined here
})
```

#### Key Example Function

Define a function which will be used to get the key for encryption/decryption is called 
for every encryption/decryption process.

```js
var demoApp = angular.module('demoApp', ['angularjs-crypto']);
demoApp.run(function(cfCryptoHttpInterceptor, $rootScope) {
    cfCryptoHttpInterceptor.base64KeyFunc = function() {
        return $rootScope.base64Key;
    }
})
```

With this html snippet you can edit the key to use only locally.

```html
<input type="text" ng-model="$root.base64Key" />
```

Demo
-------------

live
------

The http calls are mocked with angular-mock.

[Http Get Example](http://angularjs-crypto.herokuapp.com/#/get)

[Http Post Example](http://angularjs-crypto.herokuapp.com/#/post)

[Http Get query parameters encoding ](http://angularjs-crypto.herokuapp.com/#/query)

[Complete query encoding](http://angularjs-crypto.herokuapp.com/#/fullquery)

[Complete body encoding](http://angularjs-crypto.herokuapp.com/#/fullbody)

[Change base64Key Example](http://angularjs-crypto.herokuapp.com/#/key)

License
--------------

angularjs-crypto is released under the [MIT License](http://opensource.org/licenses/MIT).
