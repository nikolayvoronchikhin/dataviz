

# Handling overlapping points {#overlapping-points}

When we want to visualize large or very large datasets, we often experience the challenge that simple *x*--*y* scatter plots do not work very well because many points lie on top of each other and partially or fully overlap. And similar problems can arise even in small datasets if data values were recorded with low precision or rounded, such that multiple observations have exactly the same numeric values. The technical term commonly used to describe this situation is "overplotting", i.e., plotting many points on top of each other. Here I describe several strategies you can pursue when encountering this challenge.


## Partial transparency and jittering

We first consider a scenario with only a moderate number of data points but with extensive rounding. Our dataset contains fuel economy during city driving and engine displacement for 234 popular car models released between 1999 and 2008 (Figure \@ref(fig:mpg-cty-displ-solid)). In this dataset, fuel economy is measured in miles per gallon (mpg) and is rounded to the nearest integer value. Engine displacement is measured in liters and is rounded to the nearest deciliter. Due to this rounding, many car models have exactly identical values. For example, there are 21 cars total with 2.0 liter engine displacement, and as a group they have only four different fuel economy values, 19, 20, 21, or 22 mpg. Therefore, in Figure \@ref(fig:mpg-cty-displ-solid) these 21 cars are represented by only four distinct points, so that 2.0 liter engines appear much less popular than they actually are.

(ref:mpg-cty-displ-solid) City fuel economy versus engine displacement, for popular cars released between 1999 and 2008. Each point represents one car. The point color encodes the drive train: front-wheel drive (FWD), rear-wheel drive (RWD), or four-wheel drive (4WD). The figure is labeled "bad" because many points are plotted on top of others and obscure them. 

<div class="figure" style="text-align: center">
<img src="overlapping_points_files/figure-html/mpg-cty-displ-solid-1.png" alt="(ref:mpg-cty-displ-solid)" width="577.5" />
<p class="caption">(\#fig:mpg-cty-displ-solid)(ref:mpg-cty-displ-solid)</p>
</div>

One way to ameliorate this problem is to use partial transparency. If we make individual points partially transparent, then overplotted points appear as darker points and thus the shade of the points reflects the density of points in that location of the graph (Figure \@ref(fig:mpg-cty-displ-transp)).

(ref:mpg-cty-displ-transp) City fuel economy versus engine displacement. Because points have been made partially transparent, points that lie on top of other points can now be identified by their darker shade. 

<div class="figure" style="text-align: center">
<img src="overlapping_points_files/figure-html/mpg-cty-displ-transp-1.png" alt="(ref:mpg-cty-displ-transp)" width="577.5" />
<p class="caption">(\#fig:mpg-cty-displ-transp)(ref:mpg-cty-displ-transp)</p>
</div>

However, making points partially transparent is not always sufficient to solve the issue of overplotting. For example, even though we can see in Figure \@ref(fig:mpg-cty-displ-transp) that some points have a darker shade than others, it is difficult to estimate how many points were plotted on top of each other in each location. In addition, while the differences in shading are clearly visible, they are not self-explanatory. A reader who sees this figure for the first time will likely wonder why some points are darker than others and will not realize that those points are in fact multiple points stacked on top of each other. A simple trick that helps in this situation is to apply a small amount of jitter to the points, i.e., to displace each point randomly by a small amount in either the *x* or the *y* direction or both. With jitter, it is immediately apparent that the darker areas arise from points that are plotted on top of each other (Figure \@ref(fig:mpg-cty-displ-jitter)).

(ref:mpg-cty-displ-jitter) City fuel economy versus engine displacement. By adding a small amount of jitter to each point, we can make the overplotted points more clearly visible without substantially distorting the message of the plot.

<div class="figure" style="text-align: center">
<img src="overlapping_points_files/figure-html/mpg-cty-displ-jitter-1.png" alt="(ref:mpg-cty-displ-jitter)" width="577.5" />
<p class="caption">(\#fig:mpg-cty-displ-jitter)(ref:mpg-cty-displ-jitter)</p>
</div>

One downside of jittering is that it does change the data and therefore has to be performed with care. If we jitter too much, we end up placing points in locations that are not representative of the underlying dataset. The result is a misleading visualization of the data. See Figure \@ref(fig:mpg-cty-displ-jitter-extreme) as an example.

(ref:mpg-cty-displ-jitter-extreme) City fuel economy versus engine displacement. By adding too much jitter to the points, we have created a visualization that does not accurately reflect the underlying dataset.

<div class="figure" style="text-align: center">
<img src="overlapping_points_files/figure-html/mpg-cty-displ-jitter-extreme-1.png" alt="(ref:mpg-cty-displ-jitter-extreme)" width="577.5" />
<p class="caption">(\#fig:mpg-cty-displ-jitter-extreme)(ref:mpg-cty-displ-jitter-extreme)</p>
</div>

## 2d histograms

When the number of individual points gets very large, partial transparency (with or without jittering) will not be sufficient to resolve the overplotting issue. What will typically happen is that areas with high point density will appear as uniform blobs of dark color while in areas with low point density the individual points are barely visible (Figure \@ref(fig:nycflights-points)). And changing the transparency level of individual points will either ameliorate one or the other of these problems while worsening the other; no transparency setting can address both at the same time. 

(ref:nycflights-points) Departure delay in minutes versus the flight departure time, for all flights departing Newark airport (EWR) in 2013. Each dot represents one departure.

<div class="figure" style="text-align: center">
<img src="overlapping_points_files/figure-html/nycflights-points-1.png" alt="(ref:nycflights-points)" width="630" />
<p class="caption">(\#fig:nycflights-points)(ref:nycflights-points)</p>
</div>

Figure \@ref(fig:nycflights-points) shows departure delays for over 100,000 individual flights, with each dot representing one flight departure. Even though we have made the individual dots fairly transparent, the majority of them just forms a black band between 0 and 300 minutes departure delay. This band obscures whether most flights depart approximately on time or with substantial delay (say 50 minutes or more). At the same time, the most delayed flights (with delays of 400 minutes or more) are barely visible due to the transparency of the dots.

In such cases, instead of plotting individual points, we can make a 2d histogram, where we subdivide the entire *x*--*y* plane into small rectangles, count how many observations fall into each rectangles, and then color the rectangles by that count. Figure \@ref(fig:nycflights-2d-bins) shows the result of this approach for the departure-delay data. This visualization clearly highlights several important features of the flight-departure data. First, the vast majority of departures during the day (6am to about 9pm) actually depart without delay or even early (negative delay). However, a modest number of departures has a substantial delay. Moreover, the later a plane departs in the day the more of a delay it can have. Importantly, the departure time is the actual time of departure, not the scheduled time of departure. So this figure does not necessarily tell us that planes scheduled to depart early never experience delay. What it does tell us, though, is that if a plane departs early it either has little delay or, in very rare cases, a delay of around 900 minutes.

(ref:nycflights-2d-bins) Departure delay in minutes versus the flight departure time. Each colored rectangle represents all flights departing at that time with that departure delay. Coloring represents the number of flights represented by that rectangle.

<div class="figure" style="text-align: center">
<img src="overlapping_points_files/figure-html/nycflights-2d-bins-1.png" alt="(ref:nycflights-2d-bins)" width="630" />
<p class="caption">(\#fig:nycflights-2d-bins)(ref:nycflights-2d-bins)</p>
</div>


As an alternative to binning the data into rectangle, we can also bin into hexagons. This approach, first proposed by 
@Carr-et-al-1987, has the advantage that the points in a hexagon are, on average, closer to the hexagon center than the points in an equal-area square are to the center of the square. Therefore, the colored hexagon represents the data slightly more accurately than the colored rectangle does. Figure \@ref(fig:nycflights-hex-bins) shows the flight departure data with hexagon binning rather than rectangular binning.

(ref:nycflights-hex-bins) Departure delay in minutes versus the flight departure time. Each colored hexagon represents all flights departing at that time with that departure delay. Coloring represents the number of flights represented by that hexagon.

<div class="figure" style="text-align: center">
<img src="overlapping_points_files/figure-html/nycflights-hex-bins-1.png" alt="(ref:nycflights-hex-bins)" width="630" />
<p class="caption">(\#fig:nycflights-hex-bins)(ref:nycflights-hex-bins)</p>
</div>


## Contour lines

Instead of binning data points into rectangles or hexagons, we can also estimate the point density across the plot area and indicate regions of different point densities with contour lines. This technique works well when the point density changes slowly across both the *x* and the *y* dimensions.

As an example for this approach, we consider the relationship between population number and area for counties in the Midwest. We have data for 1055 counties, and a scatter plot looks like a cloud of points (Figure \@ref(fig:midwest-scatter)). We can highlight the distribution of points more clearly by making them very small and partially transparent and ploting them on top of contour lines that delineate regions of comparable point density (Figure \@ref(fig:midwest-density-dots)). We can also plot just the contour lines, without the individual points (Figure \@ref(fig:midwest-density-smooth)). In this case, it can be helpful to add a trendline that shows the overall trend in the data. Here, there isn't much of a trend, and the shape of the trendline (approximately flat) reflects this lack of a trend.

(ref:midwest-scatter) Population versus area for counties in midwestern states. Data are taken from the 2010 US census and are shown for 1055 counties covering 12 states. Each dot represents one county. 

<div class="figure" style="text-align: center">
<img src="overlapping_points_files/figure-html/midwest-scatter-1.png" alt="(ref:midwest-scatter)" width="630" />
<p class="caption">(\#fig:midwest-scatter)(ref:midwest-scatter)</p>
</div>


(ref:midwest-density-dots) Population versus area for counties in midwestern states. Contour lines and shaded areas indicate the density of counties for that combination of population total and area. Individual counties are shown as light blue dots.

<div class="figure" style="text-align: center">
<img src="overlapping_points_files/figure-html/midwest-density-dots-1.png" alt="(ref:midwest-density-dots)" width="630" />
<p class="caption">(\#fig:midwest-density-dots)(ref:midwest-density-dots)</p>
</div>

(ref:midwest-density-smooth) Population versus area for counties in midwestern states. Contour lines and shaded areas indicate the density of counties for that combination of population total and area. Note that some counties lie outside the largest shaded area. The solid blue line highlights the mean relationship between population total and county area. It was obtained via least-square fitting of a general additive model with cubic spline base to the underlying data.

<div class="figure" style="text-align: center">
<img src="overlapping_points_files/figure-html/midwest-density-smooth-1.png" alt="(ref:midwest-density-smooth)" width="630" />
<p class="caption">(\#fig:midwest-density-smooth)(ref:midwest-density-smooth)</p>
</div>


