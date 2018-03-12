# SwiftExtensions
A collection of Swift extensions

## Double

**`rounded(toPlaces places:)` usage:**

    let myFloat = 1.3589432
    myFloat.rounded(toPlaces: 1)


**`asString(style:) -> String`**

**Elapsed Time Example:**

    let elapsedSeconds: Double = 89032

    // "24h 43m 52s"
    elapsedSeconds.asString(style: .abbreviated)

    // "24hr 43min 52sec"
    elapsedSeconds.asString(style: .brief)

    // "24 hours, 43 minutes, 52 seconds"
    elapsedSeconds.asString(style: .full)

    // "24:43:52"
    elapsedSeconds.asString(style: .positional)

    // "24 hr, 43 min, 52 sec"
    elapsedSeconds.asString(style: .short)

    // "twenty-four hours, forty-three minutes, fifty-two seconds"
    elapsedSeconds.asString(style: .spellOut)

    extension Double {
    
        /// Rounds Double value
        func rounded(toPlaces places:Int) -> Double {
            let divisor = pow(10.0, Double(places))
            return (self * divisor).rounded() / divisor
        }
                
        func asString(style: DateComponentsFormatter.UnitsStyle) -> String {
            let formatter = DateComponentsFormatter()
            formatter.allowedUnits = [.hour, .minute, .second]
            formatter.unitsStyle = style
            guard let formattedString = formatter.string(from: self) else { return "" }
            return formattedString
        }
    }

## Floating Point

**isInt: Bool usage**

    let floatOne = 1.25
    floatOne.isInt // false

    let floatTwo = 1.00
    floatTwo.isInt // true

    extension FloatingPoint {
        /// Determine if a Floating Point number is an Int
        var isInt: Bool {
            return floor(self) == self
        }
    }

## UIView

This extension can be used for classes that descend from `UIView`. If you want to fade a `UILabel` update, here's what the code would look like:

    yourLabel.fadeTransition(0.25)
    yourLabel.text = "Updated Text"

    extension UIView {
        /// Usage: insert view.fadeTransition right before changing content
        func fadeTransition(_ duration:CFTimeInterval) {
            let animation = CATransition()
            animation.timingFunction = CAMediaTimingFunction(name:
            kCAMediaTimingFunctionEaseInEaseOut)
            animation.type = kCATransitionFade
            animation.duration = duration
            layer.add(animation, forKey: kCATransitionFade)
        }
    }

## String

These extensions can be used to figure out the height and width of a `String`. I've used for things like setting the width of a `UISegmentedControl`.

**Example Usage:**

    let myLabel = UILabel()
    myLabel.text = "Testing Testing 123"

    // {w 109.418 h 14.32}
    myLabel.text?.sizeOfString(usingFont: UIFont.systemFont(ofSize: 12))
    // {w 155.944 h 21.48}
    myLabel.text?.sizeOfString(usingFont: UIFont.systemFont(ofSize: 18))

    // 109.41796875
    myLabel.text?.widthOfString(usingFont: UIFont.systemFont(ofSize: 12))
    // 155.9443359375
    myLabel.text?.widthOfString(usingFont: UIFont.systemFont(ofSize: 18))

    // 14.3203125
    myLabel.text?.heightOfString(usingFont: UIFont.systemFont(ofSize: 12))
    // 21.48046875
    myLabel.text?.heightOfString(usingFont: UIFont.systemFont(ofSize: 18))


    extension String {

        /// Calculates the CGSize of a string
        func sizeOfString(usingFont font: UIFont) -> CGSize {
            let fontAttributes = [NSAttributedStringKey.font: font]
            let size = self.size(withAttributes: fontAttributes)
            return size
        }
        
        /// Calculates the width of a string as a CGFloat
        func widthOfString(usingFont font: UIFont) -> CGFloat {
            let fontAttributes = [NSAttributedStringKey.font: font]
            let size = self.size(withAttributes: fontAttributes)
            return size.width
        }
        
        /// Calculates the height of a string as a CGFloat
        func heightOfString(usingFont font: UIFont) -> CGFloat {
            let fontAttributes = [NSAttributedStringKey.font: font]
            let size = self.size(withAttributes: fontAttributes)
            return size.height
        }
    }
