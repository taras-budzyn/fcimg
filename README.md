FCImg - FusionCharts to Image
=============================

FCImg is a third party tool to allow servers running the LAMP stack to export FusionCharts Charts as images without a need for a browser. The download includes a copy of the FusionCharts evaluation version.

**Please note that you still need to purchase a license of FusionCharts. FCImg only enables server-side export.**

### Requirements and Installation:

Download the appropriate package for your platform and extract it in a path that your PHP Scripts can read. Currently, only PHP 5.2 or above is supported. Shared hosts are supported as long as `safe_mode` is not emabled. (Enabling `safe_mode` is not recommended and most hosts turn this off).

Also, this package supports only licensed versions of FusionCharts 3.4 and above.

### Usage:

Before you proceed with using FCImg, you need to download wkhtmltoimage - a package that's used to generate the images. **You need to manually download and install wkhtmltoimage and it's not included with your fcimg download**. [Download wkhtmltoimage here](http://wkhtmltopdf.org/downloads.html)

First, require fcimg.php from your PHP script. Now call the fusioncharts_to_image function, with the following arguments in this order:

* `outputFilePath`: The full path to the output image. If this file already exists, it will be overwritten.
* `swfName`: The FusionCharts SWF filename which will be used to determine which type of chart to render.
* `inputString`: The Data String in XML or JSON format. Note that the converter will automatically determine whether the input is XML or JSON.
* `height`: The height of the chart in pixels
* `width`: The width of the chart in pixels
* `options`: This is an array to configure extra parameters. This is treated as a key-value set. However, 2 parameters are mandatory: `licensed_fusioncharts_js` and `licensed_fusioncharts_charts_js`
* `$options["licensed_fusioncharts_js"]`: (**is mandatory**) specify the path of `fusioncharts.js` available in the FusionCharts 3.4 download. This should be physical path on your hard disk.
* `$options["licensed_fusioncharts_charts_js"]`: (**is mandatory**) specify the path of `fusioncharts.charts.js` available in the FusionCharts 3.4 download. This should be physical path on your hard disk.
* `$options["imageType"]`: Specifies the type of the image (png/jpg). Default: png.
* `$options["quality"]`: The Image quality (0-100). Note that a higher quality might take longer to render. Default: 70.
* `$options["render_delay"]`: Specifies the render delay in milliseconds that will allow browser to completely render charts before script will take image of them. Default: 1000;
* `$options["wkhtmltoimage_path"]`: You can override the path to wkhtmltoimage to an absolute path where the binary is present in your system. Otherwise we try to auto detect the path

```
require "fcimg/fcimg.php";
fusioncharts_to_image (
    "/var/www/report.png",           // path to image
    "Column3D.swf",                  // SWF Name. SWF File not required
    $inputString,                    // the input XML String
    400, 500,                        // height and width
    array(                           // options
        'licensed_fusioncharts_js' => "C:\fusioncharts\fusioncharts.js", // REQUIRED: Path to licensed fusioncharts.js
        'licensed_fusioncharts_charts_js' => "C:\fusioncharts\fusioncharts.charts.js", // REQUIRED: Path to licensed fusioncharts.charts.js
        'imageType' => 'jpg',        // OPTIONAL: set image type as JPG
        'quality' => 75              // OPTIONAL: increase Quality
        'render_delay' => 2000       // OPTIONAL: increase render delay
        'wkhtmltoimage_path' => "D:\Program Files\wkhtmltox\bin\wkhtmltoimage.exe" // OPTIONAL: alternative wkhtmltoimage_path
    )
);
```

### Notes:

* The conversion engine waits 1 second before capturing the state of the chart. In the event that the animation hasn't completed, the result of the chart might not be appropriate. By disabling animation, you can ensure that the exporter provides optimum results.
* The conversion process takes upto 2-3 seconds on an average server. The call to fusioncharts_to_image blocks the thread. Thus it is highly not recommended to use this function inside a script that serves web pages.
* Instead, this is ideal for background scripts that generates reports, where there won't be concurrent calls to the exporter, and situations where the render time won't introduce latency
* If you're using a licensed version of FusionCharts you need to supply paths to both files: `fusioncharts.js` and `fusioncharts.charts.js`
