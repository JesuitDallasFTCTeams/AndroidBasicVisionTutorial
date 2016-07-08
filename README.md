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

I will assume that you already have Android Studio installed, though I will run through some basics about getting a virtual Android device running on your machine that meets our needs for OpenCV.

###Download OpenCV
If you have not yet done so, download and install Android Studio. You can start from scratch with Android with help from the Android developers. Android Studio allows us to manage Android SDKs, which will be addressed below.

Install the most recent version of OpenCV for Android here. Uncompress the zip file containing the SDK. This folder contains the Android application package (APK) files for OpenCV Manager, which we will address below, sample OpenCV applications for Android, and the OpenCV SDK itself.

###Create New Project
Now that we have the OpenCV library, we need to start a new Android Project and learn how to use the OpenCV library with an Android app.

Go ahead and open Android Studio.

Create a new project. Call it OpenCVTutorial (or whatever you'd like) and define a "company domain".


We will be using the android.hardware.camera2 package from the Android SDK. This requires a minimum SDK defined as Android API 21 or later. You do not need to worry much about this package, as that will not be the focus of this tutorial.

Go ahead and set the minimum SDK to Android API 21 (Lollipop) for phone and tablet development.

We will start from scratch, but initialize our project with an empty activity. Call that activity MainActivity, which should be the default name in Android Studio.  The project will initialize and leave us with a relatively empty project looking like this: New project empty and loaded Now we need to add OpenCV to our Android project.

###Import OpenCV Module

To import OpenCV into our project, we import OpenCV as a new module. We do this with File → New → Import Module...

Navigate to where you unzipped the OpenCV SDK and add the sdk/java folder as the module to be imported.

Leave the default settings checked and import the OpenCV library.

Now, you will see a summary of the module import. You will see that a few things seem broken or cannot be resolved in the OpenCV library from the initial import.

This has to do with the default Android API expected by the OpenCV build, which we will fix below.

###Modify Gradle Build
We will use Grade to run our build configuration.

Open the build.gradle files for our two modules: one for the app module and one for the OpenCV module. You will notice that they look something like the below and thus do not match one another.

What we need to do is ensure that our minimum SDK is set to the correct Android API we're after, as well as our compile and target SDK versions.

Set the minimum SDK version to API 21 and the compile SDK version and target SDK version to API 22 in both build.gradle files.

Android API 23 has a new set of permissions standards that are beyond this tutorial, thus we will be targeting the newest Lollipop API instead of jumping into Marshmallow.

Go ahead and sync the Gradle files using the Gradle sync icon (or use Android Studio's sync prompt):


If you do not already have the necessary Android API versions downloaded, Android Studio may prompt you to download the correct build tools and correct SDK.

Without being prompted, you may manage your available SDKs using the SDK Manager:

Clicking the appropriate SDK versions for your build and clicking install automates the download process of the SDK for you in Android Studio.

Anyways, you'll now notice that our sync has succeeded with no errors. E.g., the camera2 components are now resolved in the Camera2Renderer class in the OpenCV library, because camera2 only exists for Android API 21 or later.

Note that we also needed to change the build tools version to "22.0.1" in both Gradle files, as well as a dependency in the app Gradle file to "compile 'com.android.support:appcompat-v7:22.2.0'".

We will be using an emulated virtual device that we can create in Android Studio to run our OpenCV applications.

###Create a Virtual Device
Our virtual device will use my Macbook Air's built-in webcam as the emulated phone's back camera. We will use a fully emulated camera for the front camera, which we will not use with OpenCV in this tutorial.

Using the Android Virtual Device Manager, let's create a new virtual device using the AVD icon.

Running an Android emulation is quite memory intensive to forewarn you. Let's go ahead and create a new virtual device.

For the sake of this example I will create a virtual Nexus 6.

Now we want our emulated Nexus 6 to run Android Lollipop 5.1 using an x86 system image.

You can leave the default settings on the main page of the device settings, but we need to activate the camera for our emulation to use with OpenCV. Open the advanced settings.

Select the appropriate camera settings for the front and back cameras. Again, we will emulate the front camera and use a built-in webcam as the back camera for OpenCV.

Go ahead and hit Finish. It may take a minute for our new virtual device to be saved.

Once the virtual device is successfully saved, you can close the AVD Manager.

Now let's start our virtual device up by running our app using the run icon.

Launch the newly created emulator to run the app.

The virtual device will boot up as if you had clicked the power button of a real device for the first time. This usually takes a bit longer than typical bootup time, but somewhere around 3 to 5 minutes is typical on my machine.

Now, before we move any further and actually develop an OpenCV application, we must make the OpenCV library available to our app for use.
Again, we will be using Android Studio for the development of our project. Having completed one or two projects in Android Studio is probably sufficient to get through this tutorial, but some basic knowledge is presumed. I recommend you start here if you haven't built anything in Android Studio before.

###Add OpenCV Library
What we did above by importing OpenCV is make the OpenCV classes available to the app for use within the app. If we try to use any OpenCV classes, you'll notice that Android Studio doesn't know how to resolve any objects that we declare. This is because we must initialize our app's environment with the libraries that we plan to use. We overcome this by making the full OpenCV libraries available to our app. We can do this by pointing our app to the library asynchronously at runtime, or we can statically upload the library to our app's project files.

What we will do here for development is called static initialization. This is meant for the purpose of development where we, as the developer, are hosting the OpenCV library as a part of the app's project files, so the app has the entire backend library at its fingertips. 

So we will be using our own copy of the library inside our app's project files while we develop in Android Studio. What we will do when the app is actually running is use OpenCV Manager, which is an app that allows us to access OpenCV libraries using async initialization. I will show how to download OpenCV Manager to your emulated device later on.

do the following:

Go to the "Project" tab in the file directory listing view (we are currently in the "Android" tab). Create a libraries folder underneath your project's main directory. For my project named OpenCVTutorial, I am creating an OpenCVTutorial/libraries folder.


Now, from the OpenCV SDK folder that we unzipped in the beginning, go to the sdk/java file that we imported as the module. Copy the java file. Paste this file into the new libraries directory that we just created in the project folder. Upon pasting, Android Studio will prompt us for a new name for the file. Enter the name "opencv". This is a large file and may take a minute to paste.

Now we must create a build.gradle file for the OpenCV library. Go ahead and create a new build.gradle file in the newly copied opencv directory.

The new build.gradle file needs the following contents:

Our settings.gradle file for our project now needs to know that we have the OpenCV library available. In our settings.gradle file in the project directory, add "include ':libraries:opencv'". 

Go ahead and sync your project. It should complete without any errors.

Now we must add our new library as a depenency of our app module. Right-click the project folder and click Open Module Settings.

Go to our app module, where we will add a new module dependency.

Add our newly created library as a dependency.

The final thing we must do in order to use the OpenCV library statically is copy all of the JNI library files for the different architectures from the OpenCV SDK into our app module.

Go to the OpenCV SDK and go to the native directory within the sdk file. We want all of the architecture files out the libs folder within the native directory. Copy all of these files.

Back in the app module, go to app/src/main. Within this directory, create a new directory called "jniLibs". Paste all of the architecture JNI library files into this new directory.

Now we can get on and start coding a bit using the OpenCV library in our app! The first thing we're going to do is get the camera up and running and displaying an image for us using the webcam.

###Get the Camera Working
The first thing that we need to do is get our XML layout file ready to use with the camera. Open activity_main.xml found in path app/main/res/layout. Delete the Hello World TextView provided by Android Studio in this file.

Change the RelativeLayout tag by deleting all of the attributes except for:

xmlns:android="..."
android:layout_width="match_parent"
android:layout_height="match_parent"

and add the attribute:

android:orientation="horizontal"
Layout skeleton
You can change the layout preview to a Nexus 6 if you would like to get a better picture of what our emulator will look like.

Now we need to add a new JavaCameraView from the OpenCV library that will take up the entirety of the screen. Add the new view as the following, giving it an ID:

You can see in the layout preview that the JavaCameraView appears, taking up the entirety of the app screen.

When the app starts, we need to get the camera started and display the camera feed in this view. Now that we have the view defined in the layout, let's see how we can get access to the camera.

Open the MainActivity.java class file. We need to implement the following methods:

* onCreate
* onPause
* onResume
* onDestroy
In addition to those methods, we will also implement a BaseLoaderCallback method, which will be used with OpenCV Manager in order to operate our app with the OpenCV libraries.

When we open the app, we want to open the camera for use with our OpenCV library and we want to display the camera's feed to the JavaCameraView.

As we implement the methods above, we will need to import a number of components. I will show a few of them, but Android Studio will let you know when you must import a new component. I will provide screen shots of everything imported at key stages.

* onCreate Method

 The first thing we need to do is force the screen to stay on when the app is opened


 Now we need to declare a new CameraBridgeViewBase object from the OpenCV library that we will use to let our app know that we are using a camera whose feed should be displayed in the JavaCameraView defined in the activity_main.xml layout file.

 Add CameraBridgeViewBase Declare CameraBridgeViewBase
 Get the JavaCameraView by its ID and set its visibility to visible. Set the camera listener to our current activity.

 Add SurfaceView Set camera listener
 Note that we are passing "this" to the setCvCameraViewListener method. In order for this to work we must specify that our activity MainActivity implements CvCameraViewListener2. We will also specify that MainActivity implements our OnTouchListener as well, which we will see later.

 MainActivity Import Implement
 Go ahead and explicitly import:

 android.view.View.OnTouchListener
 org.opencv.android.CameraBridgeViewBase.CvCameraViewListener2
 MainActivity Implements OnTouchListener and CvCameraViewListener2
 Android Studio gives us an error here because in order to for MainActivity to implement OnTouchListener and CvCameraListener2, we need some guaranteed methods defined. Go ahead and let Android Studio create those methods for you. We will work with these momentarily.

 Implement methods Implement method list Implemented methods
 onPause Method

 To implement onPause, we would simply like to disable the view that we are using. The disable view method relinquishes the camera for us (internally calling the release method on the camera) so that other apps may use it.

 onPause Method
 onResume Method

 This method is a bit more complex, as we will invoke our BaseLoaderCallback method from the OpenCV library, which we will use to reinstantiate our OpenCV view appropriately. This method will be a bit clearer when we implement the BaseLoaderCallback function for our activity in a moment.

 onResume Method
 Note that mLoaderCallback is our instance of the BaseLoaderCallback for this activity, so we will take care of that error in a moment.

 onDestroy Method

 Similar to the onPause method, we simply would like to disable the view that we are using with onDestroy.

 onDestroy Method
 BaseLoaderCallback

 When we declare a new BaseLoaderCallback, Android Studio knows that we need at least some guaranteed methods in order to use it. For this one, we only need to add the onManagerConnected method.

 Implement BaseLoaderCallback methods BaseLoaderCallback
 Now when we have OpenCV Manager installed on our device, when we resume our app's usage, we are using async initialization to load our OpenCV library and reinstantiate our view. Take a look again at how async is used in the onResume function, now error free:

 
 This is everything we have imported thus far:


 Let's run the app and see what we have so far! When you are running the app, keep the emulator open and reuse the same virtual device without relaunching it. It will act as if a device is connected by USB, more or less. You can install apps, uninstall apps, and generally use the virtual device as if it were a full Android device.

 After a minute or two, your app will automatically open on your virtual device.

 
 Oh no! What happened here? We specified that the emulator should use the webcam and everything!

 Simple! We forgot to specify that our app has permission to use the camera and that our app will be using camera features in our project's AndroidManifest.xml file. Add the following uses-permissions and uses-feature tags to that file.

 Manifest permissions
 Run the app again.

 Why doesn't anything appear? The app is allowed to use the camera and the webcam should be working...

 This is an issue with the way that we deal with camera input with the OpenCV library. Remember the methods that were automatically created when we said this activity implements a CvCameraViewListener2? That's what we must do next.

 Declare a private Mat variable called mRgba. RGBA is simply the RGB color model, while the A has to do with transparency.

 mRgba Variable
 Now we must implement the following methods:

 onCameraViewStarted
 onCameraViewStopped
 onCameraFrame
 The implementation of these is straightforward, so I will just give them all to you at once:

 Camera view and frame methods
 When our camera view starts, we want to save a new matrix to our mRgba variable.
 When a camera view stops, we want to free this matrix.
 While the camera is running, we want to store the inputFrame in the matrix that we created and return it.
 Now go ahead and run the app again.

 We have a bizarre problem. If your emulation works like mine, this is what your virtual device looks like:

 
 This is a serious problem considering the app we are trying to create expressly involves encoding colors into its RGB notation with  OpenCV. Clearly there is something wrong with how the colors of the image are encoded at some layer of our abstracted camera.

 I will be brief in summing up a solution I found here (StackOverflow). Basically the encoding that we are using must be changed within the OpenCV's JavaCameraView class when we are using this type of emulator:


 Add the following highlighted code to that class:

 
 And modify the public Mat rgba() method:

 
 And that should do it! Run your app again on your emulator and you should see the following:


 Alright, we are just about ready to move into the image processing phase of our app development! First we are going to quickly touch on how to get OpenCV Manager installed on our emulator so that we know how to emulate a real-world working OpenCV app.






