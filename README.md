# resourceTools
Tools to encrypt/decrypt ResourcePack(minecraft)

Rust version(Other authors): https://github.com/valaphee/mcrputil  

This repository does not reference any literature, I discovered these on my own  
Therefore, the Unlicense license is legal, since I am not citing any code   
  
Please make a backup of your resource pack before running this code, it can even destroy your original resource pack  

>　mcrputil posted on October 28, 2022 and I was independently discovered on November 18, 2022 at 6pm
> I was unaware of the existence of mcrputil at the time I began my research

#### Required Environment
- php 8.0 or php 8.1, 8.2 and php 8.3 are maybe supported
- openssl extension
- ext-zip extension (optional)

These requirements can be met by using the official pmmp php binary  
Please note that if needed, install the vc_redist.x64.exe that comes with the binaries  
https://github.com/pmmp/PocketMine-MP/releases/latest  
For Windows, run the following
```
.\bin\php\php.exe resourceTools.phar 
```

## command
```
  c           decode contents.json
  decrypt     decrypt resource
  encrypt     encrypt resource
  rdecrypt    decrypt resource recursively
```

## build
(and Install dependencies)
```
composer make-phar
```
To build a phar that does not use `symfony/console`, the following command can be used
```
composer make-mini
```
In case composer is not installed globally, a portable composer.phar can be used to install dependencies
```
php composer.phar make-phar
```
> The portable `composer.phar` can be downloaded from the following link  
> https://getcomposer.org/download/latest-stable/composer.phar  

## usage
### encrypt

```
php resourceTools.phar e input -o output
```
### decrypt
If the key is not specified with the `-k` option, the program will search for the `*.key` file in the root folder of the resource pack
```
php resourceTools.phar d input -o output
```

## options
```
-k, --key=KEY         32byte keys
-o, --output=OUTPUT   output dir [default: "encrypt"]
-p, --with-progress   use progress bar
```

## Other usage
This script supports reading and writing zip files  
Known Bug: Creating a zip file may result in folders inside the zip not being created properly due to improper handling of paths  
```
php resourceTools.phar encrypt input.zip -o output.zip
php resourceTools.phar decrypt input.zip -o output.zip
```
The modularized program can also be executed directly
```
php encrypt.php input -o output
php decrypt.php input -o output
```

### override
(All contents.json should be deleted after processing)
```
php resourceTools.phar rd -d original -o original
```

### Other points to note
The main program does not support non-printable keys because it uses file() and trim()  
Unprintable keys are not tested throughout the program and may cause the key to be rejected as invalid  
If there is a resource pack that uses non-printable keys, decrypt.php may work  
Please note that when using non-printable keys, the key file must be 32 bytes and must not contain newlines  


If the -r option is used with encryption, the program will generate resource_packs in pmmp format  
If the -r option is used, you must do the zip creation yourself  
( please note that contents.json must be in the root of the zip and does not support resource_packs in nested folders)
```
resourceTools.phar e testR1 -r -o "resource pack name"
```