# Tensorflow_Lite_Demo
An example Android application using TensorFLow Lite is available on Tensorflow github, Creating a project directory in tensorflow/tensorflow/contrib/lite/ , which is builted on Android studio 3.0.I have download the model of tflite format and complie the  libtensorflowlite_jni.so and libtensorflowlite.jar


In the demo app, inference is done using the TensorFlow Lite Java API. The demo app classifies frames in real-time, displaying the top most probable classifications. It also displays the inference time taken to detect the object.

# There are two ways to get the demo app to your device:
1,Use Android Studio to build the application. Here it's mine work !
2,Download the source code for TensorFlow Lite and the demo and build it using bazel.I have just give  building commands !



To build the demo app, run bazel:
# TfLiteCameraDemo ---- The demo app classifier
#./tensorflow direction run the bellow command
bazel build --cxxopt=--std=c++11 //tensorflow/contrib/lite/java/demo/app/src/main:TfLiteCameraDemo
for more infomation about [TfLiteCameraDemo](https://github.com/tensorflow/tensorflow/tree/master/tensorflow/contrib/lite/java/demo)

# tflite_demo ---- The demo app on classifier, detector, speech
#./tensorflow direction run the bellow command
sudo bazel build -c opt --config=android_arm{,64} --cxxopt='--std=c++11' "//tensorflow/contrib/lite/examples/android:tflite_demo"
for more infomation about [tflite_demo](https://github.com/tensorflow/tensorflow/tree/master/tensorflow/contrib/lite/examples/android/app)

# bazel-bin/tensorflow/contrib/lite/java/libtensorflowlite.jar ---- the java api for tensorflow lite
sudo bazel build -c opt --config=android_arm{,64} --cxxopt='--std=c++11' "//tensorflow/contrib/lite/java:tensorflowlite"

# bazel-bin/tensorflow/examples/android/libtensorflow_demo.so ---- the native c++ jni  interface for libtensorflowlite.jar
sudo bazel build -c opt --config=android_arm{,64} --cxxopt='--std=c++11' "//tensorflow/examples/android:libtensorflow_demo.so"


# attention
1, Edit your WORKSPACE to add SDK and NDK targets.

android_sdk_repository(
    name = "androidsdk",
    api_level = 25,
    # Ensure that you have the build_tools_version below installed in the
    # SDK manager as it updates periodically.
    build_tools_version = "26.0.1",
    # Replace with path to Android SDK on your system
    path = "/home/XXXX/Software/SDK",
)

android_ndk_repository(
    name="androidndk",
    path="/home/XXXX/Software/NDK/android-ndk-r13b",
    # This needs to be 14 or higher to compile TensorFlow.
    # Please specify API level to >= 21 to build for 64-bit
    # archtectures or the Android NDK will automatically select biggest
    # API level that it supports without notice.
    # Note that the NDK version is not the API level.
    api_level=25)


2, Build this demo app with Bazel. The demo needs C++11. We configure the fat_apk_cpu flag to package support for 4 hardware variants. You may replace it with --config=android_arm64 on a 64-bit device and --config=android_arm for 32-bit device:
For examples:
bazel build -c opt --cxxopt='--std=c++11' --fat_apk_cpu=x86,x86_64,arm64-v8a,armeabi-v7a \
  //tensorflow/contrib/lite/examples/android:tflite_demo

bazel build -c opt --config=android_arm{,64} --cxxopt='--std=c++11' "//tensorflow/contrib/lite/examples/android:tflite_demo"

3, Build the Tensorflow mobile demo using Bazel,which has a fuller set of supported functionality, while  TensorFlow Lite supports only a limited set of operators, so not all models will work on it by default.

#bazel-bin/tensorflow/examples/android/tensorflow_demo.apk
bazel build -c opt //tensorflow/examples/android:tensorflow_demo

#bazel-bin/tensorflow/contrib/android/libandroid_tensorflow_inference_java.jar
bazel build //tensorflow/contrib/android:android_tensorflow_inference_java

#bazel-bin/tensorflow/contrib/android/libtensorflow_inference.so
sudo bazel build -c opt //tensorflow/contrib/android:libtensorflow_inference.so --crosstool_top=//external:android/crosstool --host_crosstool_top=@bazel_tools//tools/cpp:toolchain --cpu=armeabi-v7a  --cxxopt='--std=c++11' --verbose_failures
for more information about [Tensorflow Mobile] (https://www.tensorflow.org/mobile/android_build)

  
