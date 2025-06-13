First challenge:  "Your challenge is to build out this QR code component and get it looking as close to the design as possible." [Link on Frontend Mentor](https://www.frontendmentor.io/challenges/qr-code-component-iux_sIO_H)

Second challenge: "Your challenge is to build out this blog preview card and get it looking as close to the design as possible." [Link to Frontend Mentor](https://www.frontendmentor.io/challenges/blog-preview-card-ckPaj01IcS)


# Reflection

After completing these challenges, take a few minutes to answer the following questions:

How did using Figma designs as references affect your coding process?
What challenges did you encounter when aligning your code with the design specifications?
How can the feedback and community resources on Frontend Mentor help you improve as a developer?

I often got element size, spacing, and configuration specifications direcly from Figma, rather than having to spend time tinkering and guessing.  I did style fonts according to the style guide though.

With the first challenge, I set text exactly as specified in the Figma style guide but didn't get the results I wanted.  I ended up tinkering with margins a bit to reproduce the sample.

With the second challenge, there were indicators the frame size was dynamic relative to viewport size.  This created some issues.  On 1440x990 resolution the frame is sized 384x522 with margin from frame to viewport edge 528x219y.  On 375x812 resolution the frame is sized 327x501, with margin from frame to viewport edge 24x 156y(top) 155y(bottom).  Ignoring the pixel difference in the last (it'll be rounded off without issue, I think), there is yet a problem.

The distance from frame to viewport on the x and y axes, expressed as a percentage of viewport width and height, is variable - which is not an issue of itself.  However, the frame itself also changes size on both x and y axes.

Interjecting, text sizes are also not fixed.  Even with variable frame size, text persistently had more or less the same relation expressed as a percentage of size compared to the overall frame.  Such precision looked odd to me; it turned out the Figma design sheet used predefined font size and formatting for desktop but not mobile.

So I considered using a lot of different things that weren't used in class, ranging from SASS to javascript to minmax.  But SASS and javascript were outside the scope of material presented, so using them would not be appropriate.

For mobile, (frame x/y) 327/501 = approx 0.653.  Desktop (frame x/y) 384/522 = approx 0.736.
For mobile, (frame to viewport x) 327/375 = approx 0.872  Desktop 384/1440 = approx 0.267.
For mobile, (frame to viewport y) 501/812 = approx 0.617.  Desktop 522/990 = 0.58.

There is no simple obvious relation, so I use algebra to figure out the value of a constant for calc to work on.  Frame width must be expressed as a multiple of viewport width plus a constant.  I'll make a lot of assumptions for expediency (viewport rectangular and longest on x-axis)

Desktop width:  1440 viewport, 384 frame, 1056 margin.  384 = (viewport width of 1440) * n + a.
Mobile width: 375 viewport, 327 frame, 48 margin.  327 = (viewport width of 375) * n + a.

a = 384 - 1440n = 327 - 375n.  57 = 1065n.  n = 57/1065 = approx 0.0534211268.  a = approx 307.0734774.
Checking:
Desktop width 1440, trying to compute for 384 frame.  0.0534211268 * 1440 + 307.0734774 = approx 383.999899992
Mobile width 375, trying to compute for 327 frame.  0.0534211268 * 375 + 307.0734774 = approx 327.10639995.

Desktop height:  990 viewport, 522 frame.
Mobile height: 812 viewport, 501 frame.

Community resources and feedback can always help improve a developer - not just that on Frontend Mentor.  Resources can save time looking things up and/or give insight into industry standards.  Feedback can give insight into things that a developer wouldn't think of on their own.