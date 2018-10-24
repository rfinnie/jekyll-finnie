---
date: 2018-10-24 15:10:29-07:00
excerpt: Wherein Ryan builds a printer test sheet, just like the hundreds of other
  printer test sheets out there.
excerpt_standalone: true
headline_image: /blog-media/2018/cmyk_line_test.png
layout: post
title: CMYK printer line test sheet
---
<img src="{{ site.url }}{{ site.baseurl }}/blog-media/2018/cmyk_line_test.png" alt="CMYK printer line test screenshot" class="img-responsive img-rounded img-md pull-right">
This week I bought a new color laser printer, a Brother HL-3170CDW.  It's replacing my old laser printer (Brother HL-2270DW) and inkjet (Canon MX922), the latter I got sick of being, well, an inkjet printer.

I use to have a weekly test print sent to the Canon, to prevent the nozzles from seizing and to notice when the ink cartridges were empty.  This was a cropped test print I found online, and mostly did the job.  But with the new printer, I decided to design my own test page.  It's not comprehensive; there are better tests out there for when you suspect something is wrong, for example it doesn't test gradients.  The one I designed is specifically for a weekly test, and will point out ink/toner problems while not using much ink/toner.

For each tested color (cyan, magenta, yellow, black, magenta/yellow, cyan/yellow and cyan/magenta), it shows a series of 15mm lines at 2.0, 1.0, 0.75, 0.5 and 0.25 points.  The lines are shown horizontal, vertical and at 45 degree angles.  Each tested color's total surface area is 285.84mm² (which would be about a 16.9mm square, so roughly equal to one of the colors' individual squares).  All ancillary text is grey to make the pure tested black stand out.

Download:

 * [CMYK printer line test sheet (US Letter size)](https://www.finnie.org/projects/printer/CMYK_Line_Test_US_Letter.pdf)
 * [CMYK printer line test sheet (A4 size)](https://www.finnie.org/projects/printer/CMYK_Line_Test_A4.pdf)
 * [Inkscape SVG source](https://www.finnie.org/projects/printer/CMYK_Line_Test.svg)

I have my Ubuntu laptop run the following every Sunday morning:
```
lpr -P brotherc /path/to/CMYK_Line_Test_US_Letter.pdf
```
(Replace "brotherc" with your printer identifier.)
