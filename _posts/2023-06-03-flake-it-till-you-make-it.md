---
layout: post
title: Pixel Experience Plus Fan Edition
subtitle: A semi modified rom by NopeRoms
cover-img: /assets/img/pixel.jpg
thumbnail-img: /assets/img/pixel.png
share-img: /assets/img/pixel.jpg
tags: [roms, pe]
---

This blog will detail how I did what I did, and how much work it actually was. You will need to know some basic JAVA to actually understand a bit of this, so let's begin. Remember to use github search as that does help quite a bit, github also provides github.dev for easy editing files so use that too

1. Search for OTA in vendor/aosp, vendor/aosp is usually where everything that is semi-important and should be built into the ROM is located and look for any matches,
2.  `ifneq ($(filter OFFICIAL CI,$(CUSTOM_BUILD_TYPE)),)` <br />
`PRODUCT_PACKAGES += \` <br />
`Updates` <br />
`endif`<br />
This is a very interesting line of code, it seems like it looks at the CUSTOM_BUILD_TYPE and sees if it contains OFFICIAL or CI, if it does then it adds Updates to the Product Packages, if we remove that if statement we can get the OTA app to build
3. We have gotten a way for the OTA app to be build but we havent so far updated the url in the OTA app itself to point to our server, so first create a github repo, name it whatever you want, make a folder whose name is the name of your device, and make a file called thirteen_plus or whatever the name your build version is. Look at https://api.pixelexperience.org/ota/whyred/thirteen_plus for any example JSON response.
4. Modifiy the values according to your needs, remember it uses md5sum as hash, and you can find out the time by `date +%s` in any linux terminal, If you want to push an update and you are also releasing the zip standalone then please use the UTC time that is there in /system/build.prop on your device
5. After you are done modifiying update the thirteen_plus file you just made it your github repo, the path should be like this device/thirteen_plus
6. Now click on the raw icon on the top bar, it should lead you to a page with only the text
7. Look at this commit so that you can change the URL: https://github.com/NopeNopeGuy/packages_apps_Updates/commit/c1cb13a4523e942fa9b2c207247bbae3172f9b59 (note this is how I am explaining it, as JAVA code is very self-explantory), and also look at this so you can use it in unofficial builds easily: https://github.com/NopeNopeGuy/packages_apps_Updates/commit/cab42e1779c14816d6e17fed7bc9e8ae05537180
8. You now should have a working OTA app, but you might've noticied it doesnt show up in the Settings menu, this is probably because there is a check that checks if the build is official or not, and it is probably in the Settings app itself
9. After searching in the settings app you should find a check that should be like this: https://github.com/NopeNopeGuy/packages_apps_Settings/commit/6c7f0c139e9a956c6308b9e4eceeba03b6a9a824, replicate that commit and remove the check.
10. After completing all the steps you should easily be able to make your own PixelExperience version with OTA support enabled, goodbye