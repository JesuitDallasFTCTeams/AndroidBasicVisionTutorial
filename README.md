# OpenCV for Basic Android Color Detection
### Computer Vision Tutorial

This program implements basic color detection using OpenCV for Android.
This program accompanies the tutorial, showing detailed instructions on how to complete the following using Android Studio:

* Download and import OpenCV into Android Studio project
* Emulate Android camera using webcam and virtual device
* Display camera feed using OpenCV library
* Process camera feed frames in real time with OpenCV
* Process color in image on touch event

###Overview
This tutorial is designed for you to hit the ground running with your first OpenCV project for Android.

OpenCV is an open source computer vision library that assists developers by offering a great deal of computer vision utilities including image processing, feature detection, and video analysis, as well as a machine learning library to accomplish much of our tasks.

This tutorial is designed to act as a bridge between the "hello world" example provided by OpenCV and the sample projects provided by OpenCV. This tutorial is also meant to offer a starting point for using OpenCV with Android Studio.

The above being said, we will touch on many of the concepts from the "hello world" example in order to get our app running from scratch in Android Studio. We will then move on to implement some basic image processing functionality, with the ultimate goal of displaying the hex code of a user-selected region of the real-time image.

The OpenCV project that we are bridging towards is something like the color-blob-detection sample, so we will draw from this sample in part in order to show some post-hello-world steps.

Again, we will be using Android Studio for the development of our project. Having completed one or two projects in Android Studio is probably sufficient to get through this tutorial, but some basic knowledge is presumed. I recommend you start here if you haven't built anything in Android Studio before.

I will assume that you already have Android Studio installed, though I will run through some basics about getting a virtual Android device running on your machine that meets our needs for OpenCV.

###Download OpenCV
If you have not yet done so, download and install Android Studio. You can start from scratch with Android with help from the Android developers. Android Studio allows us to manage Android SDKs, which will be addressed below.

Install the most recent version of OpenCV for Android here. Uncompress the zip file containing the SDK. This folder contains the Android application package (APK) files for OpenCV Manager, which we will address below, sample OpenCV applications for Android, and the OpenCV SDK itself.

###Create New Project
Now that we have the OpenCV library, we need to start a new Android Project and learn how to use the OpenCV library with an Android app.

Go ahead and open Android Studio.

Create a new project. Call it OpenCVTutorial (or whatever you'd like) and define a "company domain".

New project info
We will be using the android.hardware.camera2 package from the Android SDK. This requires a minimum SDK defined as Android API 21 or later. You do not need to worry much about this package, as that will not be the focus of this tutorial.

Go ahead and set the minimum SDK to Android API 21 (Lollipop) for phone and tablet development.

We will start from scratch, but initialize our project with an empty activity. Call that activity MainActivity, which should be the default name in Android Studio.  The project will initialize and leave us with a relatively empty project looking like this: New project empty and loaded Now we need to add OpenCV to our Android project.


