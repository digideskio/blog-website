# Command line arguments

��������������`Unity`ʱ,������������ʱ����һЩ��������Ϣ, ���ַ�ʽ�������ڲ����������Զ�����������������

��`MacOS`ϵͳ�£��������������������
```
 /Applications/Unity/Unity.app/Contents/MacOS/Unity
```
����windowsϵͳ������Ҫִ�������������
```
 "C:\Program Files (x86)\Unity\Editor\Unity.exe"
```

## Options

���������ᵽ��,`editor`Ҳ����������ʱͨ��һЩ�����������������Ϸ, ������оٳ�����Щ���

* `-assetServerUpdate <IP[:port] projectName username password [r <revision>]>`	��`IP:port`�ϵ�`Asset Server`ǿ�Ƹ�����Ŀ. `port`�ǿ�ѡ��,�����ָ���Ļ�,��Ĭ��ѡ��`10733`����˿�. ����������`-projectPath`����һ��ʹ��, ������ȷ���㲻����´���Ŀ.�����ָ����Ŀ���ƵĻ�,��ô��Ĭ�ϵĶ�`Unity`�ϴδ򿪵���Ŀ���и���. ���`-projectPath`·���²�������Ŀ,��ô���Զ�����һ��.
* `-batchmode`  ��`batch`ģʽ������`Unity`.�����������ǿ�ҽ���������������һ��ʹ��, ����ȷ������ͻȻ��������, �����˴���㹤��. ���ɽű������׳��쳣����`Asset Server`����ʧ�ܻ�����������������쳣,`Unity`��ֱ�ӷ��ش�����`1`���˳�. ��Ҫע�����,��`batch`ģʽ��,`Unity`���ڿ���̨���һЩ������־. ���е���`batch`ģʽ�´�һ����Ŀ,��ô`Editor`�Ͳ����ٿ��������ͬ����Ŀ.
* `-buildLinux32Player <pathname>`	Build a 32-bit standalone Linux player (e.g. -buildLinux32Player path/to/your/build).
* `-buildLinux64Player <pathname>`	Build a 64-bit standalone Linux player (e.g. -buildLinux64Player path/to/your/build).
* `-buildLinuxUniversalPlayer <pathname>`	Build a combined 32-bit and 64-bit standalone Linux player (e.g. -buildLinuxUniversalPlayer path/to/your/build).
* `-buildOSXPlayer <pathname>`	Build a 32-bit standalone Mac OSX player (e.g. -buildOSXPlayer path/to/your/build.app).
* `-buildOSX64Player <pathname>`	Build a 64-bit standalone Mac OSX player (e.g. -buildOSX64Player path/to/your/build.app).
* `-buildOSXUniversalPlayer <pathname>`	Build a combined 32-bit and 64-bit standalone Mac OSX player (e.g. -buildOSXUniversalPlayer path/to/your/build.app).
* `-buildTarget <name>`	Allows the selection of an active build target before a project is loaded. Possible options are: win32, win64, osx, linux, linux64, ios, android, web, webstreamed, webgl, xbox360, xboxone, ps3, ps4, psp2, wsa, wp8, bb10, tizen, samsungtv.
* `-buildWebPlayer <pathname>`	Build a WebPlayer (e.g. -buildWebPlayer path/to/your/build).
* `-buildWebPlayerStreamed <pathname>`	Build a streamed WebPlayer (e.g. -buildWebPlayerStreamed path/to/your/build).
* `-buildWindowsPlayer <pathname>`	Build a 32-bit standalone Windows player (e.g. -buildWindowsPlayer path/to/your/build.exe).
* `-buildWindows64Player <pathname>`	Build a 64-bit standalone Windows player (e.g. -buildWindows64Player path/to/your/build.exe).
* `-createProject <pathname>`	Create an empty project at the given path.
* `-executeMethod <ClassName.MethodName>`	Execute the static method as soon as Unity is started, the project is open and after the optional asset server update has been performed. This can be used to do continous integration, perform Unit Tests, make builds, prepare some data, etc. If you want to return an error from the commandline process you can either throw an exception which will cause Unity to exit with 1 or else call EditorApplication.Exit with a non-zero code. If you want to pass parameters you can add them to the command line and retrieve them inside the method using System.Environment.GetCommandLineArgs. To use -executeMethod, you need to place the enclosing script in an Editor folder. The method to be executed must be defined as static.
* `-exportPackage <exportAssetPath1 exportAssetPath2 ExportAssetPath3 exportFileName>`	Exports a package given a path (or set of given paths). exportAssetPath is a folder (relative to to the Unity project root) to export from the Unity project and exportFileName is the package name. Currently, this option can only export whole folders at a time. This command normally needs to be used with the -projectPath argument.
* `-force-d3d9 (Windows only)	Make the editor use Direct3D 9 for rendering. This is the default, so normally there��s no reason to pass it.
* `-force-d3d11 (Windows only)	Make the editor use Direct3D 11 for rendering.
* `-force-opengl (Windows only)	Make the editor use OpenGL for rendering, even if Direct3D is available. Normally Direct3D is used but OpenGL is used if Direct3D 9.0c is not available.
* `-force-free	Make the editor run as if there is a free Unity license on the machine, even if a Unity Pro license is installed.
* `-importPackage <pathname>`	Import the given package. No import dialog is shown.
* `-logFile <pathname>`	Specify where the Editor or Windows/Linux/OSX standalone log file will be written.
* `-silent-crashes	Don��t display crash dialog.
* `-nographics	When running in batch mode, do not initialize graphics device at all. This makes it possible to run your automated workflows on machines that don��t even have a GPU (automated workflows only work, when you have a window in focus, otherwise you can��t send simulated input commands). Please note -nographics will not allow you to bake GI on OSX, since Enlighten requires GPU acceleration.
* `-projectPath <pathname>`	Open the project at the given path.
* `-quit	Quit the Unity editor after other commands have finished executing. Note that this can cause error messages to be hidden (but they will show up in the Editor.log file).
* `-returnlicense	Return the currently active license to the license server. Please allow a few seconds before license file is removed, as Unity needs to communicate with the license server. This option is new in Unity 5.0.
* `-serial <serial>`	Activates Unity with the specified serial key. It is recommended to pass ��-batchmode -quit�� arguments as well, in order to quit Unity when done, if using this for automated activation of Unity. Please allow a few seconds before license file is created, as Unity needs to communicate with the license server. Make sure that License file folder exists, and has appropriate permissions before running Unity with this argument. In case activation fails, see the Editor.log for info. This option is new in Unity 5.0.

## Example usage
```
// C# example
using UnityEditor;
class MyEditorScript
{
     static void PerformBuild ()
     {
         string[] scenes = { "Assets/MyScene.unity" };
         BuildPipeline.BuildPlayer(scenes, ...);
     }
}


// JavaScript example
static void PerformBuild ()
{
    string[] scenes = { "Assets/MyScene.unity" };
    BuildPipeline.BuildPlayer(scenes, ...);
}
```
The following command executes Unity in batch mode, executes the MyEditorScript.MyMethod method and then quits upon completion.

* `Windows: `C:\program files\Unity\Editor\Unity.exe -quit -batchmode -executeMethod MyEditorScript.MyMethod`

* `Mac OS: `/Applications/Unity/Unity.app/Contents/MacOS/Unity -quit -batchmode -executeMethod MyEditorScript.MyMethod`

The next command executes Unity in batch mode and updates from the asset server using the supplied project path. The method is executed after all assets have been downloaded and imported from the asset server. After the method has finished execution, Unity automatically quits.
```
/Applications/Unity/Unity.app/Contents/MacOS/Unity -batchmode -projectPath ~/UnityProjects/AutobuildProject -assetServerUpdate 192.168.1.1 MyGame AutobuildUser l33tpa33 -executeMethod MyEditorScript.PerformBuild -quit
```

## Unity Editor special command line arguments

These should only be used under special circumstances, or when directed by Support.

* `-enableIncompatibleAssetDowngrade	Use when you have content made by a newer, incompatible version of Unity, that you want to downgrade to work with your current version of Unity. When enabled, Unity will present you with a dialog asking for confirmation of such a downgrade if you attempt to open a project that would require it. This procedure is unsupported and highly risky, and should only be used as a last resort.

## Unity Standalone Player command line arguments

Standalone players built with Unity also understand some command line arguments:

* `-adapter N (Windows only)	Allows the game to run full-screen on another display. The N maps to a Direct3D display adaptor. In most cases there is a one-to-one relationship between adapters and video cards. On cards that support multi-head (they can drive multiple monitors from a single card) each ��head�� may be its own adapter.
* `-batchmode	Run the game in ��headless�� mode. The game will not display anything or accept user input. This is mostly useful for running servers for networked games.
* `-force-d3d9 (Windows only)	Make the game use Direct3D 9 for rendering. This is the default, so normally there��s no reason to pass it.
* `-force-d3d9-ref (Windows only)	Make the game run using Direct3D��s ��Reference�� software renderer. The DirectX SDK has to be installed for this to work. This is mostly useful for building automated test suites, where you want to ensure rendering is exactly the same no matter what graphics card is being used.
* `-force-d3d11 (Windows only)	Make the game use Direct3D 11 for rendering.
* `-force-opengl (Windows only)	Make the game use OpenGL for rendering, even if Direct3D is available. Normally Direct3D is used but OpenGL is used if Direct3D 9.0c is not available.
* `-nographics	When running in batch mode, do not initialize graphics device at all. This makes it possible to run your automated workflows on machines that don��t even have a GPU.
* `-nolog (Linux & Windows only)	Do not produce output log. Normally output_log.txt is written in the *_Data folder next to the game executable, where Debug.Log output is printed.
* `-popupwindow	The window will be created as a a pop-up window (without a frame).
* `-screen-fullscreen	Overrides the default fullscreen state. This must be 0 or 1.
* `-screen-height	Overrides the default screen height. This must be an integer from a supported resolution.
* `-screen-width	Overrides the default screen width. This must be an integer from a supported resolution.
* `-screen-quality	Overrides the default screen quality. Example usage would be: /path/to/myGame -screen-quality Beautiful
* `-show-screen-selector	Forces the screen selector dialog to be shown.
* `-single-instance (Linux & Windows only)	Allow only one instance of the game to run at the time. If another instance is already running then launching it again with -single-instance will just focus the existing one.
* `-parentHWND <HWND>` (Windows only)	Embeds Windows Standalone application into another application, you have to pass parent application��s window handle to Windows Standalone application. See this example EmbeddedWindow.zip for more information.

## Windows Store Command line arguments

Windows Store Apps don��t accept command line arguments by default, so to pass them you have to call a special function from App.xaml.cs/cpp or App.cs/cpp. For example,
```
appCallbacks.AddCommandLineArg("-nolog");
```

You should call this before the appCallbacks.Initialize*() function.

* `-nolog	Don��t produce UnityPlayer.log.
* `-force-driver-type-warp	Force DirectX 11.0 WARP device (More info http://msdn.microsoft.com/en-us/library/gg615082.aspx)
* `-force-gfx-direct	Force single threaded rendering.
* `-force-d3d11-no-singlethreaded	Force DirectX 11.0 to be created without D3D11_CREATE_DEVICE_SINGLETHREADED flag.
* `-force-feature-level�C9�C1	Force DirectX 11.0 feature level 9.1.
* `-force-feature-level�C9�C2	Force DirectX 11.0 feature level 9.2.
* `-force-feature-level�C9�C3	Force DirectX 11.0 feature level 9.3.
* `-force-feature-level�C10�C0	Force DirectX 11.0 feature level 10.0.
* `-force-feature-level�C10�C1	Force DirectX 11.0 feature level 10.1.
* `-force-feature-level�C11�C0	Force DirectX 11.0 feature level 11.0.