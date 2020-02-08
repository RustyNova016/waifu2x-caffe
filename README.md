waifu2x-caffe (for Windows)
----------

 Author: lltcggie

This software uses only the conversion function of the image conversion software "[waifu2x] (https://github.com/nagadomi/waifu2x)"
This software was rewritten using [Caffe] (http://caffe.berkeleyvision.org/) and built for Windows.
You can convert with CPU, but you can use CUDA (or cuDNN) to convert faster than CPU.

GUI supports English, Japanese, Simplified Chinese, Traditional Chinese, Korean, Turkish, Spanish, Russian, and French.

You can download the software from the "Downloads" section of [this release page] (https://github.com/lltcggie/waifu2x-caffe/releases).


 Required environment
----------

The following environment is required to operate this software.

 * OS: Windows Vista or later 64bit (There is no exe for 32bit)
 * Memory: 1 GB or more of free memory (depending on the image size to be converted)
 * GPU: NVIDIA GPU with Compute Capability 3.0 or higher (not required when converting with CPU)
 * Microsoft Visual C ++ 2015 Redistributable Package Update 3 (x64 version) must be installed (required)
    -The above package is [here] (https://www.microsoft.com/ja-jp/download/details.aspx?id=53587)
    -After pressing the `Download` button, select` vcredist_x64.exe` and download and install.
    -If you cannot find it, try searching for "Visual C ++ 2015 Redistributable Package Update 3".

When converting with cuDNN,

 * GPU: NVIDIA GPU with Compute Capability 3.0 or higher

If you want to know the Compute Capability of your GPU, please check [this page] (https://developer.nvidia.com/cuda-gpus).


 About cuDNN
--------

cuDNN is a library for high-speed machine learning that can only be used with NVIDIA GPUs.
You can convert with CUDA without using cuDNN, but using cuDNN has the following advantages.

 * Images can be converted faster depending on the type of GPU used
 * VRAM usage can be reduced (at least less than half of CUDA. The difference increases as the partition size increases)

Although cuDNN has such advantages, the files required for operation cannot be distributed due to the license.
So, if you want to use cuDNN, download the binary for Windows (v5.1 RC or later) from [this page] (https://developer.nvidia.com/cuDNN),
Please put "cudnn64_7.dll" in the waifu2x-caffe folder.
In addition, if you put the dll while running the software, please restart the software.
(To download cuDNN, you need to register with NVIDIA Developer and CUDA Registered Developers.
 CUDA Registered Developers probably have a (simple) review, so registering doesn't mean you can download cuDNN right away. )

The measurement results of processing speed and VRAM usage in the author's environment are as follows.

 * GPU: GTX 980 Ti
 * VRAM: 6GB
 * Processing: Noise removal and enlargement with 1000 * 1000 PNG 4ch image, JPEG noise removal level 1, enlargement ratio 2.00, TTA mode not used
 * Processing time measurement method: Measure the average processing time of 10 times with CUI version. However, first perform the process twice in advance (to avoid including the time required for initialization)
 * VRAM usage calculation method: (Maximum VRAM used during processing in GUI version)-(VRAM usage after starting GUI version)

cuDNN RGB model

| Partition size | Processing time | VRAM usage (MB) |
|: ----------- |: ------------- |: ------------------- |
| 100 | 00: 00: 03.170 | 278 |
| 125 | 00: 00: 02.745 | 279 |
| 200 | 00: 00: 02.253 | 365 |
| 250 | 00: 00: 02.147 | 446 |
| 500 | 00: 00: 01.982 | 1110 |

CUDA RGB model

| Partition size | Processing time | VRAM usage (MB) |
|: ----------- |: ------------- |: ------------------- |
| 100 | 00: 00: 06.192 | 724 |
| 125 | 00: 00: 05.504 | 724 |
| 200 | 00: 00: 04.642 | 1556 |
| 250 | 00: 00: 04.436 | 2345 |
500 | Measurement not possible | Measurement not possible (6144 or more) |

cuDNN UpRGB model

| Partition size | Processing time | VRAM usage (MB) |
|: ----------- |: ------------- |: ------------------- |
| 100 | 00: 00: 02.831 | 328 |
| 125 | 00: 00: 02.573 | 329 |
| 200 | 00: 00: 02.261 | 461 |
| 250 | 00: 00: 02.150 | 578 |
| 500 | 00: 00: 01.991 | 1554 |

CUDA UpRGB model

| Partition size | Processing time | VRAM usage (MB) |
|: ----------- |: ------------- |: ------------------- |
| 100 | 00: 00: 03.669 | 788 |
| 125 | 00: 00: 03.382 | 787 |
| 200 | 00: 00: 02.965 | 1596 |
| 250 | 00: 00: 02.852 | 2345 |
500 | Measurement not possible | Measurement not possible (6144 or more) |


 How to use (GUI version)
--------

"Waifu2x-caffe.exe" is GUI software. Start by double clicking.
Alternatively, if you drag and drop a file or folder into “waifu2x-caffe.exe” using Explorer, the file will be converted using the settings from the previous startup.
In that case, depending on the settings, the dialog is automatically closed when the conversion is successful.
You can also set options on the command line using the GUI.
For details, please read the section on command line options (common) and command line options (GUI version).

After startup, drag and drop an image or folder into the "Input path" field and the "Output path" field will be set automatically.
If you want to change the output destination, change the "Output path" field.

You can change the conversion settings to your liking.


## Input / output setting
     Setting items related to file input / output.

### "Input path"
     Specify the path of the file you want to convert.
     If a folder is specified, files with "extension to convert in folder", including those in subfolders, will be converted.
     You can specify multiple files and folders by dragging.
     In that case, the file and the folder structure will be output in the new folder.
    ("(Multi Files)" is displayed in the input path field. The output folder name is generated from the file and folder name that was grasped with the mouse.)
     When selecting a file by pressing the browse button, you can select a single file, folder, or multiple files.

### "Output path"
     Specify the path to save the converted image.
    If you specify a folder in "Input path", save the converted file (with the folder structure as it is) in the specified folder. If the specified folder does not exist, it will be created automatically.

### "Extension to be converted in folder"
     If "input path" is a folder, specify the extension of the image to be converted in the folder.
     The default value is `png: jpg: jpeg: tif: tiff: bmp: tga`.
     The delimiter is `:`.
     Case is not significant.
     Example.png: jpg: jpeg: tif: tiff: bmp: tga

### "Output extension"
     Specify the format of the converted image.
    The values ​​that can be set for “Output Quality Setting” and “Output Depth Bits” differ depending on the format specified here.

### “Output quality setting”
     Specify the image quality of the converted image.
     Possible values ​​are integers.
     The range and meaning of the specifiable values ​​depend on the format set in "Output extension".

      * .jpg: Value range (0-100) The higher the number, the higher the image quality
      * .webp: Value range (1 to 100) Higher number means higher quality 100, lossless compression
      * .tga: Value range (0 to 1) 0 means no compression, 1 means RLE compression

### "Output Depth Bits"
     Specify the number of bits per channel of the converted image.
     The value that can be specified depends on the format set in “Output extension”.

## Conversion image quality / processing settings
     Setting items related to file conversion processing method and image quality.

### "Conversion mode"
     Specify the conversion mode.
      * Noise reduction and enlargement: Performs noise reduction and enlargement
      * Magnify: Enlarge
      * Noise Removal: Performs noise removal
      * Noise reduction (automatic detection) and enlargement: Enlarge. Performs noise reduction only when the input is a JPEG image

### "JPEG noise removal level"
    Specify the noise reduction level. Higher levels remove more noise, but may result in a more subtle picture.

### "Enlarged size"
    Set the size after enlargement.
      * Specify by magnification: Enlarges the image at the specified magnification
      * Specify the width after conversion: Enlarge the image to the specified width while maintaining the aspect ratio of the image (unit is pixels)
      * Specify the height after conversion: Enlarge the image to the specified height while maintaining the aspect ratio of the image (in pixels)
      * Specify by vertical and horizontal width after conversion: Enlarge to the specified vertical and horizontal width. Specify as "1920x1080" (unit is pixel)
    If the magnification is more than 2 times (when removing noise, do it only once), enlarge by 2 times until the specified magnification is exceeded, and if the magnification is not a power of 2, reduce it at the end Process. As a result, the result of the conversion may be a slender picture.

### "model"
    Specify the model to use.
    The best model depends on the image to be converted, so we recommend you to try various models.
      * 2D illustration (RGB model): 2D illustration model that converts all RGB of the image
      * Photo / Anime (Photo model): Photo / Anime model
      * 2D illustration (UpRGB model): A model that converts faster than 2D illustration (RGB model) with the same or better image quality. However, the amount of memory (VRAM) consumed is larger than that of the RGB model.
      * Photo / Animation (UpPhoto model): A model that converts faster than or equal to the image quality of Photos / Anime (Photo model). However, since the amount of memory (VRAM) that is consumed is larger than the Photo model, adjust the division size if forced termination during conversion
      * 2D illustration (Y model): 2D illustration model that converts only the brightness of the image
      * 2D illustration (UpResNet10 model): A model that converts with higher image quality than 2D illustration (UpRGB model). Note that this model will produce different results if the split size is different
      * 2D illustration (CUnet model): A model that can convert 2D illustrations with the highest image quality among the included models. Note that this model will produce different results if the split size is different

### "Use TTA mode"
    Specify whether to use TTA (Test-Time Augmentation) mode.
    Using TTA mode, the conversion is 8 times slower, but the PSNR (one of the image evaluation indices) seems to increase by about 0.15.

## Processing speed setting
     A group of setting items that affect the processing speed of image conversion.

### "Split size"
    Specifies the width (in pixels) for processing by dividing internally.
    How to determine the optimal value (the processing ends at the fastest) will be explained in the section "Division size".
    The upper part delimited by "-------" is a divisor of the vertical and horizontal size of the input image,
    The lower one is a general-purpose division size read from "crop_size_list.txt".
    If the partition size is too large, be careful as the amount of required memory (or the amount of VRAM when using GPU) exceeds the memory available on the PC and the software will forcibly terminate.
    When processing a large number of images of the same image size by specifying a folder, it is recommended to check the optimal division size before converting, as it will affect the processing speed accordingly.
    However, be aware that changing the split size may change the output result depending on the model.
    (In that case, use the default split size and adjust the batch size to increase the processing speed)

### "Batch size"
    Specify the size when processing inside collectively.
    Larger batch sizes may increase processing speed.
    Make sure that the amount of memory required as well as the partition size does not exceed the memory available on the PC.

## Operation setting
     This is a group of settings that summarizes operation settings that are unlikely to be changed.

### "Automatic conversion start setting at file input"
     Set whether to automatically start conversion when an input file is specified using the reference button or drag and drop.
     The setting of this item has no effect if the input file is given to the exe as an argument.
      * Do not start automatically: Does not start conversion automatically even if you enter the file
      * Start after inputting one file: Conversion starts automatically after inputting one file
      * Start after inputting a folder or multiple files: Automatically start conversion after inputting a folder or multiple files. Please convert a single image file only when adjusting the conversion settings.

### "Processor used"
    Specifies the processor that performs the conversion.
      * CUDA (cuDNN if available): Performs conversion using CUDA (GPU) (if cuDNN is available, cuDNN is used)
      * CPU: Perform conversion using only CPU

### "Do not overwrite output files"
     When this setting is ON, conversion is not performed if a file with the same name exists in the image writing destination.

### "Startup settings with arguments"
     Set the operation when an input file is given as an argument to the exe.
      * Convert at startup: Start conversion automatically at startup
      * Exit on success: Automatically exit if not failed at the end of conversion

### “Use GPU No”
     You can specify the device number to use when there are multiple GPUs. In CPU mode or when an invalid device number is specified, it is ignored.

### "Fixed folder for input reference"
     The folder displayed first when you press the input reference button is fixed to the folder set here.

### "Fixed folder when viewing output"
     Fix the output destination folder of the converted image to the folder set here.
     Also, fix the folder displayed first when you click the output browse button to the folder set here.

## Other
     Other setting items.

### "UI language"
    Set the UI language.
    At the first startup, the same language as the PC language setting is selected. (If it does not exist, it will be in English)

### "cuDNN check"
    You can check if cuDNN can be used by pressing the "cuDNN check" button.
    If cuDNN cannot be used, the reason is displayed.

Press the "Execute" button to start the conversion.
If you want to cancel on the way, press "Cancel" button.
However, there is a time lag until it actually stops.
The progress bar indicates the degree of progress when changing multiple images.
The estimated time remaining is displayed in the log, which is an estimate when processing multiple files with the same vertical and horizontal width.
Therefore, it is not useful when the size of the file varies, and when the number of images to be processed is two or less, only "Unknown" is displayed.


 Usage (CUI version)
--------

"Waifu2x-caffe-cui.exe" is a command line tool.
Start up `Command Prompt`, and enter the command as follows.


The following command prints usage information to the screen.
`` `
waifu2x-caffe-cui.exe --help
`` `

The following command is an example of the command to execute image conversion.
`` `
waifu2x-caffe-cui.exe -i mywaifu.png -m noise_scale --scale_ratio 1.6 --noise_level 2
`` `
If you execute the above, the conversion result will be saved to `mywaifu (noise_scale) (Level2) (x1.600000) .png`.

For details on the command list and each command, see the sections on Command Line Options (Common) and Command Line Options (CUI Version).


 Command line options (common)
--------

In this software, the following options can be specified.
In the GUI version, if the command line option other than the input file is started and specified, the option file is not saved currently.
For options not specified in the GUI version, the options at the time of the previous termination are used.

### -l <string>, --input_extention_list <string>
     If input_file is a folder, specify the extension of the image to be converted in the folder.
     The default value is `png: jpg: jpeg: tif: tiff: bmp: tga`.
     The delimiter is `:`.
     Example.png: jpg: jpeg: tif: tiff: bmp: tga

### -e <string>, --output_extention <string>
     Specify the extension of the output image when input_file is a folder.
     The default value is `png`.

### -m <noise | scale | noise_scale>, --mode <noise | scale | noise_scale>
     Specify the conversion mode. If not specified, `noise_scale` will be selected.
      * noise: Performs noise removal (accurately, performs image conversion using a model for noise removal)
      * scale: scales up (more precisely, scales up with the existing algorithm, then converts the image using the model for complementing the scaled up image)
      * noise_scale: Performs noise reduction and enlargement (after noise reduction, enlargement processing is continued)
      * auto_scale: Scale up. Performs noise reduction only when the input is a JPEG image

### -s <decimal number>, --scale_ratio <decimal number>
     Specify how many times to enlarge the image. The default value is `2.0`, but you can specify anything other than 2.0.
     If scale_width or scale_height is specified, that one has priority.
     If a value other than 2.0 is specified, the following processing is performed.
      * First, repeat the 2x magnification to cover the specified magnification as necessary and sufficient.
      * If a value other than a power of 2 is specified, the image will be reduced to the specified magnification.

### -w <integer>, --scale_width <integer>
     Enlarges the image to the specified width while maintaining the aspect ratio of the image (in pixels).
     If specified at the same time as scale_height, the image will be enlarged to the specified height and width.

### -h <integer>, --scale_height <integer>
     Enlarges the image to the specified height while maintaining the image's aspect ratio (in pixels).
     If specified at the same time as scale_width, the image will be enlarged to the specified height and width.

### -n <0 | 1 | 2 | 3>, --noise_level <0 | 1 | 2 | 3>
     Specify the noise reduction level. Only noise levels 0 to 3 are available for noise reduction models.
     Specify 0, 1, 2, or 3.
     The default value is `0`.

### -p <cpu | gpu | cudnn>, --process <cpu | gpu | cudnn>
     Specify the processor to use for processing. The default value is `gpu`.
      * cpu: Performs conversion using the CPU.
      * gpu: Performs conversion using CUDA (GPU). If you can use cuDNN only on Windows version, use cuDNN.
      * cudnn: Performs conversion using cuDNN.

### -c <integer>, --crop_size <integer>
     Specify the division size. The default value is `128`.

### -q <integer>, --output_quality <integer>
     Set the image quality of the converted image. Default value is `-1`
     The specifiable values ​​and their meanings depend on the format set in "Output Extension".
     If -1, the default value for each image format is used.

### -d <integer>, --output_depth <integer>
     Specify the number of bits per channel of the converted image. The default value is `8`.
     The value that can be specified depends on the format set in “Output extension”.

### -b <integer>, --batch_size <integer>
     Specify mini-batch size. The default value is `1`.
     The mini-batch size is the number of blocks that are processed at the same time by dividing the image by the "divided size". For example, if you specify `2`, it will convert two blocks at a time.
     Increasing the mini-batch size will increase the GPU usage rate, just as increasing the split size, but if you measure it, increasing the split size is more effective.
     (For example, it is faster to set the split size to `128` and the mini-batch size to` 1` than to set the split size to `64` and the mini-batch size to` 4`.)

### --gpu <integer>
     Specify the GPU device number to be used for processing. The default value is `0`.
     Note that GPU device numbers start at 0.
     It is ignored if you do not use the GPU for processing.
     If a non-existent GPU device number is specified, it will be executed on the default GPU.

### -t <0 | 1>, --tta <0 | 1>
     Specify `1` to use TTA mode. The default value is `0`.

###-, --ignore_rest
     Ignore all options after this option is specified.
     For scripts and batch files.


 Command line options (GUI version)
--------

In the GUI version, arguments that do not apply to option specification are recognized as input files.
Input files can be specified for files, folders, multiple files and folders at the same time.

### -o <string>, --output_folder <string>
     Set the path to the folder where you want to save the converted image.
     Save the converted file in the specified folder.
     The naming rule of the converted file is the same as the output file name that is automatically determined when setting the input file in the GUI.
     If not specified, it will be saved in the same folder as the first input file.

### --auto_start <0 | 1>
     If you specify `1`, conversion starts automatically at startup.

### --auto_exit <0 | 1>
     If `1` is specified, if the conversion is successful at the time of startup and the conversion is successful, it will end automatically.

### --no_overwrite <0 | 1>
     If `1` is specified, conversion will not be performed if a file with the same name exists in the image writing destination.

### -y <upconv_7_anime_style_art_rgb | upconv_7_photo | anime_style_art_rgb | photo | anime_style_art_y | upresnet10 | cunet>, --model_type <upconv_7_anime_style_art_rgb | upconv_7_photo | anime_style_art_rgb | photo | style |
     Specify the model to use.
     Corresponds to the setting item "Model" in the GUI as follows.
      * upconv_7_anime_style_art_rgb: 2D illustration (UpRGB model)
      * upconv_7_photo: Photo / Animation (UpPhoto model)
      * anime_style_art_rgb: 2D illustration (RGB model)
      * photo: Photo / Anime (Photo model)
      * anime_style_art_y: 2D illustration (Y model)
      * upresnet10: 2D illustration (UpResNet10 model)
      * cunet: 2D illustration (CUnet model)


 Command line option (CUI version)
--------

### --version
     Output version information and exit.

###-?, --help
     Show usage and exit.
     Please when you want to easily check how to use.

### -i <string>, --input_file <string>
     (Required) Path to the image to convert
     If a folder is specified, all image files under that folder are converted and output to the folder specified by output_file.

### -o <string>, --output_file <string>
     Path to file to save converted image
     (if input_file is a folder) Path to folder to save converted image
     (When input_file is an image file) Be sure to enter the extension (such as the last .png).
     If not specified, the file name is determined automatically and saved in that file.
     The rule for determining the file name is
     `[Original image file name]` `(model name) '' (mode name)` `(noise removal level (in noise removal mode))` `(enlargement ratio (in enlargement mode))` `(output Number of bits (other than 8 bits)) `` .output extension`
     It is like.
     The save location is basically the same directory as the input image.

### --model_dir <string>
     Specify the path to the directory where the model is stored. The default value is `models / cunet`.
     The following models are attached as standard.
      * `models / anime_style_art_rgb`: 2D illustration (RGB model)
      * `models / anime_style_art`: 2D illustration (Y model)
      * `models / photo`: Photo / Anime (Photo model)
      * `models / upconv_7_anime_style_art_rgb`: 2D illustration (UpRGB model)
      * `models / upconv_7_photo`: Photo / Animation (UpPhoto model)
      * `models / upresnet10`: 2D illustration (UpResNet10 model)
      * `models / cunet`: 2D illustration (CUnet model)
      * `models / ukbench`: Old-fashioned photographic model (only the enlarged model is included; noise cannot be removed)
     Basically, it is not necessary to specify. Specify this when using a model other than the default or your own model.

### --crop_w <integer>
     Specify the division size (width). If not set, the value of crop_size is used.
     If you specify a divisor of the width of the input image, conversion may be faster.

### --crop_h <integer>
     Specify the division size (vertical width). If not set, the value of crop_size is used.
     If you specify a divisor of the vertical width of the input image, conversion may be faster.


 Division size
--------

When converting images, waifu2x-caffe (although waifu2x also)
The process of dividing the image by a certain size, converting it one by one, and finally combining and returning to one image.
The division size (crop_size) is the width (in pixels) when dividing this image.

If the GPU is not used up during conversion with CUDA (GPU usage rate is not close to 100%),
Increasing this number may result in faster processing. (Because the GPU can be used up)
[GPU-Z] (http://www.techpowerup.com/gpuz/) Please adjust while checking GPU Load (GPU usage rate) and Memory Used (VRAM usage rate).
Also, please refer to the following characteristics.

 * Higher numbers do not necessarily mean faster
 * If the division size is a divisor of the vertical and horizontal size of the image (or a number that has a small remainder when divided), the amount of wasteful calculation is reduced and the speed is increased. (It seems that there is a case where the numerical value that does not apply to this condition is fastest?)
 * If you double the number, theoretically the memory used will be quadrupled (actually 3-4 times), so be careful not to lose the software. In particular, be aware that CUDA consumes much more memory than cuDNN


 About images with alpha channel
--------

This software supports enlargement of images with alpha channel.
However, please note that it takes about twice as long to enlarge the image with alpha channel because it is the process to enlarge the alpha channel by itself.
However, if the alpha channel is composed of a single color, it can be expanded in almost the same time as without.


 The format of language files
--------

Language files format is JSON.
If you create new language file, add language setting to 'lang / LangList.txt'.
'lang / LangList.txt' format is TSV (Tab-Separated Values).

  * LangName: Language name
  * LangID: Primary language [See MSDN] (https://msdn.microsoft.com/en-us/library/windows/desktop/dd318693.aspx)
  * SubLangID: Sublanguage [See MSDN] (https://msdn.microsoft.com/en-us/library/windows/desktop/dd318693.aspx)
  * FileName: Language file name

ex.

  * Japanese LangID: 0x11 (LANG_JAPANESE), SubLangID: 0x01 (SUBLANG_JAPANESE_JAPAN)
  * English (US) LangID: 0x09 (LANG_ENGLISH), SubLangID: 0x01 (SUBLANG_ENGLISH_US)
  * English (UK) LangID: 0x09 (LANG_ENGLISH), SubLangID: 0x02 (SUBLANG_ENGLISH_UK)


Acknowledgment
------------

This software is not guaranteed.
Use it at your discretion.
The author has no obligation.


Acknowledgments
------
Produced the original [waifu2x] (https://github.com/nagadomi/waifu2x) and a model and released it under the MIT license [ultraist] (https://twitter.com/ultraistter) ,
オリジナルのwaifu2xを元に[waifu2x-converter](https://github.com/WL-Amigo/waifu2x-converter-cpp)を作成して下さった [アミーゴ](https://twitter.com/WL_Amigo)さん(READMEやLICENSE.txtの書き方、OpenCVの使い方等かなり参考にさせていただきました)  
[waifu2x-chainer](https://github.com/tsurumeso/waifu2x-chainer)を作成してオリジナルのモデルの制作を行い、MITライセンスの下で公開して下さった[tsurumeso](https://github.com/tsurumeso)さん  
に、感謝します。  
また、メッセージを英訳してくださった @paul70078 さん、メッセージを中国語(簡体字)に翻訳してくださった @yoonhakcher さん、中国語(簡体字)訳のプルリクエストを下さった @mzhboy さん、  
メッセージを韓国語に翻訳してくださった @kenin0726 さん、韓国語訳の改善を提案してくださった @aruhirin さん、  
メッセージを中国語(繁体字)に翻訳してくださった @lizardon1995 さん、@yoonhakcher さん、トルコ語訳のプルリクエストを下さった @Scharynche さん、フランス語訳のプルリクエストを下さった @Serized さん、  
GUI版のアイコンを提供してくださった JYUNYAさん に感謝します。
