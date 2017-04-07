# Android TensorFlow Detect Object Example
A simple android using Tensor Flow and pre-trained model to detect objects captured from camera

################################################

Let build Tensor Flow first

- Clone Tensor Flow source code from https://github.com/tensorflow/tensorflow.git
- Download NDK https://developer.android.com/ndk/downloads/older_releases.html#ndk-12b-downloads (should download revision 12b to build tensor flow without error)
- Instal Bazel, Bazel is the primary build system for TensorFlow. On Mac using homebrew for installing easily https://bazel.build/versions/master/docs/install-os-x.html
- Go to source code of Tensor Flow cloned above, open WORKSPACE file to edit. Uncomment android_sdk_repository and android_ndk_repository like this:
<pre>
<code>
android_sdk_repository(
    name = "androidsdk",
    api_level = 23,
    build_tools_version = "25.0.2",
    # Replace with path to Android SDK on your system
    path = "/Android/sdk/",
)

android_ndk_repository(
    name="androidndk",
    path="/Android/android-ndk-r12b/",
    api_level=14)
    </code></pre>
    
    
 - Build TensorFlow with Bazel, must go to folder of tensor flow source code to run this command
    
 <pre><code> 
    bazel build -c opt //tensorflow/contrib/android:lib_tensorflow.so \
   --crosstool_top=//external:android/crosstool \
   --host_crosstool_top=@bazel_tools//tools/cpp:toolchain \
   --cpu=armeabi-v7a \
   --strategy=CppCompile=standalone
 </code></pre>
    
 - When build finished successfully, you will find lib_tensorflow.so file in 
 <pre><code>
 [tensorflow source code path]/bazel-bin/tensorflow/contrib/android/lib_tensorflow.so
 </code></pre>
 
 - Now you need to generate jar file to import into android project, run this command
 <pre><code>
 bazel build //tensorflow/contrib/android:android_tensorflow_java
 </code></pre>
 
 - So now you have android_tensorflow_java.jar in folder [tensorflow source code path]/bazel-bin/tensorflow/contrib/android/
 and can add as library to android project, create jniLibs folder in src/main and copy lib_tensorflow.so to there.
 
 - In this example will use the Google pre-trained model which does the object detection on a given image. Download here https://storage.googleapis.com/download.tensorflow.org/models/inception5h.zip . Then copy them to assest folder of android project.
 
 
