# Como desgooglizar, aumentar a segurança e melhorar a privacidade e a usabilidade do Asus Zenfone via ADB

Neste artigo vamos aprender a *desgooglizar* o smartphone e a desinstalar todo o *bloatware* que vem de fábrica sem comprometer funcionalidades do dispositivo. Para isso utilizaremos o comando ADB (android debug bridge) no Linux.
Quando compramos um smart-phone android, em teoria ficamos com um canivete suíço para o dia-a-dia, mas na prática, como o compramos à *electrónica de consumo* o dispositivo traz tantos *trackers* de fábrica que nunca conseguiremos tirar partido do seu máximo potencial a menos que façamos algumas alterações. Como as firmas que os produzem continuam a priorizar não a experiência e a liberdade do utilizador, mas antes o modelo de *publicidade direccionada* que obriga ao rastreamento do comportamento dos utilizadores, a miríade de trackers que essa opção acarreta, torna o dispositivo lento e a sua utilização exasperante, frequentemente obrigando o dispositivo a consumir mais memória, mais bateria e, caso decidamos tomar medidas para contrariar esse comportamento abusivo, usando *disable* ou *freeze*, causando *loops* que dificultarão o acesso à rede. A única opção é impedir que essas apps sejam executadas sem o nosso consentimento, deixando assim de competir com as apps que pretendemos usar. Porque se trata de apps instaladas a nível do sistema, *Uninstall* é o único comando eficaz para as parar. Podemos então ver-mo-nos livres desses .apk que apenas servem interesses de terceiros, não fazendo nada pelo utilizador-comprador, que é quem detém o legítimo direito de utilizar-se do dispositivo como e apenas como entender. Para usufruirmos do canivete suíço por cuja expectativa prescindimos do nosso dinheiro temos de limpar o dispositivo que acabámos de comprar! Sim é a triste verdade. Podemos pagar mais por alternativas *mais* limpas de fábrica como o [Volla Phone](https://volla.online/en/) ou o [Murena Fairphone 3+ – eSolutions – deGoogled phones and services](https://esolutions.shop/shop/murena-fairphone-3-plus-fr/) ou então limpar nós próprios o telefone via adb. Basta seguir este manual.

Requisitos prévios: ao ligar o telefone pela primeira vez não devemos ter qualquer cartão SIM com plano de dados ou cartão de memória inserido no dispositivo e, apesar da app Google nos pedir isso, não devemos permitir que o dispositivo se ligue a quaisquer redes. E, claro, não devemos introduzir nenhum tipo de credenciais ou número de telefone em app alguma *de sistema*. Não clicar *sim* nem *aceito* nem *concordo* com quaisquer perguntas que a aplicação Google nos faça. Vamos dizendo *não* e *configurar "mais tarde"* até que consigamos sair da aplicação. Fechar a aplicação.
Para ter acesso às ferramentas de programador que estão *escondidas* no android ir até *settings* &gt; *about* &gt; *software information* &gt; e clicar repetidamente sete vezes em *build number*. Receberemos uma mensagem *toast* a dizer *"congratulations! You are now a programmer"*. A partir daí na ante-penúltima entrada das *settings* deveremos ler *developer options*. Então vamos lá!

1. No telefone, habilitar o *usb debugging* nas > *settings* > *developper options*. É aconselhável também seleccionar o *stay awake*. No meu caso abri a [appManager](https://github.com/MuntashirAkon/AppManager), pesquisei pela app a desinstalar e mantive a app aberta para verificar que ao executar o comando adb no pc, a app *desaparecia* efectivamente do ecrã do smartphone (deixava de estar disponível para o utilizador 0).
2. No Linux abrir a linha de comandos, no caso do Kubuntu, o Konsole e digitar ```adb devices``` que devolve na primeira linha ```* deamon started successfully``` e noutra linha uma *string* de 8 dígitos com o número de série do dispositivo.
3. Para desinstalar os .apk propriamente ditos há que digitar o comando ```adb shell pm uninstall --user 0 (.apk name)```. Por exemplo, para desinstalar o google assist digitar ```adb shell pm uninstall --user 0 com.google.android.googlequickserchbox```. O comando devolve na linha abaixo a palavra "Success". Na realidade o .apk continua lá mas não fica disponível para o utilizador 0 (que somos nós) e portanto não será executável. 

### E se precisarmos de reinstalar algumas dessas apps?

A desinstalação pode ser revertida com o comando _install r_. Primeiro temos de encontrar a localização do ficheiro de instalação (o .apk). Para isso digitamos ```adb shell pm dump com.google.android.<nome-da-app> | grep Path``` que devolve o caminho do .apk e depois pode-se reinstalá-lo digitando ```adb shell pm install -r --user 0 /absolute/path/to/file/nome_da_app_v0.0.0.apk (pseudo code)```. Exemplicando, se quisermos continuar a poder instalar / desinstalar / reinstalar .apks a partir do próprio telefone temos de reinstalar uma das apps que estão na lista abaixo. Primeiro, temos de a procurar digitando *adb shell pm dump com.google.android.packageinstaller | grep Path* que devolve estas duas linhas:
```
      codePath=/system/priv-app/GooglePackageInstaller
      resourcePath=/system/priv-app/GooglePackageInstaller
```
para reinstalar digitamos *adb shell pm install -r --user 0 /system/priv-app/GooglePackageInstaller* que devolve:
```
/com.google.android.packgeinstaller-1.apk       pkg: /system/priv-app/GooglePackageInstaller
Success
```

### Lista (comentada) dos 49 *.apk* a desinstalar (.apk que contêm trackers e podem ser substituídos por apps melhores) e de comandos úteis

```
  adb devices //inicializa a comunicação com o dispositivo
  adb --help //ver ajuda sobre todos os comandos adb
  adb shell pm list permission-groups //ver grupos de permissões do dispositivo
  adb shell pm list users //listar utilizadores do dispositivo
  adb shell pm get-install-location //ver onde é que os apk estão a ser instalados
  adb shell pm list packages // lista todos os apk instalados para o utilizador
  //comandos para todos os .apk a desinstalar (com alguns comentários). Ao premir ENTER e executar cada comando abaixo, o programa deverá devolver "Sucess":
   adb shell pm uninstall --user 0 com.google.android.onetimeinitializer
   adb shell pm uninstall --user 0 com.google.android.googlequicksearchbox //o google assist que está sempre à escuta
   adb shell pm uninstall --user 0 com.google.android.marvin.talkback //o que permite que a google te ouça
   adb shell pm uninstall --user 0 com.google.android.syncadapters.contacts //envia os teus contactos para a google
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
   adb shell pm uninstall --user 0 com.google.android.packageinstaller //!Atenção esta é a app que permite gerir .apks directamente no android. Ver acima como a reinstalar caso necessitemos de gerir apps sem acesso a adb. Não tem trackers pelo que podemos não desinstalar esta!
   adb shell pm uninstall --user 0 com.google.driveactivator
   adb shell pm uninstall --user 0 com.android.cellbroadcastreceiver //permite que o governo via isp te envie mensagens de propaganda
   adb shell pm uninstall --user 0 com.google.android.backuptransport
   adb shell pm uninstall --user 0 com.android.vending
   adb shell pm uninstall --user 0 com.amazon.kindle //o que é que isto está a fazer num telefone??
   adb shell pm uninstall --user 0 com.tripadvisor.tripadvisor //o que é que isto está a fazer num telefone??
   adb shell pm uninstall --user 0 com.oma.drm //app de censura que te pode impedir de ler conteúdo de ficheiros (não respeita gnu/copyleft e tenta impôr censura escorada no copyright)
   adb shell pm uninstall --user 0 com.asus.dm //tenta fazer download do sistema (+-2GB) de cada vez que ligas os dados: assemelha-se a um ataque DDOS
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
   adb shell pm uninstall --user 0 com.asus.contacts //permite gerir contactos SIM (útil se tivermos de usar um dumbphone) mas torna o nosso .vcf de contactos praticamente inutilizável fazendo *merges* que escondem os contactos duplicados (e há apps alternativas para isso)
   adb shell pm uninstall --user 0 com.asus.asusincallui //faz um hi-jacking do dialer
   adb shell pm uninstall --user 0 com.asus.camera
   adb shell pm uninstall --user 0 com.asus.deskclock
   adb shell pm uninstall --user 0 com.asus.systemupdate
   adb shell pm uninstall --user 0 com.asus.zentalk
   adb shell pm uninstall --user 0 com.qualcomm.location.XT //para usares o gps precisas só de com.qualcomm.location essa XT fica sempre em excução mesmo dentro de edifícios mapeando o sítio onde te encontras e permitindo a triangulação além disso de cada vez que inicias o gps ela pede permissão para te chatear até que digas que sim (dark design pattern)
  adb shell pm trim-caches //apagar a cache das aplicações
  adb shell pm reboot //reiniciar o dispositivo

```
Uma vez removidas as aplicações indesejáveis é preciso substituí-las. Fica aqui a lista, por ordem alfabética, das apps **FOSS** (código-fonte disponível para escrutínio e melhoramentos por qualquer pessoa) e **sem trackers** para tornar o telefone um canivete suíço amigo do utilizador:

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
Uma nota sobre a app de assistência! Desinstalámos a *assist app* da Google por ser demasiado invasiva da nossa privacidade uma vez que ouve e reporta tudo o que dissermos e também tudo o que escrevermos (se a usássemos em conjunto com a aplicação de teclado que vem de fábrica, o nosso histórico de digitação seria também enviado para o fabricante; neste caso a ASUS) com o pretexto de nos «assistir» e «melhorar (err... conhecer!) a experiência do utilizador». Convém pois que a substituamos por uma que tenha a possibilidade de funcionar apenas offline. Uma vez que instalámos o [KISS launcher](https://search.f-droid.org/?q=fr.neamar.kiss) podemos utilizá-lo não como o default launcher (para isso eu uso o [Lawnchair](https://search.f-droid.org/?q=ch.deletescape.lawnchair.plah) mas como *assist app*. Podemos escolher o que queremos que a app encontre para nós: eu disse-lhe para procurar apenas em a) contactos, b) ficheiros e c) aplicações (mas também é possível incluirmos *web search* e escolhermos o motor de busca); fiz também *freeze* à *history* para não aumentar exponencialmente a base de dados da aplicação. Para usar o KISS como app de assistência: ir até *settings* &gt; *apps* &gt; *ícone roda dentada* &gt; *default apps* &gt; *assist app* &gt; e escolher da lista o KISS Launcher.

Agora já podemos inserir os nossos cartões no dispositivo e configurá-lo para as nossas necessidades. A lista acima contempla exaustivamente as funcionalidades de todos os .apks que acabámos de desinstalar, e acrescenta mais algumas funcionalidades respectivas às minhas necessidades pessoais no momento em que escrevi este tutorial; salvaguarda-se que eventualmente algumas pessoas não precisarão de tantas aplicações enquanto que outras precisarão de mais. Os repositórios FOSS estão repletos de boas aplicações que podemos testar.

Glossário:
* adb = android debug bridge
* apk = android package
* shell = linha de comandos que comunica com os serviços do sistema operativo
* pm = package manager
