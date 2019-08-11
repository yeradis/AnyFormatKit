![AnyFormatKit: Simple text formatting in Swift](https://github.com/luximetr/AnyFormatKit/blob/develop/Assets/anyformatkit.png)


[![CI Status](http://img.shields.io/travis/luximetr/AnyFormatKit.svg?style=flat)](https://travis-ci.org/luximetr/AnyFormatKit)
[![Version](https://img.shields.io/cocoapods/v/AnyFormatKit.svg?style=flat)](http://cocoapods.org/pods/AnyFormatKit)
[![Carthage Compatible](https://img.shields.io/badge/Carthage-compatible-4BC51D.svg?style=flat)](https://github.com/Carthage/Carthage)
[![License](https://img.shields.io/cocoapods/l/AnyFormatKit.svg?style=flat)](http://cocoapods.org/pods/AnyFormatKit)
[![Platform](https://img.shields.io/cocoapods/p/AnyFormatKit.svg?style=flat)](http://cocoapods.org/pods/AnyFormatKit)
![Swift](https://img.shields.io/badge/%20in-swift%204.0-orange.svg)

Text formatting framework written on Swift 4.0.

## Features

| |Features |
|-------------------|------------------------------------------------------------|
:performing_arts:| Convert string into formatted string and vice versa
:bicyclist:| Formatting text during typing
:hash:| Set format using '#' characters like '### ##-###'


## Example

To run the example project, clone the repo and run `pod install` from the Example directory first.

### Phone number example

![AnyFormatKitDemo: Simple text formatting in Swift](https://github.com/luximetr/AnyFormatKit/blob/develop/Assets/demo.gif)

### Currency example

![AnyFormatKitCurrencyDemo: Currency formatting in Swift](https://github.com/luximetr/AnyFormatKit/blob/develop/Assets/currencyDemo.gif)

## Requirements

- iOS 8.0+
- Swift 4.0+
- Xcode 9.0+

## Migration Guides

- [AnyFormatKit 0.2.0 MigrationGuide](https://github.com/luximetr/AnyFormatKit/blob/master/Documentation/AnyFormatKit%200.2.0%20MigrationGuide.md)
- [AnyFormatKit 1.0.0 MigrationGuide](https://github.com/luximetr/AnyFormatKit/blob/master/Documentation/AnyFormatKit%201.0.0%20MigrationGuide.md)

## Installation

### CocoaPods

AnyFormatKit is available through [CocoaPods](http://cocoapods.org). To install
it, simply add the following line to your Podfile:

```ruby
pod 'AnyFormatKit'
```

Then, run the following command:

```bash
$ pod install
```

### Carthage

To integrate AnyFormatKit into your Xcode project using Carthage, specify it in your `Cartfile`:

```ogdl
github "luximetr/AnyFormatKit" ~> 0.2.0
```

Run `carthage update` to build the framework and drag the built `AnyFormatKit.framework` into your Xcode project.

## Usage

### Import

```swift
import AnyFormatKit
```

### Formatting with TextFormatter

```swift
let phoneFormatter = DefaultTextFormatter(textPattern: "### (###) ###-##-##")
phoneFormatter.format("+123456789012") // +12 (345) 678-90-12

let customFormatter = DefaultTextFormatter(textPattern: "###-###custom###-###")
customFormatter.format("111222333444") // 111-222custom333-444
```

You can also set your own symbol in the pattern

```swift
let cardFormatter = DefaultTextFormatter(textPattern: "XXXX XXXX XXXX XXXX", patternSymbol: "X")
cardFormatter.format("4444555566667777") // 4444 5555 6666 7777
```

For string with different length

```swift
let formatter = DefaultTextFormatter(textPattern: "## ###-##")
formatter.format("1234") // 12 34
formatter.format("123456789") // 12 345-67
```

Unformatting

```swift
let formatter = DefaultTextFormatter(textPattern: "## ###-##")
formatter.unformat("99 888-77") // 9988877
```
### Formatting during typing

It is necessary to create TextInputController instance with formatter for formatting during typing. You need to implement `TextInput` protocol in your own `UITextField`/`UITextView`/something else or use ready solutions (`TextInputField`/`TextInputView`) by subclassing. It is necessary to set controllers's `textInput` property.

```swift
let textInputController = TextInputController()

let textInput = TextInputField() // or TextInputView or any TextInput
textInputController.textInput = textInput // setting textInput

let formatter = TextInputFormatter(textPattern: "### (###) ###-##-##", prefix: "+12")
textInputController.formatter = formatter // setting formatter
```
The controller listens `textInput(_:shouldChangeCharactersIn:replacementString:)` delegate method. But you can also add more than one delegate if needed. Methods of the delegates, that should return `Bool` value gather using `&&` operator. Therefore, if one of the delegates returns `false`, that means that `textInput` will receive `false`. If you want to send `true` to `textInput`, all delegates must return `true`.

You can set `allowedSymbolsRegex` to the formatter to filter input symbols with the RegEx. All symbols, that satisfy the RegEx will be available for typing in the `textInput`.
This property only applies to inputed symbols from the keyboard, but not to the prefix.

```swift
inputFieldFormatter.allowedSymbolsRegex = "[0-9]" // allowed only numbers
```

### Attributes for range

To set attributes for string at range use `addAttributes(_:range:)` method for `textInput`.
```swift
textInput.addAttributes([.foregroundColor : UIColor.lightGray], range: NSRange(location: 0, length: 3))
```

## Author

luximetr, alexandr.orlov@brander.ua

## License

AnyFormatKit is available under the MIT license. See the LICENSE file for more info.
