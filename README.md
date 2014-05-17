# Mobile Icon Resizer 

This tool can be used to resize iOS and Android application icons in batch. That is, given a 1024x1024 icon, this tool will generate all necessary icon sizes.

## Dependencies

The tool itself is a Node.js app/module so you'll need to have Node.js v0.8+ installed.

The image resizing is done with [ImageMagick](http://www.imagemagick.org/). Make sure you have ImageMagick's `convert` command available in the command line.

## Installation

To install the tool as another Node.js module's dependency, run the following command:

    npm install mobile-icon-resizer

To install it globally as an executable binary, install it as follows:

    npm install mobile-icon-resizer -g

## Usage

This tool can either be used as a standalone application or as a module directly from within another Node.js module.

### Module

    var resize = require('mobile-icon-resizer');
    var options = {
      // Your options here
    };
    resize(options, function (err) {
    });

The `resize()` function's `options` argument takes the following optional parameters:

* **platformsToBuild**: For which platforms should the icons be resized. Comma-separated list. Possible values ['ios', 'android']
* **originalIconFilename**: The original image's relative path and filename such as '../someIcon.png'. Default: 'appicon_1024.png'.
* **iosFilenamePrefix**: The prefix of the iOS image files. Default: 'Icon'.
* **iosOutputFolder**: The output folder for the iOS icons. Default: '.'.
* **androidOutputFolder**: The output folder for the Android icons. Default: '.'.
* **androidOutputFilename**: The output file name for the Android icons.
* **config**: Optional path to a `.js` or `.json` file that defines the thumbnail size configuration. Default: use the built-in `config.js` file.

### Standalone Application

You can run the tool as an application like this:

    mobile-icon-resizer OPTIONS

The application can write its usage documentation to the command line. To view it, run:

    mobile-icon-resizer -h

Example output:

    mobile-icon-resizer OPTIONS

    Options:
      --input, -i        The prefix of the iOS image files.                [default: "appicon_1024.png"]
      --iosprefix        The prefix of the iOS image files.                            [default: "Icon"]
      --iosof            The output folder for the iOS icons.                             [default: "."]
      --androidof        The output folder for the Android icons.                         [default: "."]
      --androidofn       The output file name for the Android icons.
      --platforms        For which platforms should the icons be resized. Comma-separated list.
                         Possible values: ios, android                          [default: "ios,android"]
      --config           A file with custom thumbnail sizes configuration.
      -v, --version      Print the script's version.
      -h, --help         Display this help text.

Example execution:

    mobile-icon-resizer -i appicon_1024.png --iosprefix="Icon" --iosof=output/ios --androidof=output/android --config=custom-sizes.js

## Thumbnail sizes configuration

By default, the thumbnail sizes that are generated are the ones defined in the provided [config.js](//github.com/muzzley/mobile-icon-resizer/blob/master/config.js) file.

You can optionally define a file with a custom set of thumbnail size settings and use that instead. The file is either a CommonJS JavaScript file or a plain JSON file.

### CommonJS JavaScript file

Example:

    var config = {
      iOS: {
        "images": [
          {
            "size" : "29x29",
            "idiom" : "iphone",
            "filename" : "-Small.png", // The filename will be prefixed with the provided iOS filename prefix
            "scale" : "1x"
          },
          {
            "size" : "29x29",
            "idiom" : "iphone",
            "filename" : "-Small@2x.png",
            "scale" : "2x"
          },
          // ...
          {
            "size" : "76x76",
            "idiom" : "ipad",
            "filename" : "-76@2x.png",
            "scale" : "2x"
          }
        ]
      },
      android: {
        "images" : [
          {
            "ratio" : "1/4",
            "folder" : "drawable-mdpi"
          },
          // ...
          {
            "ratio" : "1",
            "folder" : "drawable-xxxhdpi"
          }
        ]
      }
    };

    // Don't forget to export the config object!
    exports = module.exports = config;

### Plain JSON file

Example:

    {
      "iOS": {
        "images": [
          {
            "size" : "29x29",
            "idiom" : "iphone",
            "filename" : "-Small.png", // The filename will be prefixed with the provided iOS filename prefix
            "scale" : "1x"
          },
          // ...
          {
            "size" : "76x76",
            "idiom" : "ipad",
            "filename" : "-76@2x.png",
            "scale" : "2x"
          }
        ]
      },
      "android": {
        "images" : [
          {
            "ratio" : "1/4",
            "folder" : "drawable-mdpi"
          },
          // ...
          {
            "ratio" : "1",
            "folder" : "drawable-xxxhdpi"
          }
        ]
      }
    }
