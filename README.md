# Swift ZPL

**Swift ZPL** provides a declarative interface to write ZPL faster, easier and safer.

```swift
ZPL {
    LabelHome(x: 0, y: 0)
    Field(x: 50, y: 50) {
        FieldData(text: "swift-zpl")
    }
}
```

# Usage

```swift
import ZPLBuilder

let zpl = ZPL {
    LabelHome(x: 0, y: 0)
    ChangeAlphanumericDefaultFont(font: "A", height: 50)
    Field(x: 50, y: 50) {
        FieldData(text: "hello")
    }
}

print(zpl.string) // ^XA^LH0,0^CFA,50,0^FO50,50^FDhello^FS^XZ
```

[See All Commands](https://github.com/scchn/swift-zpl/tree/main/Sources/ZPLBuilder/Commands)

# Examples

## Boxes

```swift
ZPL {
    LabelHome(x: 0, y: 0)
    
    let width = 50
    let spacing = 30
    
    for row in 0..<3 {
        for col in 0..<3 {
            let x = width * col + spacing * (col + 1)
            let y = width * row + spacing * (row + 1)
            let lineWidth = col == row ? width / 2 : 1
            
            Field(x: x, y: y) {
                GraphicBox(width: width, height: width, lineWidth: lineWidth)
            }
        }
    }
}
```

![boxes](https://github.com/scchn/swift-zpl/blob/main/Images/boxes.png)

## Barcodes

### Code 128

```swift
ZPL {
    LabelHome(x: 0, y: 0)
    Field(x: 50, y: 50) {
        BarcodeCode128(data: "hello30678", height: 100, interpretation: .bottom)
    }
}
```

![boxes](https://github.com/scchn/swift-zpl/blob/main/Images/code128.png)

### QR code

```swift
ZPL {
    LabelHome(x: 0, y: 0)
    Field(x: 50, y: 50) {
        BarcodeQR(data: "hello", size: 10)
    }
}
```

![boxes](https://github.com/scchn/swift-zpl/blob/main/Images/qr.png)

## Text & Image

```swift
let image: UIImage = ...
let labelWidth = Int(203 * 2.8)
let labelHeight = 203 * 3
let settings = ZPL {
    LabelHome(x: 0, y: 0)
    LabelReversePrint(enabled: true)
    ChangeAlphanumericDefaultFont(font: "A", height: 35)
}
let body = ZPL {
    Field(x: labelWidth / 2, y: 0) {
        GraphicBox(width: labelWidth / 2, height: labelHeight, lineWidth: labelWidth / 2)
    }
    
    let imageHeight = Int(Double(labelWidth) * image.size.height / image.size.width)
    
    Field(x: 0, y: 0) {
        GraphicField(image: image, size: .init(width: labelWidth, height: imageHeight))
    }
    
    Field(x: 0, y: imageHeight) {
        FieldBlock(width: labelWidth, lines: 1, justification: .center)
        FieldData(text: "Swift")
    }
}
let zpl = ZPL {
    settings
    body
}
```

![text&image](https://github.com/scchn/swift-zpl/blob/main/Images/text_image.png)

# More Commands

This package currently only contains a few commands, if you can't find what you need:

**Write a new command:**

```swift
struct NewCommand: ZPLCommandConvertible {
    var command: String {
        // command...
    }
}

ZPL {
    NewCommand()
}
``` 

**Open a pull request:**

Open a pull request to add a new command or improve/fix existing ones.
