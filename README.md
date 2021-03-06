# PeimariSplitAnalyzer

PeimariSplitAnalyzer aka "köyhän miehen winsplits" uses "client side" to do analysis for [orienteering](http://en.wikipedia.org/wiki/Orienteering) split times.

This application was originally created some years ago for Peimarin Rastit when changing result system to EResults and at the same time making larger overall rework for Peimarin Rastit site. The site admin app was created with [Vaadin](https://vaadin.com/), but in this part I had to go with pure GWT (or 'client side Vaadin
) to be able to use existing cheap "php hosting". The app eats either IOF-XML files or jaxb xml files generated by my "iofdomain" project (shared with related the server side admin app). Since then it has gained support to analyze also other split time files, as cross domain hosted and even as a bookmarklet.

Hosted app for non technical users, without any kind of warranty, at: http://v3.tahvonen.fi/splitanalyzer/ 

## GWT tricks in this app

Although the app is as such quite low quality piece of software (shame on me), it demonstrates some advanced GWT tricks. The project is built with GWT cross site (iframe) linker so it can be served from third party domain as well. In addition to that, this paves way for two cool features:

 * [Dynamically injecting GWT module](https://github.com/mstahv/splitanalyzer/blob/master/src/main/webapp/pirila.js) into host page may be handy in large GWT modules that are not always needed to save initially required bandwidth. The linked script just adds button that will then again load the actual GWT stuff when clicked. Note, that bit similar effect (but at later phase when some GWT stuff is already loaded) can also be achieved with [Code Splitting](http://www.gwtproject.org/doc/latest/DevGuideCodeSplitting.html).
 * [Using a GWT app as bookmarklet](https://github.com/mstahv/splitanalyzer/blob/master/src/main/webapp/index.html#L71). See also [the example video](http://youtu.be/J6RWwKYd7aM).
 * Injecting JavaScript libraries with XSIframeLinker. `XSIFrameLinker` don't support commonly used "&lt;script&gt;" tags in ".gwt.xml" files. Instead one can use `ResourceBundle`s and `ScriptInjector`
 * Using [Precompress](https://github.com/mstahv/splitanalyzer/blob/master/src/main/java/org/peimari/splits/Splits.gwt.xml#L13) module in .gwt.xml file will use linker that will pre compress generated js with gzip. E.g. Jetty is then by default smart enought to serve them if the browser knows gzip (and yes, all do).

Note that the visualization part uses [Highcharts library](http://highcharts.com/) and its [GWT wrapper](http://www.moxiegroup.com/moxieapps/gwt-highcharts/). If you use this software you need to comfort to their license. Pretty much all orienteering usage should be fine. Once you have downloaded the file, install it to local maven repo with

    mvn install:install-file -DgroupId=org.moxieapps.gwt  \
    -DartifactId=org.moxieapps.gwt.highcharts \
    -Dversion=1.5.0  \
    -Dfile=org.moxieapps.gwt.highcharts-1.5.0.jar \
    -Dpackaging=jar \
    -DgeneratePom=true

When starting developing (or building this project) download and add highcharts scripts to src/main/resources/org/peimari/splits/public/js directory. Then project should build with "mvn gwt:compile install" command, gwt debugger can be launched with "mvn gwt:debug".

The build also requires this project into your (local) maven repo:
 * [https://github.com/mstahv/iofdomain](https://github.com/mstahv/iofdomain)

### Known issues:
 * IE fails to parse IOF-XML files if they have iso-8859-1 encoding and host page has UTF-8.

