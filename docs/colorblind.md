

# Choosing colorblind-friendly color scales

When coloring elements in your figures, you need keep in mind that a good proportion of your readers may have color-vision deficiency (i.e., are colorblind) and may not be able to distinguish the colors that look so clearly different to you. You could address this problem by preparing all figures in grayscale, but those figures would look drab and boring to everybody, even those with impaired color vision. Alternatively, you can (and should) employ redundant coding, discussed in more detail in Chapter \@ref(redundant-coding). However, independent of whether or not you use redundant coding, you should consider using a color scale whose colors are distinguishable even for people with color-vision deficiency. 

In choosing your color scale, remember that people with impaired color vision are not literally unable to see any colors. Instead, they will typically have difficulty to distinguish certain types of colors, for example red and green (red--green colorblindness) or blue and green (blue-yellow colorblindness). The key to making a colorblind-friendly color scale is to (i) choose colors with different levels of brightness and saturation, and (ii) choose color combinations that tend to look dissimilar even if color vision is partially impaired.

<div class="rmdtip">
<p>Unless you're an expert in color theory, go with an existing color scheme instead of designing your own.</p>
</div>


## Qualitative color scales

We frequently need to color discrete items that do not have an inherent order, such as different countries on a map or different manufacturers of a certain product. In those cases, we use qualitative color scales, which are color scales with a finite set of specific colors that are chosen to look as different from each other as possible.

My preferred color scale of this type, which I use extensively throughout this book, was developed specifically to work well for all the major types of color-vision deficiency [@Okabe-Ito-CUD]:
<img src="colorblind_files/figure-html/unnamed-chunk-3-1.png" width="816" style="display: block; margin: auto;" />
By providing eight different colors, this palette works for nearly any scenario with discrete colors. You should probably not color-code more than eight different items in a plot anyways.

A variant of the palette replaces black with gray, if you don't like to see completely black visual elements:
<img src="colorblind_files/figure-html/unnamed-chunk-4-1.png" width="816" style="display: block; margin: auto;" />

In these palettes, the alphanumeric codes represent the colors in RGB space, encoded as hexadecimals. In many plot libraries and image-manipulation programs, you can just enter these codes directly. If your software does not take hexadecimals directly, you can also use Table \@ref(tab:color-codes).


Table: (\#tab:color-codes) Colorblind-friendly color scale, developed by @Okabe-Ito-CUD. 

Name            Hex code     Hue     C, M, Y, K (%)   R, G, B (0-255)   R, G, B (%)
--------------  ------------ ------- ---------------- ----------------- ------------
         black       #000000       -  0, 0, 0, 100    0, 0, 0           0, 0, 0
          gray       #999999       -  0, 0, 0, 60     153, 153, 153     60, 60, 60
        orange       #E69F00     41°  0, 50, 100, 0   230, 159, 0       90, 60, 0
      sky blue       #56B4E9    202° 80, 0, 0, 0      86, 180, 233      35, 70, 90
  bluish green       #009E73    164° 97, 0, 75, 0     0, 158, 115       0, 60, 50
        yellow       #F0E442     56° 10, 5, 90, 0     240, 228, 66      95, 90, 25
          blue       #0072B2    202° 100, 50, 0, 0    0, 114, 178       0, 45, 70
     vermilion       #D55E00     27° 0, 80, 100, 0    213, 94, 0        80, 40, 0
reddish purple       #CC79A7    326° 10, 70, 0, 0     204, 121, 167     80, 60, 70
 
 
## Directional color scales

A second common use of color is to indicate intensity of some sort, for example temperature or speed. In those cases, we need a directional color scale that clearly indicates (i) which values are larger or smaller than which other ones, and (ii) how distant two specific values are from each other. The second point is particularly important, because it implies that the color scale needs to vary uniformly across its entire range. There cannot be parts of the scale where color changes more quickly than in other parts.

Designing such a color scale, in particular while respecting the additional requirement that they work for viewers with impaired color vision, can be surprisingly difficult. However, recent research into color scales has solved these issues and produced some very nice color scales [@Smith-vanderWalt-2015]. In brief, all these color scales satisfy the following conditions. First, brightness varies continuosly from dark to light, such that the color scale works even when reproduced in grayscale. Second, the major color axis changes from blue to yellow, because the alternative, red to green, would not work for the most common forms of color-vision deficiency, red-green blindness. Third, the blue colors appear on the dark side of the scale and the yellow colors on the light side, because gradients from dark yellow to bright blue look unnatural. With these constraints, there is only one major choice remaining, which is whether to transition from blue to yellow through green or through red. The following four color scales, which are now available for both python and R graphics and are becoming increasingly popular, meet all these requirements.

<img src="colorblind_files/figure-html/unnamed-chunk-5-1.png" width="816" style="display: block; margin: auto;" />

You can see examples using these scales throughout this book, for example in the chapter on redundant coding (Chapter \@ref(redundant-coding)).



