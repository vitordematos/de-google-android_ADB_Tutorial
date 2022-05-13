# How to de-googlize, increase the security and improve the privacy and usability of the Asus Zenfone via ADB

In this article we will learn how to *de-googlize* the smartphone and uninstall all the *bloatware* that ships from factory without compromising device functionality. For this we will use the ADB command (android debug bridge) in Linux.
When we buy an android smart-phone, in theory we get a Swiss army knife for everyday life, but in practice, as we buy it from *consumer electronics* the device comes with so many *trackers* from  factory that we will never be able to take advantage of its full potential unless we make some changes. As the firms that produce them continue to prioritize not user experience and freedom, but rather the *targeted advertising* model that forces tracking of user behaviour, the myriad of trackers that this option entails slows down the device and makes it harder to use, often forcing the device to consume more memory, more battery and, should we decide to take action to counter this abusive behaviour, using *disable* or *freeze*, causing *loops* that will hinder network access. The only option is to prevent those apps from running without our consent, thus ceasing to compete with the apps we intend to use. Because these are system-level apps, *Uninstall* is the only effective command to stop them. We can then get rid of those .apk that only serve third party interests, doing nothing for the user-purchaser, who is the one who has the legitimate right to use the device as and only as he wishes. In order to enjoy the Swiss Army Knife for which we gave up our money, we have to clean the device we just bought! Yes it is the sad truth. We can pay more for *more* factory clean alternatives like [Volla Phone](https://volla.online/en/) or [Murena Fairphone 3+ - eSolutions - deGoogled phones and services](https://esolutions.shop/shop/murena-fairphone-3-plus-fr/) or else clean the phone ourselves via adb. Just follow this manual.

Pre-requisites: when you first turn on your phone you must not have any SIM card with data plan or memory card inserted in your device and, despite the Google app asking us to do so, you must not allow your device to connect to any networks. And, of course, we must not enter any kind of credentials or phone number into any *system* app. Not clicking *yes* nor *accepting* nor *agreeing* to any questions the Google app asks us. Just keep saying *no* and *set "later "* until we can exit the app. Close the application.
To access the developer tools that are *hidden* in android go to *settings* > *about* > *software information* > and repeatedly click *build number* seven times. You'll get a *toast* message saying *"congratulations! You are now a programmer "*. From there on the second-to-last entry in the *settings* should read *developer options*. So let's get to it!

1. On the phone, enable *usb debugging* in the > *settings* > *developer options*. It is also advisable to select *stay awake*. In my case I opened the [appManager](https://github.com/MuntashirAkon/AppManager), searched for the app to uninstall and kept the app open to verify that by running the adb command on the pc, the app *disappeared* effectively from the smartphone screen (it was no longer available to user 0).
2. In Linux open the terminal, in the case of Kubuntu, open Konsole (CTL+ALT+T) and type ```adb devices``` which returns on the first line ```* deamon started successfully``` and on another line an 8 digit *string* with the serial number of the device.
3. To uninstall the .apk itself you must type the command ``adb shell pm uninstall --user 0 (.apk name)``. For example, to uninstall google assist type ``adb shell pm uninstall --user 0 com.google.android.googlequickserchbox``. The command returns on the line below the word "Success". In reality the .apk is still there but it is not available to user 0 (that's us) and therefore will not be executable. 

### What if we need to reinstall some of those apps?

The uninstall can be reversed with the *install r* command. First we need to find the location of the install file (the .apk). Just type ```adb shell pm dump com.google.android.<app-name> | grep Path``` which returns the path of the .apk and then we can reinstall it by typing ```adb shell pm install -r --user 0 /absolute/path/to/file/app-name_v0.0.0.apk (pseudo code)```. For example, if we want to continue to be able to install / uninstall / reinstall .apks from within the phone itself we have to reinstall one of the apps that are in the list below. First, we have to search for it by typing *adb shell pm dump com.google.android.packageinstaller | grep Path* which returns these two lines:
```
      codePath=/system/priv-app/GooglePackageInstaller
      resourcePath=/system/priv-app/GooglePackageInstaller
```
to reinstall we type *adb shell pm install -r --user 0 /system/priv-app/GooglePackageInstaller* which returns:
```
/com.google.android.packgeinstaller-1.apk pkg: /system/priv-app/GooglePackageInstaller
Success
```

### List (with comments) of the 49 *.apk* to uninstall (.apk that contain trackers and can be replaced by better apps) and other useful commands

```
  adb devices //initialize communication with the device
  adb --help //help on all adb commands
  adb shell pm list permission-groups //view device permission groups
  adb shell pm list users //list users on the device
  adb shell pm get-install-location //see where apk is being installed
  adb shell pm list packages // list all apk installed for the user
  //commands for all .apk to uninstall (with some comments). Upon pressing ENTER and executing each command below, the program should return "Success":
   adb shell pm uninstall --user 0 com.google.android.onetimeinitializer
   adb shell pm uninstall --user 0 com.google.android.googlequicksearchbox //the google assistant that's always listening
   adb shell pm uninstall --user 0 com.google.android.marvin.talkback //the one that lets google listen to you
   adb shell pm uninstall --user 0 com.google.android.syncadapters.contacts //sends your contacts to google
   adb shell pm uninstall --user 0 com.google.android.apps.messaging
   adb shell pm uninstall --user 0 com.google.android.apps.docs
   adb shell pm uninstall --user 0 com.google.android.gsf.login
   adb shell pm uninstall --user 0 com.google.android.gsf
   adb shell pm uninstall --user 0 com.android.chrome
   adb shell pm uninstall --user 0 com.google.android.calendar
   adb shell pm uninstall --user 0 com.google.android.vending
   adb shell pm uninstall --user 0 com.google.android.gms
   adb shell pm uninstall --user 0 com.google.android.gm
   adb shell pm uninstall --user 0 com.google.android.youtube
   adb shell pm uninstall --user 0 com.google.android.youtube
   adb shell pm uninstall --user 0 com.google.android.apps.maps
   adb shell pm uninstall --user 0 com.google.android.apps.photos
   adb shell pm uninstall --user 0 com.google.android.talk
   adb shell pm uninstall --user 0 com.google.android.music
   adb shell pm uninstall --user 0 com.google.android.videos
   adb shell pm uninstall --user 0 com.google.android.feedback
   adb shell pm uninstall --user 0 com.google.android.webview
   adb shell pm uninstall --user 0 com.google.android.apps.docs.oem
   adb shell pm uninstall --user 0 com.google.android.tts
   adb shell pm uninstall --user 0 com.google.android.configupdater
   adb shell pm uninstall --user 0 com.google.android.partnersetup
   adb shell pm uninstall --user 0 com.google.android.packageinstaller //!Warning this is the app that allows you to manage .apks directly on android. See above how to reinstall it in case we need to manage apps without adb access. It has no trackers so we may not uninstall this one!
   adb shell pm uninstall --user 0 com.google.driveactivator
   adb shell pm uninstall --user 0 com.android.cellbroadcastreceiver //allow the government via isp to send you propaganda messages
   adb shell pm uninstall --user 0 com.google.android.backuptransport
   adb shell pm uninstall --user 0 com.android.vending
   adb shell pm uninstall --user 0 com.amazon.kindle //what is this doing on a phone?
   adb shell pm uninstall --user 0 com.tripadvisor.tripadvisor //what is this doing on a phone?
   adb shell pm uninstall --user 0 com.oma.drm //Censorship app which may prevent you from reading file content (not gnu/copyleft compliant and tries to impose copyright censorship)
   adb shell pm uninstall --user 0 com.asus.dm //tries to download system updates (+-2GB) every time you enable a data connections: resembles a DDOS attack
   adb shell pm uninstall --user 0 com.ironsource.appcloud.oobe.asus
   adb shell pm uninstall --user 0 com.asus.ia.asusapp
   adb shell pm uninstall --user 0 com.asus.weathertime
   adb shell pm uninstall --user 0 com.asus.gallery
   adb shell pm uninstall --user 0 com.asus.flashlight
   adb shell pm uninstall --user 0 com.asus.ephotoburst
   adb shell pm uninstall --user 0 com.asus.userfeedback
   adb shell pm uninstall --user 0 com.asus.calculator
   adb shell pm uninstall --user 0 com.asus.easylauncher
   adb shell pm uninstall --user 0 com.asus.filemanager
   adb shell pm uninstall --user 0 com.asus.soundrecorder
   adb shell pm uninstall --user 0 com.asus.ime
   adb shell pm uninstall --user 0 com.asus.as //asus analytics
   adb shell pm uninstall --user 0 com.asus.contacts.theme.dark
   adb shell pm uninstall --user 0 com.asus.contacts //allows you to manage SIM contacts (useful if you have to use a dumbphone) but makes your .vcf contacts almost unusable by making *merges* that hide duplicate contacts (and there are alternative apps for managing SIM contacts)
   adb shell pm uninstall --user 0 com.asus.asusincallui //hi-jacks the dialer app
   adb shell pm uninstall --user 0 com.asus.camera
   adb shell pm uninstall --user 0 com.asus.deskclock
   adb shell pm uninstall --user 0 com.asus.systemupdate
   adb shell pm uninstall --user 0 com.asus.zentalk
   adb shell pm uninstall --user 0 com.qualcomm.location.XT //to use gps you only need com.qualcomm.location not the XT that keeps running even inside buildings mapping where you are and allowing triangulation plus every time you start gps it asks permission yhus nagging you until you say yes (dark design pattern)
  adb shell pm trim-caches //delete all applications cache
  adb shell pm reboot //restart the device

```
Once the undesirable applications have been removed, they need to be replaced. Here is the list, in alphabetical order, of the **FOSS** apps (source code available for anyone to scrutinize and improve) and **no trackers** to make your phone a user-friendly Swiss army knife:

```

* [Activity Launcher](https://search.f-droid.org/?q=de.szalkowski.activitylauncher) 
* [AF Weather](https://search.f-droid.org/?q=net.gitsaibot.af) 
* [Anstop](https://search.f-droid.org/?q=An.stop) 
* [APK Export](https://apkpure.com/search?q=com.ses.app.apkexport) 
* [App Lock](https://search.f-droid.org/?q=io.github.subhamtyagi.privacyapplock) 
* [App Manager](https://search.f-droid.org/?q=io.github.muntashirakon.AppManager) 
* [Arcticons Dark](https://search.f-droid.org/?q=com.donnnno.arcticons) 
* [Audio Recorder](https://search.f-droid.org/?q=com.github.axet.audiorecorder) 
* [Battery Notifier BT Free](https://apkpure.com/search?q=com.larryvgs.battery) 
* [BikeComputer](https://search.f-droid.org/?q=de.nulide.bikecomputer) 
* [Birthday Buddy](https://search.f-droid.org/?q=com.procrastimax.birthdaybuddy) 
* [BirthDayDroid](https://search.f-droid.org/?q=com.tmendes.birthdaydroid) 
* [Blacklist Blocker](https://search.f-droid.org/?q=com.kaliturin.blacklist) 
* [Boldbeast Recorder](https://apkpure.com/search?q=com.boldbeast.recorder) 
* [Browsor](https://apkpure.com/search?q=com.trim.web.browser) 
* [Bubble](https://search.f-droid.org/?q=org.woheller69.level) 
* [CalcTastic](https://apkpure.com/search?q=com.shaytasticsoftware.calctasticfree) 
* [Calendar](https://search.f-droid.org/?q=com.simplemobiletools.calendar.pro) 
* [Calendar Import-Export](https://search.f-droid.org/?q=org.sufficientlysecure.ical) 
* [Caller Name Announcer](https://apkpure.com/search?q=com.caller.sms.announcer) 
* [CallerDetails](https://search.f-droid.org/?q=com.wordpress.sarfraznawaz.callerdetails) 
* [Camera Color Picker](https://search.f-droid.org/?q=fr.tvbarthel.apps.cameracolorpicker) 
* [Camera Roll](https://search.f-droid.org/?q=us.koller.cameraroll) 
* [ChronoSnap](https://search.f-droid.org/?q=com.nathanosman.chronosnap) 
* [ClassyShark3xodus](https://search.f-droid.org/?q=com.oF2pks.classyshark3xodus) 
* [Clear List](https://search.f-droid.org/?q=douzifly.list) 
* [Clima](https://search.f-droid.org/?q=co.prestosole.clima) 
* [ClipboardCleaner](https://search.f-droid.org/?q=io.github.deweyreed.clipboardcleaner) 
* [Contacts](https://search.f-droid.org/?q=com.simplemobiletools.contacts.pro) 
* [Contacts On Desktop](https://apkpure.com/search?q=contacts%20on%20desktop) 
* [Countdown for DashClock](https://search.f-droid.org/?q=com.cr5315.cfdc) 
* [Counter](https://search.f-droid.org/?q=me.tsukanov.counter) 
* [DashClock Battery Extension](https://apkpure.com/dashclock-battery-extension/me.grantland.dashclock_battery) 
* [DashClock Birthday Extension](https://search.f-droid.org/?q=fr.nicopico.dashclock.birthday) 
* [DashClock custom extension](https://apkpure.com/dashclock-custom-extension/com.fleckdalm.dashclockcustomextension) 
* [DashClock Device Info Extension](https://apkpure.com/dashclock-deviceinfo-extension/com.pixelus.dashclock.ext.mydevice) 
* [DashClock Disk Space Extension](https://search.f-droid.org/?q=com.pixelus.dashclock.ext.mystorage) 
* [DashClock Week Number](https://search.f-droid.org/?q=de.jonaskoeritz.android.dashclockweeknumber) 
* [DashClock Widget](https://search.f-droid.org/?q=net.nurik.roman.dashclock) 
* [DAVx⁵](https://search.f-droid.org/?q=at.bitfire.davdroid) 
* [Dialer](https://search.f-droid.org/?q=com.simplemobiletools.dialer) 
* [Dicionário Priberam](https://apkpure.com/search?q=pt.priberam.dicionariolinguaportuguesa) 
* [Disable Manager](https://search.f-droid.org/?q=com.nagopy.android.disablemanager2) 
* [Document Viewer](https://search.f-droid.org/?q=org.sufficientlysecure.viewer) 
* [Dumbphone Assistant](https://search.f-droid.org/?q=com.github.yeriomin.dumbphoneassistant) 
* [Editor](https://search.f-droid.org/?q=org.billthefarmer.editor) 
* [Element](https://search.f-droid.org/?q=im.vector.app) 
* [Emerald Dialer](https://search.f-droid.org/?q=ru.henridellal.dialer) 
* [FairEmail](https://search.f-droid.org/?q=eu.faircode.email) 
* [Fake Contacts](https://search.f-droid.org/?q=me.billdietrich.fake_contacts) 
* [Falling Blocks](https://search.f-droid.org/?q=de.stefan_oltmann.falling_blocks) 
* [Feeder](https://search.f-droid.org/?q=com.nononsenseapps.feeder) 
* [Fennec](https://search.f-droid.org/?q=org.mozilla.fennec_fdroid) 
* [FillUp](https://search.f-droid.org/?q=com.github.wdkapps.fillup) 
* [ForRunners](https://search.f-droid.org/?q=net.khertan.forrunners) 
* [Foxy Droid](https://search.f-droid.org/?q=nya.kitsunyan.foxydroid) 
* [Fritter](https://search.f-droid.org/?q=com.jonjomckay.fritter) 
* [GCompris](https://search.f-droid.org/?q=net.gcompris.full) 
* [Hourly Reminder](https://search.f-droid.org/?q=com.github.axet.hourlyreminder) 
* [Identiconizer!](https://search.f-droid.org/?q=com.germainz.identiconizer) 
* [JABtalk](https://apt.izzysoft.de/fdroid/index/apk/com.jabstone.jabtalk.basic) 
* [JustChess](https://search.f-droid.org/?q=com.alaskalinuxuser.justchess) 
* [KDE Connect](https://search.f-droid.org/?q=org.kde.kdeconnect_tp) 
* [KeePassDX](https://search.f-droid.org/?q=com.kunzisoft.keepass.libre) 
* [Kids Solar Explorer](https://search.f-droid.org/?q=com.apesoup.KidsSolarExplorer) 
* [KISS launcher](https://search.f-droid.org/?q=fr.neamar.kiss) 
* [LastCaller](https://search.f-droid.org/?q=com.dwak.lastcall) 
* [Lawnchair](https://search.f-droid.org/?q=ch.deletescape.lawnchair.plah) 
* [LibGen](https://search.f-droid.org/?q=com.manuelvargastapia.libgen) 
* [List My Apps](https://search.f-droid.org/?q=de.onyxbits.listmyapps) 
* [Lockeye](https://apkpure.com/search?q=com.tafayor.lockeye2) 
* [LTE Cleaner](https://search.f-droid.org/?q=theredspy15.ltecleanerfoss) 
* [Markers](https://search.f-droid.org/?q=org.dsandler.apps.markers) 
* [Material Files](https://search.f-droid.org/?q=me.zhanghai.android.files) 
* [Material Tea Timer](https://search.f-droid.org/?q=org.ligi.materialteatimer) 
* [MidiSheetMusic](https://search.f-droid.org/?q=com.midisheetmusic) 
* [MSnake](https://search.f-droid.org/?q=com.fgrim.msnake) 
* [My Contacts](https://search.f-droid.org/?q=community.fairphone.mycontacts) 
* [Natural Hour](https://github.com/forrestguice/NaturalHour/releases) 
* [Nekogram X](https://search.f-droid.org/?q=nekox.messenger) 
* [Net Monitor](https://search.f-droid.org/?q=org.secuso.privacyfriendlynetmonitor) 
* [Night Screen](https://search.f-droid.org/?q=info.papdt.blackblub) 
* [ObscuraCam](https://search.f-droid.org/?q=org.witness.sscphase1) 
* [Oburo.O](https://search.f-droid.org/?q=fr.s3i.pointeuse) 
* [OctoDroid](https://search.f-droid.org/?q=com.gh4a) 
* [On Call End](https://search.f-droid.org/?q=com.brosix.callend) 
* [OneTwo](https://search.f-droid.org/?q=com.nicue.onetwo) 
* [Open Camera](https://search.f-droid.org/?q=net.sourceforge.opencamera) 
* [Open FlashLight](https://search.f-droid.org/?q=io.github.sanbeg.flashlight) 
* [OpenTimer](https://search.f-droid.org/?q=edu.killerud.kitchentimer) 
* [OpenTracks](https://search.f-droid.org/?q=de.dennisguse.opentracks) 
* [Orbot](https://github.com/guardianproject/orbot/releases) 
* [Oscilloscope](https://search.f-droid.org/?q=org.billthefarmer.scope) 
* [OsmAnd Contour lines](https://search.f-droid.org/?q=net.osmand.srtmPlugin.paid) 
* [OsmAnd Parking](https://search.f-droid.org/?q=net.osmand.parkingPlugin) 
* [OsmAnd~](https://search.f-droid.org/?q=net.osmand.plus) 
* [ownCloud](https://search.f-droid.org/?q=com.owncloud.android) 
* [Pace+](https://search.f-droid.org/?q=appinventor.ai_AERvanHouten.PacePlus) 
* [Papirus Icon Pack](https://www.pling.com/p/1662847/) 
* [Password Generator](https://search.f-droid.org/?q=com.vecturagames.android.app.passwordgenerator) 
* [PDF Converter](https://search.f-droid.org/?q=swati4star.createpdf) 
* [PilferShush Jammer](https://search.f-droid.org/?q=cityfreqs.com.pilfershushjammer) 
* [QR & Barcode Scanner](https://search.f-droid.org/?q=com.example.barcodescanner) 
* [Radian Degree Converter Calc](https://search.f-droid.org/?q=theteendevs.radiandegreeconvertercalc) 
* [RadioDroid](https://search.f-droid.org/?q=net.programmierecke.radiodroid2) 
* [RAR](https://www.rarlab.com/download.htm) 
* [Retroboy](https://search.f-droid.org/?q=se.embargo.retroboy) 
* [RHVoice](https://search.f-droid.org/?q=com.github.olga_yakovleva.rhvoice.android) 
* [Rule of Three](https://search.f-droid.org/?q=de.steinpfeffer.rdt) 
* [RxDroid](https://search.f-droid.org/?q=at.jclehner.rxdroid) 
* [ScreenCam](https://search.f-droid.org/?q=com.orpheusdroid.screenrecorder) 
* [Screenshot Blocker](https://apkpure.com/screenshot-blocker-prevent-screenshots/com.tafayor.screenshotblocker) 
* [Semitone](https://search.f-droid.org/?q=mn.tck.semitone) 
* [Shortcuts for Calendar/Contacts](https://search.f-droid.org/?q=org.shortcuts) 
* [Silence](https://search.f-droid.org/?q=org.smssecure.smssecure) 
* [Simple Keyboard](https://search.f-droid.org/?q=rkr.simplekeyboard.inputmethod) 
* [SkyTube Extra](https://apt.izzysoft.de/fdroid/index/apk/free.rm.skytube.extra) 
* [Solar Compass](https://search.f-droid.org/?q=com.agnibho.android.solarcompass) 
* [Solunar Periods](https://github.com/forrestguice/SolunarPeriods/releases) 
* [Sound Recorder](https://search.f-droid.org/?q=net.micode.soundrecorder) 
* [SpeedCrunch](https://search.f-droid.org/?q=org.syrja.speedcrunch) 
* [Stellarium](https://search.f-droid.org/?q=com.noctuasoftware.stellarium_free) 
* [Stopwatch](https://search.f-droid.org/?q=com.keuwl.stopwatch) 
* [Stopwatch](https://search.f-droid.org/?q=com.kodarkooperativet.notificationstopwatch) 
* [Suntimes](https://search.f-droid.org/?q=com.forrestguice.suntimeswidget) 
* [Suntimes Calendars](https://search.f-droid.org/?q=com.forrestguice.suntimescalendars) 
* [SuperFreezZ](https://search.f-droid.org/?q=superfreeze.tool.android) 
* [Tic Tac Toe](https://search.f-droid.org/?q=com.earthblood.tictactoe) 
* [Time Calc](https://search.f-droid.org/?q=com.noxproductions.TimeCalc) 
* [TimeTable](https://search.f-droid.org/?q=com.asdoi.timetable) 
* [Tor Browser](https://search.f-droid.org/?q=org.torproject.torbrowser) 
* [Trail Sense](https://search.f-droid.org/?q=com.kylecorry.trail_sense) 
* [TTS Util](https://search.f-droid.org/?q=com.danefinlay.ttsutil) 
* [Vector Pinball](https://search.f-droid.org/?q=com.dozingcatsoftware.bouncy) 
* [VLC](https://search.f-droid.org/?q=org.videolan.vlc) 
* [Voice Notify](https://search.f-droid.org/?q=com.pilot51.voicenotify) 
* [WateryDroid](https://search.f-droid.org/?q=tmendes.com.waterydroid) 
* [Wi-Fi Privacy Police](https://search.f-droid.org/?q=be.uhasselt.privacypolice) 
* [Wi-Fi Widget](https://search.f-droid.org/?q=org.androidappdev.wifiwidget) 
* [World Clock & Weather](https://search.f-droid.org/?q=ch.corten.aha.worldclock) 
* [World Weather](https://search.f-droid.org/?q=com.haringeymobile.ukweather) 

List made using [List My Apps](https://search.f-droid.org/?q=de.onyxbits.listmyapps)
```

* [Orbot](https://github.com/guardianproject/orbot/releases) 

* [Oscilloscope](https://search.f-droid.org/?q=org.billthefarmer.scope) 

* [OsmAnd Contour lines](https://search.f-droid.org/?q=net.osmand.srtmPlugin.paid) 

* [OsmAnd Parking](https://search.f-droid.org/?q=net.osmand.parkingPlugin) 

* [OsmAnd~](https://search.f-droid.org/?q=net.osmand.plus) 

* [ownCloud](https://search.f-droid.org/?q=com.owncloud.android) 

* [Pace+](https://search.f-droid.org/?q=appinventor.ai_AERvanHouten.PacePlus) 

* [Papirus Icon Pack](https://www.pling.com/p/1662847/) 

* [Password Generator](https://search.f-droid.org/?q=com.vecturagames.android.app.passwordgenerator) 

* [PDF Converter](https://search.f-droid.org/?q=swati4star.createpdf) 

* [PilferShush Jammer](https://search.f-droid.org/?q=cityfreqs.com.pilfershushjammer) 

* [QR & Barcode Scanner](https://search.f-droid.org/?q=com.example.barcodescanner) 

* [Radian Degree Converter Calc](https://search.f-droid.org/?q=theteendevs.radiandegreeconvertercalc) 

* [RadioDroid](https://search.f-droid.org/?q=net.programmierecke.radiodroid2) 

* [RAR](https://www.rarlab.com/download.htm) 

* [Retroboy](https://search.f-droid.org/?q=se.embargo.retroboy) 

* [RHVoice](https://search.f-droid.org/?q=com.github.olga_yakovleva.rhvoice.android) 

* [Rule of Three](https://search.f-droid.org/?q=de.steinpfeffer.rdt) 

* [RxDroid](https://search.f-droid.org/?q=at.jclehner.rxdroid) 

* [ScreenCam](https://search.f-droid.org/?q=com.orpheusdroid.screenrecorder) 

* [Screenshot Blocker](https://apkpure.com/screenshot-blocker-prevent-screenshots/com.tafayor.screenshotblocker) 

* [Semitone](https://search.f-droid.org/?q=mn.tck.semitone) 

* [Shortcuts for Calendar/Contacts](https://search.f-droid.org/?q=org.shortcuts) 

* [Silence](https://search.f-droid.org/?q=org.smssecure.smssecure) 

* [Simple Keyboard](https://search.f-droid.org/?q=rkr.simplekeyboard.inputmethod) 

* [SkyTube Extra](https://apt.izzysoft.de/fdroid/index/apk/free.rm.skytube.extra) 

* [Solar Compass](https://search.f-droid.org/?q=com.agnibho.android.solarcompass) 

* [Solunar Periods](https://github.com/forrestguice/SolunarPeriods/releases) 

* [Sound Recorder](https://search.f-droid.org/?q=net.micode.soundrecorder) 

* [SpeedCrunch](https://search.f-droid.org/?q=org.syrja.speedcrunch) 

* [Stellarium](https://search.f-droid.org/?q=com.noctuasoftware.stellarium_free) 

* [Stopwatch](https://search.f-droid.org/?q=com.keuwl.stopwatch) 

* [Stopwatch](https://search.f-droid.org/?q=com.kodarkooperativet.notificationstopwatch) 

* [Suntimes](https://search.f-droid.org/?q=com.forrestguice.suntimeswidget) 

* [Suntimes Calendars](https://search.f-droid.org/?q=com.forrestguice.suntimescalendars) 

* [SuperFreezZ](https://search.f-droid.org/?q=superfreeze.tool.android) 

* [Tic Tac Toe](https://search.f-droid.org/?q=com.earthblood.tictactoe) 

* [Time Calc](https://search.f-droid.org/?q=com.noxproductions.TimeCalc) 

* [TimeTable](https://search.f-droid.org/?q=com.asdoi.timetable) 

* [Tor Browser](https://search.f-droid.org/?q=org.torproject.torbrowser) 

* [Trail Sense](https://search.f-droid.org/?q=com.kylecorry.trail_sense) 

* [TTS Util](https://search.f-droid.org/?q=com.danefinlay.ttsutil) 

* [Vector Pinball](https://search.f-droid.org/?q=com.dozingcatsoftware.bouncy) 

* [VLC](https://search.f-droid.org/?q=org.videolan.vlc) 

* [Voice Notify](https://search.f-droid.org/?q=com.pilot51.voicenotify) 

* [WateryDroid](https://search.f-droid.org/?q=tmendes.com.waterydroid) 

* [Wi-Fi Privacy Police](https://search.f-droid.org/?q=be.uhasselt.privacypolice) 

* [Wi-Fi Widget](https://search.f-droid.org/?q=org.androidappdev.wifiwidget) 

* [World Clock & Weather](https://search.f-droid.org/?q=ch.corten.aha.worldclock) 

* [World Weather](https://search.f-droid.org/?q=com.haringeymobile.ukweather) 



List made using [List My Apps](https://search.f-droid.org/?q=de.onyxbits.listmyapps)



A note about the assistance app! We have uninstalled Google's *assist app* as it is too invasive of one's privacy as it listens and reports everything we say and also everything we type (if we use it together with the factory-shipped keyboard app, our typing history would also be sent to the manufacturer; in this case ASUS) under the pretext of "assisting" us and "improving (err... acknowledge!) the user experience". We should therefore replace it with one that has the possibility to work offline only. Once we have installed the [KISS launcher](https://search.f-droid.org/?q=fr.neamar.kiss) we can use it not as the default launcher (for which I use [Lawnchair](https://search.f-droid.org/?q=ch.deletescape.lawnchair.plah) but as an *assist app*. We can choose what we want the app to find for us: I told it to search only for a) contacts, b) files and c) apps (but it's also possible to include *web search* and choose the search engine); I also keep the *history* *frozen* so as not to exponentially increase the app's database. To use KISS as an assistance app: go to *settings* > *apps* > *icon cogwheel* > *default apps* > *assist app* > and choose KISS Launcher from the list.

Now we can insert our cards into the device and configure it to our needs. The above list exhaustively covers the features of all the .apks we just uninstalled, and adds some more features respective to my personal requirements at the time of writing this tutorial; despite that eventually some people will not require so many apps while others may require more. The FOSS repositories are full of good applications that we can test.

Glossary:
* adb = android debug bridge
* apk = android package
* shell = command line that communicates with operating system services
* pm = package manager
