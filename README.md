# GIFmaker

We are going to create a colorful GIF using Swift and Swift Playgrounds. 🍭

**Step 1 - Creating a playground**

Open Xcode and select the first option in the left *Get started with a playground*
Make sure the *Blank* template is selected and click *Next*.
Give it a name that represents what you are doing and a location in your computer then continue. 

Now delete the two lines of content there so we can start from scratch.

**Step 2 - The code**

We need to import some frameworks to make this work. `UIKit` provides the required infrastructure for your iOS apps while `PlaygroundSuppor`t gives us tools to make the playground experience better.

Add this at the top of your file.

```Swift
import UIKit
import PlaygroundSupport
```

We need a few constants, add them as you learn what they do.

Our GIF will transition from one color to another. `frameCount` is the number of frames that transition is going to take. Let’s start with a value of 50.

`let frameCount = 50`

`width` and `height` will determine the size of your final GIF.

```Swift
let width = 400
let height = 400
```

Now let’s create the animation. Start typing this:

`let animation = Animation.cr` and stop right there. We should be looking at some options as autocomplete. We are going to use the first one so just hit enter.  

You should be looking at a function with placeholders for us to fill with the variables that we want.

You will also notice it turns red and marks an error. If you click on the red circle at the right it will tell you what’s wrong. Basically the function might fail and it’s asking you to fix it.

Change your code to this to include the variables and get rid of the error. Don’t worry at this time if you don’t understand fully what’s each component doing.

```Swift

let animation2 = try Animation.createAutoReversedLoop(frameCount, width: width, height: height, frameDelay: 0.1) { (idx, context) in
    
}
```

Next add this after in and before the closing bracket.

```Swift
 let progress: CGFloat = CGFloat(idx) / CGFloat(frameCount)
    let from =  colorLiteral(red: 0.4050287008, green: 0.3449084759, blue: 0.8464239836, alpha: 1)
    let to =  colorLiteral(red: 0, green: 0.7610777617, blue: 0.9553645253, alpha: 1)
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
```

That handles the interpolation between the two colors using hue, saturation and brightness. It’s changing the colors over time. The constants to and from are the two main colors, try changing them to any colors you want by modifying the RGB values.

That’s it for the `createAutoReversedLoop` function.

Below everything we’ve done. Add this:

```Swift

// This is one way in which UIKit let’s you show elements in your apps. Using an instance of UIImageView. You give it  an image, in this case it will come from our animation that we did before.

let imageView = UIImageView(image: animation.animatedImage())
imageView.frame = CGRect(x: 0, y: 0, width: width, height: height)
PlaygroundPage.current.liveView = imageView

// You can write result out as an animated GIF and save it in the device

 let resultURL = playgroundSharedDataDirectory.appendingPathComponent(“result.gif”)
 let GIFData = animation.animatedGIFRepresentation()
 do {
     try GIFData.write(to: resultURL)
 } catch {
     print(“Error Writing File: \(error.localizedDescription)”)
 }
```

We are done! How can we see the result?

**Step 3 - Live View**

To see the result click on the second option from the three buttons at the top right corner. It looks like a Venn Diagram. Ask if you can’t find it. This will open the Live View.

Now click the play button at lower left corner and see your GIF come to life. 

It should work by now. Ask for help if you don’t get the desired result. 

**Step 4 - Saving**

But wait, we get an error in the debug console. It says the GIF couldn’t be saved. This is because the saving is trying to happen in this specific directory `~/Documents/Shared Playground Data/`
You probably don’t have this folder. So go ahead and create it. Then run your code again. Go back to the folder and see your GIF there. NOW you’re done. 😎

Now that you are done, you can play around with the values of the colors and the number of frames. Be careful not to have this number too low if you are sensitive to light & color changes happening too fast.


From: https://github.com/danielrhammond/GIF-Playground
