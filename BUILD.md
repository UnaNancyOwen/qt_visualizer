How to Build Sample Program
===========================

(1) Build VTK with Qt (QVTK) and Generate QVTKWidgetPlugin
----------------------------------------------------------
以下のドキュメントに従い、QVTKをビルドする。  
QVTKWidgetPluginを生成してQt Designerに登録する。  

  * [Building QVTK(VTK7+Qt5) with Visual Studio](https://gist.github.com/UnaNancyOwen/77d61f9f21376c9b59fc#file-qvtk7-md)

(2) Build PCL with QVTK
-----------------------
以下のドキュメントに従い、PCLをビルドする。  
VTKはQVTKを利用、WITH_QTにチェックを入れる。  

  * [Building PCL 1.8.0 with Visual Studio](https://gist.github.com/UnaNancyOwen/59319050d53c137ca8f3#file-pcl1-8-0-md)

(3) Build Sample Program
------------------------
1. サンプルプログラムをダウンロードする。(qt_visualizer)  

  * [Qt_Visualizer](https://github.com/UnaNancyOwen/qt_visualizer)

2. Qt Creatorの起動してプロジェクトファイル(qt_visualizer\sample\src\pcl_visualizer.pro)を開く。  
   [Configure Project]ボタンを押す。  

  ![capture_001](https://cloud.githubusercontent.com/assets/816705/23370496/31813cb4-fd58-11e6-82ee-68acc8b50c4d.PNG)

3. [Projects]ボタンを押してBuild Settingsを開き、以下のように設定する。  
    1. Edit build configurationのプルダウンメニューから設定するビルド構成を選択する。(Debug or Release)  

    2. GeneralのBuild directoryにビルドディレクトリを指定する。(../build)  
       ビルドディレクトリはソースコードディレクトリと同じ深さにある必要があります。  

       ```
       ├── build
       └── src
           ├── CMakeLists.txt
           ├── main.cpp
           ├── pclviewer.cpp
           ├── pclviewer.h
           ├── pclviewer.ui
           └── pcl_visualizer.pro
       ```

    3. Build Stepsにビルドステップを設定します。  
       既存のビルドステップはすべて削除します。  
       Build StepsのプルダウンメニューからCustom Process Stepを選択、追加します。  
       このCustom Process Stepを２つ追加します。  
       1つめはCMake、2つめはMSBuildの設定をします。  

       * CMake
         * **Command** cmake
         * **Arguments** -G "Visual Studio 14 2015 Win64" ../src
         * **Working directory** %{buildDir}/../build

       * MSBUild
         * **Command** msbuild
         * **Arguments** pcl_visualizer.vcxproj /p:configuration=release
         * **Working directory** %{buildDir}/../build

  ![capture_009](https://cloud.githubusercontent.com/assets/816705/23370508/3bb3fdc0-fd58-11e6-9f39-8b2ec175f9d1.PNG)
  ![capture_003](https://cloud.githubusercontent.com/assets/816705/23370516/40a94420-fd58-11e6-97b5-07790f1cf6ba.PNG)

4. ビルド構成を選択、[Build Project]ボタンを押してプロジェクトをビルドします。  

  ![capture_007](https://cloud.githubusercontent.com/assets/816705/23370535/4f6e2930-fd58-11e6-9a81-d53b0adad15c.PNG)
  ![capture_005](https://cloud.githubusercontent.com/assets/816705/23370549/5fbc3a84-fd58-11e6-9864-d34aa30f2e16.PNG)

5. [Run]ボタンを押してプログラムを実行します。  

  ![capture_006](https://cloud.githubusercontent.com/assets/816705/23370559/6203ffde-fd58-11e6-8389-91877814c256.PNG)

Where is refactored from original tutorial?
-------------------------------------------
元のチュートリアルからの変更は以下の通りです。  
CMakeLists.txtはほぼすべて書き直しました。  

* [CMakeLists.txt#ALL](https://github.com/UnaNancyOwen/qt_visualizer/blob/master/sample/src/CMakeLists.txt)
* [pcl_visualizer.pro#L9](https://github.com/UnaNancyOwen/qt_visualizer/blob/master/sample/src/pcl_visualizer.pro#L9)

  ```
  greaterThan(QT_MAJOR_VERSION, 5): QT += widgets
  ```

How to set-up Qt Creator when libraries installed using Vcpkg?
--------------------------------------------------------------
Vcpkgを使用することで、Qtオプション(WITH_QT)を含むPCLを簡単に構築することができます。  
ただし、Qt Creatorを使用するためには追加の設定が必要です。  

* [How to use Qt that installed by Vcpkg with Qt Creator](https://gist.github.com/UnaNancyOwen/e24e67966af5f1d5f5cce4a88af7b4a4)