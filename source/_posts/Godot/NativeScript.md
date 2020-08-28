## C++

### Initial setup

You need to clone the GDNative C++ bindings in the root of this project:

您需要在此项目的根目录中克隆GDNative C ++绑定：

```sh
git clone --recursive https://github.com/GodotNativeTools/godot-cpp
```


### Updating the api.json

Our api.json file contains meta data of all the classes that are part of the Godot core and are needed to generate the C++ binding classes for use in GDNative modules. 

我们的api.json文件包含Godot核心一部分中所有类的元数据，并且是生成用于GDNative模块的C ++绑定类所必需的。

This file is supplied with our godot_headers repository for your convinience but if you are running a custom build of Godot and need access to classes that have recent changes a new api.json file must be generated. You do this by starting your Godot executable with the following parameters:

为了方便起见，此文件随我们的godot_headers存储库一起提供，但是如果您正在运行Godot的自定义版本，并且需要访问最近进行更改的类，则必须生成一个新的api.json文件。 为此，您可以使用以下参数启动Godot可执行文件：

```shell
$ godot --gdnative-generate-json-api api.json
```

Now copy the api.json file into your folder structure so its easy to access. **Note** the remark below for the extra ```custom_api_file``` command line parameter needed to tell scons where to find your file.

现在将api.json文件复制到您的文件夹结构中，以便于访问。 **注意**下面的注释是用于告诉scons在哪里找到文件所需的额外的“ custom_api_file”命令行参数。

### Compiling the cpp bindings library

The final step is to compile our cpp bindings library:

```
$ cd godot-cpp
$ scons platform=<your platform> generate_bindings=yes
$ cd ..
```

> Replace `<your platform>` with either `windows`, `linux` or `osx`.

> Include `use_llvm=yes` for using clang++

> Include `target=runtime` to build a runtime build (windows only at the moment)
>
> 包含“ target = runtime”以构建运行时内部版本（目前仅Windows）

> The resulting library will be created in `godot-cpp/bin/`, take note of its name as it will be different depending on platform.
>
> 生成的库将在`godot-cpp / bin /`中创建，请注意其名称，因为其名称因平台而异。

> If you want to use an alternative api.json file add `use_custom_api_file=yes custom_api_file=../api.json`, be sure to specify the correct location of where you placed your file.
>
> 如果要使用其他api.json文件，请添加`use_custom_api_file = yes custom_api_file = .. / api.json`，请确保指定放置文件的正确位置。


### Compile the demos

A `Makefile` is provided in each demo to make the process more convenient.