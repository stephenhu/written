# Migration from Swift 2.3 to 3.0 and Xcode 7.3 to 8.2.1

I've read several articles on migrating to Swift 3 which are all quite good, but this is my experience migrating my iOS mobile app to Swift 3/Xcode 8.2.1.  I started my iOS development back in January of 2016 and right around October everything turned upside down with the release of Swift 3.  Since I was furiously trying to get features into my app so I could go live on the App Store, I had put migration in the backlog for some time with a lower priority, if it ain't broke then don't fix it, right?  It wasn't until I wanted to leverage some external libraries that I decided Swift 2.3 was holding me back so it was time to upgrade--I'm looking at you openears!  I seem to remember that you can install different Swift compiler versions and reference it by changing symlinks or something, but I didn't want to hack up my development environment so the only way was to first upgrade Xcode 7.3 to 8.  Fortunately I have multiple Macs and can install different versions of Xcode to play around with, but my first experience with Xcode 8 after you open up a workspace/project is that you get this annoying popup dialogue asking if you want to convert your code to Swift 3, there is an option to do this **later**, but if you click this, there's a follow on popup that asks if you're sure you want to do this later and reverses the order of the **later** and **convert** buttons, it's almost like those free porn sites where they have these almost real looking hyperlinks upon first look, but when you click on them, they end up opening 50 tabs on your browser to obscure places on the web, and then you notice the actual link a little bit further down the page, completely dishonest, but there's no material gain for Apple seemingly for this, they must really want developers to convert to Swift 3 and to do it quickly!  Prompting for conversion immediately smelled to me of some interface incompatibility issues with Swift 2.3, yes, I'm one of those people that never read the manual and dive directly in, on the one hand it's nice that Apple's created a tool to help with migration, but that just scares the living bejesus out of me, holy sweet baby jesus, I need a tool to get me through this!  Another ensuing dialogue pops up if you click **convert** asking which frameworks to include in the conversion.  For the record, I have been using Cocoapods and rely on quite a few external libraries:  Alamofire, Crypto, Font-Awesome-Swift, KeychainSwift, Kingfisher, SwiftyJSON, and SwiftWebSocket.  By default, all of your pods are included and checked for conversion.  I typically take the defaults if I'm new to something, it's like when you're reading Charles Dickens for the first time, say The Great Expectations, and you find yourself checking the dictionary every 10 words to the point where you're fed up and then you realize that if you had only skipped ahead, you'd probably get much more context from the next 20 words and could take a pretty darn good guess at what the word means, the same applies here, no need to go knee deep if it's unnecessary, there are plenty of other things to learn, it'll all start to fall in place over time.  But taking the defaults ends up giving you a huge list of files that need conversion, including some of the external library source files.  So if you go to the migration guide for Swift 3, it suggests that you either wait for the author to make their library Swift 3 compliant, or you include their sources in your project as a sub-module and do the conversion for them, hell no, not in a million years, I'm not going to maintain that code base for any amount of time, perhaps do a pull request, but I haven't the time!  For all of my external library dependencies, I noticed that they had mostly migrated to Swift 3 either stated explicitly on their website or through my sniffing around in their github issues, but the fact that the migration tool was telling me that they had all this incorrect syntax in their library really caused me to be doubtful.

So that first experience caused me to give up and go back to adding features to my app instead.  But over the Christmas Holidays, I decided that it was time to bite the bullet and take care of this technical debt so that I could run full speed in 2017!  For some reason, I looked at all the github pages again for my external dependencies and all the github issues searching for Swift 3 and they all confirmed that they mostly had support for this, which cleared some of my doubts, so I tried again.  And again, the popup with all the frameworks checked by default showed up again and this time, what's typically my last resort, it was trial and error time, I decided to uncheck all the external libraries because they _should_ all be Swift 3 compatible, I was seeing some glaring 2.3 code from the conversion diff output, it was like the source code was cached somewhere for an older version that wasn't Swift 3 compatible (I deleted my Pods directory several times to make certain that I had the latest external depedency sources).  Low and behold that worked like a charm, what I was left with was the mess of code from my own source tree that needed to be converted to Swift 3 compatible syntax, rock on!

Most of my code conversions were changes to variables, like Alamofire, for instance, used to call something like this to get a singleton of the Alamofire instance:

`let web = Alamofire.Manager.sharedInstance`

And this is the new code under Swift 3

`let web = Alamofire.SessionManager.default`

Not entirely intuitive, but easy to fix with find/replace.  The next type of conversions that seemed frequent were for things like requiring a key for each parameter passed into a function, it used to be that you could omit the very first parameter key, but Swift 3 does not like this very much, in fact it hates it and will hate you and never let you compile the code successfully in a million years or until you fix it.

I also ran into several issues where a class lowercased its enum list like for UIAlertActionStyle and UIAlertControllerStyle, but those were trivial.

I went through this motion for about 2 hours, I have about 30 swift files.  Alamofire was the most widely used external library and had changed the way responses that do not include data were handled, this tripped me up a little bit at first, the documentation was not succinct so I had to resort to stackoverflow, but most everything else was simple.  SwiftWebsockets didn't require any changes, another thing to note is that the compilation errors which ranged in the triple digits only showed something like 10 at a time, after I fixed those, Xcode would give me the next 10 or so, I didn't explicitly count, but I really think it was around 10 or so, why Xcode just didn't give me the 100 or so errors all at once, I don't know, perhaps it's one of those glass half full things, so fine, whatever, don't have the time to figure that out, just move on.  Eventually the compiler was happy, save for those hundred or so warnings from my storyboard regarding `Frame for "Stack View" will be different at run time.`.  There were some other subtle errors that I did have to clean up as a result of the conversion where I was initializing some associative array like such:

```
var game = [String: String]()

return game["id": self.data[indexPath.items][0], "name": self.data[indexPath.items][1]]
```

The error I received was "Expression was too complex to be solved in reasonable time."  Under 2.3 this was not a compile error and seemed to work fine, under 3.0 this was preventing me from successfully building.  I fixed this by unravelling the expression like so:

```
var game = [String: String]()

game["id"] = self.data[indexPath.items][0]
game["name"] = self.data[indexPath.items][1]

return game
```

After all these changes, I was finally able to compile without any errors and build my archive.  Here's where things hit the fan, and I still don't know how I got through this, but I'll try to give you all the steps and missteps that I took.

So the archive built fine, this was always the same process for me, make sure things compile fine with no errors, run automatic and manual tests to verify basic functionality.  If all looked good I'd then proceed with bumping the build number, changing the API urls (I have these stored in a swift file where I toggle between production and development, I have a backlog item to create different info.list files for production and development), setting the devices to **Generic iOS Device**, and building my archive.  When the archive's done, the Xcode Organizer pops up and I upload the binaries to the App Store.  I've successfully gone through this process multiple times, creating a release version through iTunes Connect and then waiting the 2 days for the app review process to take its course.  But suddenly under Xcode 8.2.1, I got this really ambiguous error message **ERROR ITMS-90046: "Invalid Code Signing Entitlements.  Your application bundle's signature contains code signing entitlements that are not supported on iOS.**  The error code is specific enough, but I spent hours on google and stackoverflow, most of the advice can be summed up with the following bullets:

_Be forewarned, use these at your discretion, there are good chances that these steps could alter your environment in a non-reversible way._

* Project -> Clean, then restart Xcode, some responses even suggested restarting Mac OS X, I think...
* Delete the ~/Library/Developer/Xcode/DerivedData contents which flushes the cache, some of the profiles get cached
* Delete ~/Library/MobileDevice/Provisioning Profiles contents
* Clear out Keychain entries for iOS Developer/Production Provisioning Profiles and associated private keys
* Turn off/on Automatically manage signing under (Project Name) -> Target -> Signing
* Delete/Revoke all certificates and provisioning profiles from developer.apple.com, recreate and import
* Make certain the Bundle ID in the build settings and info.list are matching
* Make certain the TEAMID is matching with the Bundle ID
* Make modifications to Entitlements.list (never found this file)
* Target -> (Project Name) -> Signing -> Code Signing Identity for Debug and Release use _iOS Developer_
* Disable Associated Domains for the provisioning profile

I also read a lot of horror stories about how this issue happened often with Xcode 4-6, but quite honestly in Xcode 7.x, I never ever ran into issues with code signing, it always just worked, I followed the instructions from the apple developer documentation the first time to set up certificates and profiles and it just worked smoothly after that, not one single issue, so this just baffled me, something was broken.  For the record, I did buy one of those new macbook pro touchbar laptops and imported my developer profile from another laptop, but I had done this successfully in the past, so that probably wasn't the issue, it probably has to do with this **Automatically manage signing** feature in Xcode 8.

Anyhow, none of these worked for me and I was getting frustrated because I needed to release a new version of my app, so I started down all the different permutations of these changes.  Here's what seemingly worked for me:

_Note:  use at your own discretion, may have non-reversible effects on your environment._

1.  Delete/Revoke development/distribution certificates and provisioning profiles from developer.apple.com
1.  Create new development/distribution certificates and provisioning profiles on developer.apple.com
1.  Project -> Clean
1.  Quit Xcode
1.  Delete contents from ~/Library/Developer/Xcode/DerivedData
1.  Delete contents from ~/Library/MobileDevice/Provisioning Profiles
1.  Make certain iOS Developer was selected for Debug/Release for code signing identity in build settings of the target project, i didn't need to change these, this was already set correctly from 7.3
1.  I found that my _Product Bundle Identifier_ under Target -> Build Settings -> Packaging had a lowercase version of my app, so I proper cased this
1.  Open Xcode, open workspace
1.  Build Archive
1.  Upload the App Store

Voila, no more code signing error messages, the archive was sent successfully!  However, from iTunes Connect, I couldn't find my build, the add build plus sign wasn't even visible from the release submission page.  I was panicking that I had deleted my profile incorrectly which would cause me to have to open up a support ticket with Apple, anything but that!  But later, I received an email from Apple stating that my build submission was missing a string in the info.list, particularly **NSPhotoLibraryUsageDescription**, I export photos to the Photo Library and this message is needed by Xcode 8, apparently, not needed by Xcode 7.  If your app doesn't access the Photo Library then I think you're good, no change necessary.  This was easy enough, rebuilt my archive with this change, uploaded to the App Store, and after 30 minutes or so, the build was available and selectable for my release, and I was able to submit for review!  As of the time of writing this article, I am still waiting for my review to complete, but this appears normal thus far, will update you if that fails for whatever myriad of reasons.

Overall, the update to Xcode 8.2 had resulted in me burning roughly 10 hours sifting through misleading forum suggestions and trying every permutation I could think of, not the best of experiences surely, and though I am a fan of iOS development and I hope that the tools improve over time, this was painful, I lost a lot of productivity and do not think I'm any stronger as a result of this except that my ego is even more inflated as I have solved the impossible yet again.  Apple's developer forum was the most useless of these, Stack Overflow slightly better, and I suggest that Apple provide better documentation and FAQs.  Perhaps I had changed my app bundle name from uppercase to lowercase some time ago so no one else was running into the same issue because surely someone else must have run into this, but this worked on Xcode 7, perhaps if I just made this bundle identifier change instead of running around and deleting/recreating certificates, I would have fixed this issue immediately.  Nevertheless, I seemed to have gotten through this rough patch for now, back to developing features.

_Before you click any of these, be forewarned, madness ensues..._

* https://developer.apple.com/library/content/qa/qa1879/_index.html
* https://developer.apple.com/library/content/documentation/General/Conceptual/DevPedia-CocoaCore/AppID.html
* http://blog.bitrise.io/2016/09/21/xcode-8-and-automatic-code-signing.html
* http://stackoverflow.com/questions/15881534/how-to-localize-nsphotolibraryusagedescription-key-alassets
* http://stackoverflow.com/questions/39432242/nsphotolibraryusagedescription-in-xcode8
* http://stackoverflow.com/questions/28106791/error-itms-90164-90046-invalid-code-signing-entitlements
* http://stackoverflow.com/questions/29877677/apple-store-submit-fails-with-error-itms-90046-but-associated-domains-is-not-am
* http://stackoverflow.com/questions/34905742/error-itms-90046-invalid-code-signing-entitlements
* https://forums.developer.apple.com/thread/12758

