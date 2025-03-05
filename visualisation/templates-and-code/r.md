---
icon: r-project
---

# R

GSS Theme

<details>

<summary>GSS Theme Code</summary>

{% code title="GSS Theme" overflow="wrap" fullWidth="true" %}
```r
# load packages
library(ggplot2)
library(tidyverse)
library(ggtext)
library(gghighlight)
library(sysfonts)
library(showtextdb)
library(showtext)
library(glue)
library(scales)
library(kableExtra)
library(patchwork)
library(forcats)
library(tidyverse)
library(ggbump)
library(reshape2)
library(plotly)


# load the font
font_add("century gothic bold", "Font/Century Gothic.ttf")

# make sure ggplot recognizes the font 
# and set the font to high-res
showtext_auto()
showtext::showtext_opts(dpi = 300)

# set default colour for plots with multiple categories
options(ggplot2.discrete.colour = c("#210D69", "#DB2E76", "#586889", "#227C42"))
options(ggplot2.discrete.fill   = c("#210D69", "#DB2E76", "#586889", "#227C42"))

# set default colour for plots with a single category
update_geom_defaults("bar",   list(fill = "#27A0CC"))
update_geom_defaults("col",   list(fill = "#27A0CC"))

# update the font to show in geom_text()
update_geom_defaults("text",   list(family = "century gothic bold", size = 4.5 ))
GSS_font <- "century gothic bold"
# create a GGS theme based on the theme_gray()
gssthemes<-function(){
  theme_gray() %+replace%
    theme(
      text=element_text(family = "century gothic bold",
                        colour="black",
                        size=10),
      plot.margin = margin(0.5,0.3, 0.3, 0.3, "cm"),
      # plot.title =element_textbox_simple(family="century gothic bold", size=16,
      #                                    lineheight=1,
      #                                    margin=margin(b=10)),
      # plot.title.position="plot",
      plot.caption=element_markdown(hjust=0, color="gray",
                                    lineheight=1.5,
                                    margin =margin(t=10)),
      plot.caption.position="plot",
      axis.title.y=element_text(color="black", angle=90, size = 10),
      axis.title.x=element_text(color="black",size = 10),
      axis.text.x=element_text(color="black", size = 10, vjust = 0, margin = margin(t = 5, r = 5, b = 0, l = 0, unit = "pt")),
      axis.text.y=element_text(color="black", size = 10, hjust = 1, margin = margin(t = 5, r = 5, b = 0, l = 0, unit = "pt")),
      legend.text=element_text(color="black",  size = 10),
      panel.grid.major.y=element_line(color="gray", size=0.25),
      panel.grid.major.x=element_blank(),
      panel.grid.minor=element_blank(),
      panel.background=element_rect(fill="white", color=NA),
      plot.background=element_rect(fill="white", color=NA),
      legend.background=element_rect(fill="white", color=NA),
     strip.background =element_rect(fill="white",  color=NA),
     legend.key = element_rect(fill = "white", color = NA), 
     strip.text = element_text( size = 20,  margin = margin(t = 5, r = 0, b = 10, l = 0, unit = "pt"))
    )
}

# this will make the labels of the bar chart a bit nicer, by ending above the highest data point
nicelimits <- function(x) {
  range(scales::extended_breaks(only.loose = TRUE)(x))
}


# Define color palette

statscolours_color_scheme  <- c("#382873", "#0168C8", "#00B050")

#locality
national_color <- "#27A0CC"
urban_color <- "#871A5B"
rural_color <- "#22D0B6"
urbanrural_color_scheme  <- c("#27A0CC", "#871A5B", "#22D0B6")

#Sex
male_color <- "#206095"
female_color <- "#F66068"
malefemale_color_scheme <- c("#27A0CC", "#206095", "#F66068")

#positive/negative
negative_color <- "#cc3333"
positive_color <- "#33cccc"

#pallete
neutral_color_scheme  <- c("#002060", "#0070C0", "#00B0F0", "#8EA9DB", "#9BC2E6", "#FFFFCC")
posneg_color_scheme  <- c("#38761D","#6AA84F","#93C47D","#F4CCCC","#E06666","#990000")
posneutralneg_color_scheme  <- c("#38761D","#6AA84F","#FFFFCC","#E06666","#990000")
population_color_scheme  <- c("#FFFFCC","#C7E9B4","#7FCDBB","#41B6C4","#2C7FB8")
incidence_color_scheme  <- c("#FECCCC","#FF9999","#FF6666","#FF3333","#CC0000","#990000")


#Economic sector colour
industry_color<-"#14607A"
agric_color<-"#07BB9E"
services_color<-"#F98B00"
economic_sectors<- c("#14607A","#07BB9E","#F98B00")

#food
food_colour<- "#3ECDB9"
nonfood_colour<-"#04BCFC"

# Region
Ahafo_color_code <- "#4B644B"
Ashanti_color_code <- "#7D96AF"
Bono_color_code <- "#EBA07E"
Bono_East_color_code <- "#09979B"
Central_color_code <- "#EA879C"
Eastern_color_code <- "#E5B9AD"
Greater_Accra_color_code <- "#CC9B7A"
Northern_color_code <- "#FDD835"
North_East_code <- "#0070C0"
Oti_color_code <- "#AE2B47"
Savannah_color_code <- "#F94240"
Upper_East_color_code <- "#903000"
Upper_West_color_code <- "#0F3B6C"
Volta_color_code <- "#59A77F"
Western_color_code <- "#FDAE6B"
Western_North_color_code <- "#B173A0"
```
{% endcode %}

</details>

***

***

## Bar charts

### One Colour Bar Chart

<details>

<summary><em><code>one colour bar chart code</code></em></summary>

{% code title="" overflow="wrap" fullWidth="true" %}
```r
# Pipe operator to pass data frame to the next line
n_chopbars_df %>% 
  # Create a ggplot object and set the aesthetic mappings
  ggplot(mapping = aes(x = region, y = number_of_chop_bars)) +
  # Add a column chart with bars of equal width
  geom_col(width = 0.8) +
  # Apply a custom theme to the plot
  gssthemes() +
# The expand argument controls whether the range of the y-axis is expanded to include a small margin around the data. The limits argument sets the upper and lower limits of the y-axis to nicelimits, which is a function that makes sure the limits are always above the largest data point. he breaks argument sets the tick marks on the y-axis to use extended_breaks from the scales package, which generates a sequence of evenly spaced values with loose spacing.
  scale_y_continuous( expand = c( 0, 1 ),
                      limits = nicelimits,
                                        labels = scales::comma,
                      breaks = scales::extended_breaks(only.loose = TRUE)) +
    # Set the scale for the x-axis with tick labels rotated by 90 degrees
  scale_x_discrete(guide = guide_axis(angle = 90)) +
     # Add axis labels to the plot
    labs(x = NULL,
             y = "number of chop bars")+
    # Set the coordinate system for the plot, allowing data points to be partially displayed outside of the plot area.
  coord_cartesian(clip = "off")
```
{% endcode %}



</details>

<div data-full-width="true"><figure><img src="../../.gitbook/assets/image (1).png" alt="" width="563"><figcaption></figcaption></figure></div>

<details>

<summary>One Color (Rotated) with labels</summary>



</details>

<details>

<summary>One Color (Rotated) with labels and ordered</summary>



</details>

***

## Line Charts

































