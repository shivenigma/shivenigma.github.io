---
layout: post
title: "Flutter - Creating Custom Launcher Icons"
date: 2018-12-25
excerpt: "How to change launcher icons in flutter?"
tags: [flutter, launcher icon, android, ios, hybrid-apps]
published: true
featured: "/assets/img/posts/flutter-assets/flutter-logo.png"
---
<figure>
	<img src="/assets/img/posts/flutter-assets/flutter-logo.png">
</figure>

I am sure most of us might have already heard about `flutter` and some of us are really excited and trying it out. In case if you're living under a rock, <a target="_blank" href="https://flutter.io">Flutter</a> is a hybrid application development framework which is developed by Google.

#### Hybrid applications?

Those unusable, unstable, responsive websites put into a webview?
<figure>
<a target="_blank" href="https://xkcd.com/1174/">
    <img src="https://imgs.xkcd.com/comics/app.png"/>
</a>
</figure>


Before you beginning to think just another hybrid application framework, I suggest you to read my <a target="_blank" href="https://www.agiratech.com/7-reasons-why-google-flutter-should-be-your-next-choice/">post</a> at my work about why you should choose flutter. TLDR, I  worked heavily with ionic framework and I know how webviews can be a <a target="_blank" href="https://www.urbandictionary.com/define.php?term=pita">`PITA`</a> for developers. Flutter eliminates most of the pain points by taking low level control unlike ionic and it provides you with a perfect ecosystem for development (For ex. Hot Reload with state). I can go ahead and consume the whole post for convincing you to try flutter, but let's not do it at this time.

I am working in a play project using flutter and I decided to write about the experience of it. Today I tried to change the launcher icon for that app and I was able to find a package for that which works really great. Flutter has a great community around it so you can find a package for most of the mundane things.

#### Creating new application

Follow the below steps to create a new flutter app and change its launcher icon to a different image. I am assuming that you've downloaded and installed flutter SDK and Android studio + Android SDK.

* Open your terminal and enter the following commands to create a new application, replace `myapp` with whatever you would like.
{% highlight bash %}
flutter create myapp
{% endhighlight %}

* Switch into the newly created folder by entering
{% highlight bash %}
cd myapp
{% endhighlight %}

* Connect a device or run an android emulator and then run
{% highlight bash %}
flutter run
{% endhighlight %}

Once your command has finished it'll wait for any further input or changes. You can see the default flutter app in your phone. From here on you can make changes to your code and press `r` in the terminal which means `reload` and it'll automatically refreshes the application.

#### Launcher icons
Launcher icons are what we see in our main menu or home screen, I am not sure what the iOS people would call it. Minimize your application to see the launcher icon, you'll see the flutter logo in the launcher, let's see how to change it to our desired icon. When choosing source image for the icon I suggest you to keep a high resolution image of the icon, the image will be scaled up / down by a script and placed into the respective folders for iOS and Android.

* Edit your `pubspec.yaml` file in your project folder and add the following in that file.
{% highlight yaml %}
dev_dependencies: 
  flutter_launcher_icons: "^0.7.0"
{% endhighlight %}

The `pubspec.yaml` file is similar to `package.json` file we have for NPM. This manages all the configurations and dependencies of your app. The above code will add a package called <a target="_blank" href="https://pub.dartlang.org/packages/flutter_launcher_icons">`flutter launcher icons`<a> to your code

* Add the configuration for the `launcher_icons` like below, It'll be picked up by the package when we run it.
{% highlight yaml %}
flutter_icons:
  android: "launcher_icon" 
  ios: true
  image_path: "assets/icon/icon.png"
{% endhighlight %}

The `image_path` is where we placed the image, change the name according to your need and there are lot of other configuration options available for this package, We'll see them below.
- **android/ios** 
    1. true - Overwrite the existing flutter icons and create the new icons for the platform
    2. false - Ignore this platform when making icons
- **image_path** - The location of the image which you want to use as launcher icon.

If you want to use different icons for Android and iOS,
- **image_path_android** - specific icon for android, if not given image_path will be used.

- **image_path_ios** - specific icon for iOS, if not given image_path will be used.

Some android specific <a target="_blank" href="https://developer.android.com/guide/practices/ui_guidelines/icon_design_adaptive">adaptive icons</a> configurations are also available,

- **adaptive_icon_background** - A color or path to an image asset which will be used as a background of adaptive icons on android.
- **adaptive_icon_foreground** - The image asset which will be used as a foreground image.

After entering your options in the `pubspec.yaml` file we have to run the package's publish command to generate images and place them into the right folders. run the following in the terminal

{% highlight bash %}
flutter packages pub run flutter_launcher_icons:main
{% endhighlight %}

Now if you run `flutter run` again you can see your new icon in your launcher instead of the default one.

> Note: pubspec.yaml file can become large as your project grows, so it's wise to split the configurations into their own files whenever possible.

This package supports having a separate configuration file in the name of `flutter_launcher_icons.yaml` in your lib folder. You can even have a different name for the file but you have to specify the file name when running this package as below.


{% highlight bash %}
flutter_launcher_icons:main -f <your config file name>
{% endhighlight %}

Thanks for reading, I hope you've enjoyed it and it is useful for you. This is my first post on this blog and after a long time. I would love to hear your thoughts about the writing, the website, readability and anything.

### Please leave a comment.
