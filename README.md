<p align="center">
  <img src="https://raw.githubusercontent.com/yeahdongcn/RSBarcodes_Swift/master/home-hero-swift-hero.png">
</p>

RSBarcodes, now Swift.
==========
[![Total views](https://sourcegraph.com/api/repos/github.com/yeahdongcn/RSBarcodes_Swift/counters/views.png)](https://sourcegraph.com/github.com/yeahdongcn/RSBarcodes_Swift)
[![Views in the last 24 hours](https://sourcegraph.com/api/repos/github.com/yeahdongcn/RSBarcodes_Swift/counters/views-24h.png)](https://sourcegraph.com/github.com/yeahdongcn/RSBarcodes_Swift)

RSBarcodes allows you to read 1D and 2D barcodes using metadata scanning capabilities introduced with iOS 7 and generate the same set of barcode images for displaying and sharing. Now Swift.

* Objective-C version. [RSBarcodes](https://github.com/yeahdongcn/RSBarcodes)

##TODO

###Generators
- [x] Code39
- [x] Code39Mod43
- [x] ExtendedCode39
- [x] Code93
- [x] Code128
- [x] UPCE
- [x] EAN FAMILIY (EAN8 EAN13 ISBN13 ISSN13)
- [x] ITF14
- [x] Interleaved2of5
- [ ] DataMatrix
- [x] PDF417
- [x] QR
- [x] Aztec
- [x] Views

###Reader
- [x] Views
- [x] ReaderController

##Installation

1. Add RSBarcodes_Swift as a [submodule](http://git-scm.com/docs/git-submodule) by opening the Terminal, `cd`-ing into your top-level project directory, and entering the command `git submodule add https://github.com/yeahdongcn/RSBarcodes_Swift.git`
2. Open the `RSBarcodes_Swift` folder, and drag `RSBarcodes.xcodeproj` into the file navigator of your app project.
3. In Xcode, navigate to the target configuration window by clicking on the blue project icon, and selecting the application target under the "Targets" heading in the sidebar.
4. Ensure that the deployment target of RSBarcodes.framework matches that of the application target.
5. In the tab bar at the top of that window, open the "Build Phases" panel.
6. Expand the "Target Dependencies" group, and add `RSBarcodes.framework`.
7. Click on the `+` button at the top left of the panel and select "New Copy Files Phase". Rename this new phase to "Copy Frameworks", set the "Destination" to "Frameworks", and add `RSBarcodes.framework`.

##Usage

###Generators

The simplest way to use the generators is:

    RSUnifiedCodeGenerator.shared.generateCode("2166529V", machineReadableCodeObjectType: AVMetadataObjectTypeCode39Code)

It will generate a UIImage instance if the `2166529V` is a valid code39 string. For AVMetadataObjectTypeCode128Code, you can change `useBuiltInCode128Generator` to `false` to use my implementation (AutoTable for code128).

P.S. There are 4 tables for encoding a string to code128, `TableA`, `TableB`, `TableC` and `TableAuto`; the `TableAuto` is always the best choice, but if one has specific requirements, try this:

    RSCode128Generator(codeTable: .A).generateCode("123456", machineReadableCodeObjectType: AVMetadataObjectTypeCode128Code)

Example of these simple calls can be found in the test project.

###Reader

Place a `UIViewController` in storyboard and set `RSCodeReaderViewController` as its custom class, and you're almost there, as the focus mark layer and corners layer are already there working for you. There are two handlers: one for the single tap on the screen (along with the focus mark), and the other is the detected objects handler, which ensures that all detected objects will come to you. Set them up in `viewDidLoad()` or some other place more suitable for your project:

    override func viewDidLoad() {
        super.viewDidLoad()
        
        self.focusMarkLayer.strokeColor = UIColor.redColor().CGColor
        
        self.cornersLayer.strokeColor = UIColor.yellowColor().CGColor
        
        self.tapHandler = { point in
            println(point)
        }
        
        self.barcodesHandler = { barcodes in
            for barcode in barcodes {
                println(barcode)
            }
        }
    }
    
If you want to ignore some code types, you'd better add the following lines:

    let types = NSMutableArray(array: self.output.availableMetadataObjectTypes)
    types.removeObject(AVMetadataObjectTypeQRCode)
    self.output.metadataObjectTypes = NSArray(array: types)
    
###Helper

Try `RSAbstractCodeGenerator.resizeImage(<#source: UIImage#>, scale: <#CGFloat#>)` to scale the generated image.

##Miscellaneous

[The Swift Programming Language 中文版](https://github.com/numbbbbb/the-swift-programming-language-in-chinese/)

[Online version](http://numbbbbb.github.io/the-swift-programming-language-in-chinese/) generated using [GitBook](https://www.gitbook.io/)

##License

    The MIT License (MIT)

    Copyright (c) 2012-2014 P.D.Q.

    Permission is hereby granted, free of charge, to any person obtaining a copy of
    this software and associated documentation files (the "Software"), to deal in
    the Software without restriction, including without limitation the rights to
    use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of
    the Software, and to permit persons to whom the Software is furnished to do so,
    subject to the following conditions:

    The above copyright notice and this permission notice shall be included in all
    copies or substantial portions of the Software.

    THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
    IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS
    FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR
    COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER
    IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN
    CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
