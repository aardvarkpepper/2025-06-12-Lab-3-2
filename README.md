First challenge:  "Your challenge is to build out this QR code component and get it looking as close to the design as possible." [Link on Frontend Mentor](https://www.frontendmentor.io/challenges/qr-code-component-iux_sIO_H)

Second challenge: "Your challenge is to build out this blog preview card and get it looking as close to the design as possible." [Link to Frontend Mentor](https://www.frontendmentor.io/challenges/blog-preview-card-ckPaj01IcS)


# Reflection

After completing these challenges, take a few minutes to answer the following questions:

How did using Figma designs as references affect your coding process?
What challenges did you encounter when aligning your code with the design specifications?
How can the feedback and community resources on Frontend Mentor help you improve as a developer?

I often got element size, spacing, and configuration specifications direcly from Figma, rather than having to spend time tinkering and guessing.  I did style fonts according to the style guide though.

With the first challenge, I set text exactly as specified in the Figma style guide but didn't get the results I wanted.  I ended up tinkering with margins a bit to reproduce the sample.

With the second challenge, there were indicators the frame size was dynamic relative to viewport size.  This created some issues.  On 1440x990 resolution the frame is sized 384x522 with margin from frame to viewport edge 528x219y.  On 375x812 resolution the frame is sized 327x501, with margin from frame to viewport edge 24x 156y(top) 155y(bottom).  Ignoring the pixel difference in the last (it'll be rounded off without issue, I think), there were yet issues.  Details in below sections.

Community resources and feedback can always help improve a developer - not just that on Frontend Mentor.  Resources can save time looking things up and/or give insight into industry standards.  Feedback can give insight into things that a developer wouldn't think of on their own.

## Frame Size

The distance from frame to viewport on the x and y axes, expressed as a percentage of viewport width and height, is variable - which is not an issue of itself.  However, the frame itself also changes size on both x and y axes.

Interjecting, text sizes are also not fixed.  Even with variable frame size, text persistently had more or less the same relation expressed as a percentage of size compared to the overall frame.  Such precision looked odd to me; it turned out the Figma design sheet used predefined font size and formatting for desktop but not mobile.

So I considered using a lot of different things that weren't used in class, ranging from SASS to javascript to minmax.  But SASS and javascript were outside the scope of material presented, so using them would not be appropriate.

For mobile, (frame x/y) 327/501 = approx 0.653.  Desktop (frame x/y) 384/522 = approx 0.736.
For mobile, (frame to viewport x) 327/375 = approx 0.872  Desktop 384/1440 = approx 0.267.
For mobile, (frame to viewport y) 501/812 = approx 0.617.  Desktop 522/960 = 0.544.

There is no simple obvious relation, so I use algebra to figure out the value of a constant for calc to work on.  Frame width must be expressed as a multiple of viewport width plus a constant.  I'll make a lot of assumptions for expediency (viewport rectangular and longest on x-axis)

Desktop width:  1440 viewport, 384 frame, 1056 margin.  384 = (viewport width of 1440) * n + a.
Mobile width: 375 viewport, 327 frame, 48 margin.  327 = (viewport width of 375) * n + a.

a = 384 - 1440n = 327 - 375n.  57 = 1065n.  n = 57/1065 = approx 0.0535211268.  a = approx 306.9295774.
Checking:
Desktop width 1440, trying to compute for 384 frame.  1440 * 0.0535211268 + 306.9295774 = 383.99999999.
Mobile width 375, trying to compute for 327 frame.  375 * 0.0535211268 + 306.9295774 = 326.99999995.
Confirmed; frame width may be expressed as 0.0535211268 * viewport width + 306.9295774.

Desktop height:  960 viewport, 522 frame, 468 margin.  522 = (viewport height of 960) * n + a.
Mobile height: 812 viewport, 501 frame, 311 margin.  501 = (viewport height of 812) * n + a.

a = 522 - 960n = 501 - 812n.  21 = 148n.  n = 21/148 = approx 0.1418918919.  a = approx 385.78378378.
Checking:
Desktop height 960, trying to compute for 522 frame.  (960 * 0.1418918919) + 385.78378378 = approx 522.00000000
Mobile height 812, trying to compute for 501 frame.  (812 * 0.1418918919) + 385.78378378 = approx 501.00000000
Confirmed; frame height may be expressed ast 0.1418918919 * viewport height + 385.78378378.

## Image Container

The image container is 24px inside the frame.  The height is fixed at 200, but the width is variable, potentially much larger than the width of the provided image.  This was not an issue for the Figma wireframes shown, and barely detectable at my widest screen setting.  Much wider viewports could make the card look awkward, but repeating the image also looks odd and complicates the code.  I left it as is.

## Text size

Similar to the frame size issue, the desktop textbox is 336x194 for 1440x960, the mobile textbox 279x193, quite different ratios.  Taking some shortcuts to get a rougher approximation -

336 194 textbox desktop (1440 x 960)
learning
text preset 3 bold; figtree extra bold 14px, 150% line height, 0px letter spacing
published
text preset 3;figtree medium 14px, 150% line height, 0px letter spacing
14 / 960 = 0.01458333333
html&css
text preset 1; figtree extra bold 24px, 150% line height, 0px letter spacing
24 / 960 = 0.025
these
text preset 2; figtree medium 16px, 150% line height, 0px letter spacing
16 / 960 = 0.0166666667

279 193 textbox mobile (375 x 812)
learning
Figtree extra bold 12px line height 150% letter spacing 0%
published
figtree medium 12px line height 150% letter spacing 0%
12 / 812 = 0.0147783251
html&css
figtree extra bold 20px 150% line height letter spacing 0%
20 / 812 = 0.0246305419
these
figtree medium 14px 150% line height letter spacing 0%
14 / 812 = 0.0172413793

learning/published avg 0.0146808292
html&css avg 0.02481527095
these avg 0.01695402