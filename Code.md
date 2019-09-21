```Swift
import UIKit
import PlaygroundSupport

let frameCount = 50
let width = 400
let height = 400

let animation = try! Animation.createAutoReversedLoop(frameCount, width: width, height: height, frameDelay: 0.1) { idx, context in
    // Here's an example block that just interpolater between two colors using HSV (via http://stackoverflow.com/a/24687720)
    let progress: CGFloat = CGFloat(idx) / CGFloat(frameCount)
    let from = #colorLiteral(red: 0.4050287008, green: 0.3449084759, blue: 0.8464239836, alpha: 1)
    let to = #colorLiteral(red: 0, green: 0.7610777617, blue: 0.9553645253, alpha: 1)
    var h1: CGFloat = 0
    var s1: CGFloat = 0
    var b1: CGFloat = 0
    var a1: CGFloat = 0
    from.getHue(&h1, saturation: &s1, brightness: &b1, alpha: &a1)
    var h2: CGFloat = 0
    var s2: CGFloat = 0
    var b2: CGFloat = 0
    var a2: CGFloat = 0
    to.getHue(&h2, saturation: &s2, brightness: &b2, alpha: &a2)
    
    let fill = UIColor(
        hue: (h1 + (h2 - h1) * progress),
        saturation: (s1 + (s2 - s1) * progress),
        brightness: (b1 + (b2 - b1) * progress),
        alpha: (a1 + (a2 - a1) * progress))
    
    context.setFillColor(fill.cgColor)
    context.fill(CGRect(x: 0, y: 0, width: width, height: height))
}

let imageView = UIImageView(image: animation.animatedImage())
imageView.frame = CGRect(x: 0, y: 0, width: width, height: height)
PlaygroundPage.current.liveView = imageView

let resultURL = playgroundSharedDataDirectory.appendingPathComponent("result.gif")
let GIFData = animation.animatedGIFRepresentation()
do {
    try GIFData.write(to: resultURL)
} catch {
    print("Error Writing File: \(error.localizedDescription)")
}

```

From: https://github.com/danielrhammond/GIF-Playground
