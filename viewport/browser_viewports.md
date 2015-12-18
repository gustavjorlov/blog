# What makes responsove design work?

In this brief introduction I would like to give you an understanding of the mechanisms that make responsive web design possible, as well as highlighting some pitfalls and inconsistencies at the market. I aim not to go into unnecessary details, but to explain concepts at a level web developers can benefit from.

## First off, what is a web browser?
A web browser is a computer program which lets the user view and interact with web pages. Technically, a browser (i.e. Google Chrome or Firefox) is of a computer program that runs natively on the current operating system. That program handles things like window management, tabs, bookmarks and more. It also uses a "rendering engine" to calculate what should be shown in the greater part of the interface. The browser is provided a url (usually the user writing in the address bar or clicking a link) which it passes to the rendering engine along with a surface that is used to paint the interface on.

Historically browsers tend to do the rendering task somewhat different. Which is a subject of its own, but let's stick to the subject and move on.

## Responsive web design
Responsive web design is, according to Wikipedia (https://en.wikipedia.org/wiki/Responsive_web_design) "an approach to web design aimed at crafting sites to provide an optimal viewing and interaction experience — easy reading and navigation with a minimum of resizing, panning, and scrolling—across a wide range of devices (from desktop computer monitors to mobile phones)". The practical translation for a web developer is: 

    `we want complete control of the screen area and the interface we put on it has to have the right size for interaction and viewing.`

But mobile and desktop screens have different size and ratio and they have different types of input devices. Interfaces need to be customized to the target screen and input device.

## Render the interface
So far, what we know is that a desktop browser might communicate the width of the window and a mobile browser can communicate the width of the phone to the rendering engine (remember the rendering engine wanted a surface to paint on). Everything seems to be solved.

But wait a minute. When Steve Jobs revealed the first iPhone (Safari being one of the first mobile browsers claiming to render full html pages) he browsed the New York Times web page, which was rendered the same way as the desktop size page was. 
[Image:...screenshot]
The New York Times page did not have a style sheet targetting 320px (width of the first iPhone). It targetted 970px.

This introduces a limitation. Not only do we need full knowledge about the device and control over the interface. For internet to be useful on mobile devices, every existing web page needed to work properly on a small screen. If every web page that wanted to be properly rendered on a mobile phone was forced to reimplement the styling, no one would have done that.

By now, one should begin to realize that we need an abstraction from the phone's physical pixels. The logic saying that a web page should be rendered on the width of the browser needs some kind of modification. (Let's leave high resolution displays for later, but that comes into this equation as well)

## The key to responsive design: viewport

### The surprise
When the browser is about to layout a web page it uses the rendering engine, which is told what surface (with a desired size) to paint on. Here is a surprise: the iPhone from the prior section didn't tell the rendering engine "320px", it told the rendering engine "980px". Thus the rendering engine thought it worked with a 980px wide screen and therefore constructed an interface suitable for that.

Why would the mobile browser tell the rendering engine something else than its real size? Because we need to satisfy the prementioned limitation that every existing web page needs to render properly. The abstraction we need is the viewport. The viewport is what constrain the <html> element, the uppermost building block of a web page. The <htmL> element will have the same width as the viewport. On a desktop browser, the viewport is defined as the width of the browser. On mobile, it's something else (as you might have guessed, defaulting to 980px for the first iPhone).

You may notice that I omit 'height', that is because the height of a web page is most often variable and is not as important when dealing with responsive design as the width.

### The solution
As a developer wanting to gain control over the screen area, I want to either get or set the viewport size, then the interface can be customized to fit the screen. Luckily, we can set the viewport size. But since the viewport is not an HTML construct it can't be controlled with CSS (although working drafts are in progress [http://www.w3.org/TR/css-device-adapt/#the-viewport]). The current solution is to provide a meta tag in the <head> section of the web page.
    
    <meta name="viewport" content="width=device-width,initial-scale=1">

This viewport meta tag tells the rendering engine to paint on a canvas as wide as the pixels of the device and the 'initial-scale' makes sure that the resulting canvas fills the screen. Instead of device-width, you can input a value you like, telling the rendering engine to evaluate styling as if the screen really were your desired width, but it's most common to use the width of the current screen. (More options are described by Peter Paul Koch here [http://www.quirksmode.org/mobile/metaviewport/].)

Is this starting to get confusing or are you suspecting the benefits with this?

### The confusion
Let's give you another surprise! Starting with a question: Why didn't the iPhone 4 (640px wide) render the web interfaces half the size from before? If the rendering engine operated on the width of the screen, it should operate on 640px. The answer is: devicePixelRatio (accessible from the window object in browsers, go try your browser and see what it says...).

When the iPhone 4 got its higher resolution, it also got a devicePixelRatio of 2. The rendering engine were told: 640 / 2 = 320px. In fact, a device might have whatever devicePixelRatio it wants, even fractional numbers.

## Show me the code

We have discussed a lot of fuzzy things like viewports and devicePixelRatio. What do we need to put into our code to make our web pages behave responsively?

### Meta viewport
We need that meta viewport tag from earlier

    <meta name="viewport" content="width=device-width,initial-scale=1">

### CSS media queries
When the rendering engine knows how wide the browser is. Or as we leared, how wide the browser want the rendering engine to think it is (takning into account devicePixelRatio, default viewport width or the width specified in the meta tag). How do we target styling for specific widths?

We use media queries:

    @media (min-width: 480px) {
		.example {
			background-color: green;
		}
	}
	@media (min-width: 768px) {
		.example {
			background-color: red;
		}
	}

With this styling added, what do these statements affect?
- Device resolution
- 



## What did i avoid in this article?
- Browser differences