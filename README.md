
<!-- README.md is generated from README.Rmd. Please edit that file -->

``` r
library(cowplot)
library(glue)
library(ggtext)
library(gridtext)
library(here)
library(magick)
library(RColorBrewer)
library(tidyverse)
```

# plot-recreation

<!-- badges: start -->

<!-- badges: end -->

Re-creating plots to learn cool tricks and practice problem-solving.

## Exercises

View the plots below and try to write the code that creates them\! There
are more exercises here than you can probably get to in the time
allotted. Just pick ones to work on based on how interested you are in
learning how to create it. These are roughly arranged so that later
plots build on earlier plots (sometimes), but feel free to jump around.
At the end, we’ll each share one solution.

As much as this is an exercise in data viz with R, it’s also an exercise
in finding solutions in package documentation and general
problem-solving. Google is your friend\! If you get stuck, take a look
at the [hints](hints.md) or send Kelly a message in Slack.

### Formatting text

#### Italics

``` r
otu_data <- tribble(~otu, ~bact, ~value,
                       1, 'Staphylococceae', -0.5,
                       2, 'Moraxella', 0.5,
                       3, 'Streptococcus', 2,
                       4, 'Acinetobacter', 3.0
                    ) %>% mutate(name = glue("*{bact}* (OTU {otu})"))
otu_data %>% 
    ggplot(aes(name, value)) +
    geom_col() + 
    coord_flip() +
    theme(axis.text.y = element_markdown())
```

![](figures/otu_italics-1.png)<!-- -->

Source: [Claus Wilke’s talk at
rstudio::conf(2020)](https://twitter.com/ClausWilke/status/1222944728443809792)

#### Color axis text

``` r
# make some fake data
otu_data <- otu_data %>% 
    mutate(color = brewer.pal(4, "Dark2"),
           name = glue("<i style='color:{color}'>{bact}</i> (OTU {otu})"))
# plot it
otu_data %>% 
    ggplot(aes(name, value)) +
    geom_col() + 
    coord_flip() +
    theme(axis.text.y = element_markdown())
```

![](figures/otu_color-1.png)<!-- -->

Source: [Claus Wilke’s talk at
rstudio::conf(2020)](https://twitter.com/ClausWilke/status/1222944728443809792)

#### Color & bold axis label

``` r
# make some fake data
time_data <-tibble(time = seq(0,5,0.1),
               yes = sin(time * 0.8) + 0.8,
               no = sin(time * -0.5) + 1
                ) %>% pivot_longer(-time, names_to = "infected", values_to = "value")
# plot it
time_data %>% ggplot(aes(x = time, y = value, color = infected)) +
    geom_line() +
    ylab("<b style='color:#00BFC4'>Candidemia</b> vs. <b style='color:#F8766D'>no Candidemia</b> ITS1 diversity") +
    theme_classic() +
    theme(legend.position = "None",
          axis.title.y = element_markdown())
```

![](figures/candidemia_color-1.png)<!-- -->

Inspiration: fig. 2 from Jay’s recent JC paper [Zhai et al 2018 Nature
Med](https://www.nature.com/articles/s41591-019-0709-7/figures/2)

### Assign colors manually

``` r
otu_table <- tibble(
    sample = c("1", "2", "3", "1", "2", "3"),
    otu = c("A", "A", "A", "B", "B", "B"),
    abun = c(5, 15, 6, 15, 5, 2)
)
palette = RColorBrewer::brewer.pal(n = 4, name = "Paired")
colors = c(
  A = palette[[2]],
  B = palette[[3]]
)

# plot total counts (aboslute abundace)
otu_table %>%
    ggplot(aes(x = sample, y = abun, fill = otu)) +
    geom_col() +
    scale_fill_manual("otu", values=colors) +
    ylab("Total bacteria") +
    theme_classic()
```

![](figures/color_manual-1.png)<!-- -->

Adapted from:
<https://github.com/SchlossLab/compositional_data_analysis>

### Image on a plot

``` r
otu_table <- tibble(
    sample = c("1", "2", "3", "1", "2", "3"),
    otu = c("A", "A", "A", "B", "B", "B"),
    abun = c(5, 15, 6, 15, 5, 2)
)
# from https://github.com/mothur/logo
logo_file <- here("figures", "mothur_RGB.png")

# plot total counts (aboslute abundace)
plot_total <-  otu_table %>%
    ggplot(aes(x = sample, y = abun, fill = otu)) +
    geom_col() +
    ylab("Total bacteria") +
    theme_classic()


ggdraw() + 
  draw_plot(plot_total) +
  draw_image(logo_file, x = 1, y = 1, 
             hjust = 1.2, vjust = 1.2, 
             width = 0.3, height = 0.3)
```

![](figures/img_on_plot-1.png)<!-- -->

Adapted from:
<https://github.com/SchlossLab/compositional_data_analysis>

### Plot on an image

*With great power, comes great responsibility. Use at your own risk\!*

``` r
rides <- read_csv('https://raw.githubusercontent.com/kelly-sovacool/strava/master/data/processed/activities.csv') %>% 
    filter(distance_mi > 1, as.character(type) == 'Ride', year >= 2019) %>% 
    select(distance_mi, start_date)
```

    ## Parsed with column specification:
    ## cols(
    ##   .default = col_double(),
    ##   commute = col_logical(),
    ##   display_hide_heartrate_option = col_logical(),
    ##   external_id = col_character(),
    ##   flagged = col_logical(),
    ##   from_accepted_tag = col_logical(),
    ##   gear_id = col_character(),
    ##   has_heartrate = col_logical(),
    ##   has_kudoed = col_logical(),
    ##   heartrate_opt_out = col_logical(),
    ##   location_country = col_character(),
    ##   manual = col_logical(),
    ##   map.id = col_character(),
    ##   map.summary_polyline = col_character(),
    ##   name = col_character(),
    ##   private = col_logical(),
    ##   start_date = col_datetime(format = ""),
    ##   start_date_local = col_datetime(format = ""),
    ##   timezone = col_character(),
    ##   trainer = col_logical(),
    ##   type = col_character()
    ##   # ... with 3 more columns
    ## )

    ## See spec(...) for full column specifications.

``` r
# source https://www.pinclipart.com/picdir/big/196-1969309_free-bicycle-bike-cartoon-no-background-clipart.png
bike_img <- here('figures', 'bike.png') %>%
    image_read() %>% 
    image_colorize(70, "white")

# make the plot
rides_plot <- rides %>%
  ggplot(aes(start_date, distance_mi)) +
  geom_point(color='blue', alpha=0.8, size=2) +
    theme_cowplot()

ggdraw() + 
  draw_image(bike_img) + 
  draw_plot(rides_plot)
```

![](figures/plot_on_img-1.png)<!-- -->

Adapted from:
<https://wilkelab.org/cowplot/articles/drawing_with_on_plots.html#combining-plots-and-images>

### Plots in a grid

``` r
benchmarks_fit <-read_tsv('https://raw.githubusercontent.com/SchlossLab/OptiFitAnalysis/master/subworkflows/2_fit_reference_db/results/benchmarks.tsv?token=AEHR6TPPUNNM245DZ7TUP2C7ADK5E')
```

    ## Parsed with column specification:
    ## cols(
    ##   s = col_double(),
    ##   `h:m:s` = col_time(format = ""),
    ##   max_rss = col_double(),
    ##   max_vms = col_double(),
    ##   max_uss = col_double(),
    ##   max_pss = col_double(),
    ##   io_in = col_double(),
    ##   io_out = col_double(),
    ##   mean_load = col_double(),
    ##   dataset = col_character(),
    ##   ref = col_character(),
    ##   region = col_character(),
    ##   seed = col_double(),
    ##   method = col_character(),
    ##   printref = col_logical()
    ## )

``` r
benchmarks_clust <- read_tsv('https://raw.githubusercontent.com/SchlossLab/OptiFitAnalysis/master/subworkflows/1_prep_samples/results/benchmarks.tsv?token=AEHR6TNCIGRVN3B5GEVRVNC7ADLAS')
```

    ## Parsed with column specification:
    ## cols(
    ##   s = col_double(),
    ##   `h:m:s` = col_time(format = ""),
    ##   max_rss = col_double(),
    ##   max_vms = col_double(),
    ##   max_uss = col_double(),
    ##   max_pss = col_double(),
    ##   io_in = col_double(),
    ##   io_out = col_double(),
    ##   mean_load = col_double(),
    ##   dataset = col_character(),
    ##   ref = col_logical(),
    ##   region = col_logical(),
    ##   seed = col_double(),
    ##   method = col_character(),
    ##   printref = col_logical(),
    ##   iter = col_logical(),
    ##   numotus = col_logical()
    ## )

``` r
plot_box_time <- function(df) {
  df %>%
    group_by(dataset, method) %>%
    ggplot(aes(x = method, y = s, color = dataset)) +
    geom_boxplot() +
    scale_color_discrete() +
    facet_wrap("ref") +
    ylim(0,2100) +
    labs(y = 'seconds') +
    theme_cowplot() +
    theme(axis.title.x = element_blank())
}

time_plot_fit <- benchmarks_fit %>% 
  plot_box_time() +
  theme(axis.text.y = element_blank(),
        axis.title.y = element_blank())

time_plot_clust <- benchmarks_clust %>% 
  plot_box_time() +
  theme(legend.position = "None")

plot_grid(time_plot_clust, time_plot_fit,
          align = "h", rel_widths = c(1,3))
```

![](figures/plot_grid-1.png)<!-- -->

Source:
<https://github.com/SchlossLab/OptiFitAnalysis/tree/master/exploratory/2020#performance-as-measured-by-runtime>

### One title for two plots in a figure

``` r
mph_per_kph <- 0.621371
rides <- read_csv('https://raw.githubusercontent.com/kelly-sovacool/strava/master/data/processed/activities.csv') %>% 
    filter(distance_mi > 1, as.character(type) == 'Ride') %>% 
    mutate(average_speed_mph = average_speed * mph_per_kph) %>% 
    select(distance_mi, average_speed_mph, moving_time_hrs, start_date)
```

    ## Parsed with column specification:
    ## cols(
    ##   .default = col_double(),
    ##   commute = col_logical(),
    ##   display_hide_heartrate_option = col_logical(),
    ##   external_id = col_character(),
    ##   flagged = col_logical(),
    ##   from_accepted_tag = col_logical(),
    ##   gear_id = col_character(),
    ##   has_heartrate = col_logical(),
    ##   has_kudoed = col_logical(),
    ##   heartrate_opt_out = col_logical(),
    ##   location_country = col_character(),
    ##   manual = col_logical(),
    ##   map.id = col_character(),
    ##   map.summary_polyline = col_character(),
    ##   name = col_character(),
    ##   private = col_logical(),
    ##   start_date = col_datetime(format = ""),
    ##   start_date_local = col_datetime(format = ""),
    ##   timezone = col_character(),
    ##   trainer = col_logical(),
    ##   type = col_character()
    ##   # ... with 3 more columns
    ## )

    ## See spec(...) for full column specifications.

``` r
# make a plot grid consisting of two panels
p1 <- ggplot(rides, aes(x = average_speed_mph, y = distance_mi)) + 
  geom_point(colour = "blue") + 
  theme_half_open(12) + 
  background_grid(minor = 'none')

p2 <- ggplot(rides, aes(x = moving_time_hrs, y = distance_mi)) + 
  geom_point(colour = "green") + 
  theme_half_open(12) + 
  background_grid(minor = 'none') +
    theme(axis.title.y = element_blank())

plot_row <- plot_grid(p1, p2)

# now add the title
title <- ggdraw() + 
  draw_label(
    "Distance travelled with average speed and time spent moving",
    fontface = 'bold',
    x = 0,
    hjust = 0
  ) +
  theme(
    # add margin on the left of the drawing canvas,
    # so title is aligned with left edge of first plot
    plot.margin = margin(0, 0, 0, 7)
  )
plot_grid(
  title, plot_row,
  ncol = 1,
  # rel_heights values control vertical title margins
  rel_heights = c(0.1, 1)
)
```

![](figures/one_title_grid-1.png)<!-- -->

Adapted from:
<https://wilkelab.org/cowplot/articles/plot_grid.html#joint-plot-titles>

### Inset plots

``` r
rides <- read_csv('https://raw.githubusercontent.com/kelly-sovacool/strava/master/data/processed/activities.csv') %>% 
    filter(distance_mi > 1, as.character(type) == 'Ride') %>% 
    select(distance_mi, start_date, year)
```

    ## Parsed with column specification:
    ## cols(
    ##   .default = col_double(),
    ##   commute = col_logical(),
    ##   display_hide_heartrate_option = col_logical(),
    ##   external_id = col_character(),
    ##   flagged = col_logical(),
    ##   from_accepted_tag = col_logical(),
    ##   gear_id = col_character(),
    ##   has_heartrate = col_logical(),
    ##   has_kudoed = col_logical(),
    ##   heartrate_opt_out = col_logical(),
    ##   location_country = col_character(),
    ##   manual = col_logical(),
    ##   map.id = col_character(),
    ##   map.summary_polyline = col_character(),
    ##   name = col_character(),
    ##   private = col_logical(),
    ##   start_date = col_datetime(format = ""),
    ##   start_date_local = col_datetime(format = ""),
    ##   timezone = col_character(),
    ##   trainer = col_logical(),
    ##   type = col_character()
    ##   # ... with 3 more columns
    ## )

    ## See spec(...) for full column specifications.

``` r
plot_point <- rides %>% ggplot(aes(start_date, distance_mi)) +
  geom_point(color='red') +
  theme_minimal_grid(12)

inset <- rides %>%  
    group_by(year) %>% 
    summarize(sum_distance_mi=sum(distance_mi)) %>% 
    ggplot(aes(year, sum_distance_mi)) + 
  geom_bar(stat = "Identity", fill = "skyblue2", alpha = 0.7) + 
    labs(y='Total Distance (mi)') +
  scale_y_continuous(expand = expand_scale(mult = c(0, 0.05))) +
  theme_minimal_hgrid(11)
```

    ## Warning: `expand_scale()` is deprecated; use `expansion()` instead.

``` r
ggdraw(plot_point + theme_half_open(12)) +
  draw_plot(inset, x = 0.1, y = .45, width = .5, height = .5) +
  draw_plot_label(
    c("A", "B"),
    c(0, 0.1),
    c(1, 0.95),
    size = 12
  )
```

![](figures/inset-1.png)<!-- -->

Adapted from:
<https://wilkelab.org/cowplot/articles/drawing_with_on_plots.html#making-inset-plots>

### Overlaying plots

``` r
# make up some non-sensical data
alpha_data <- tibble(sample = as.factor(1:10),
                     otu_count = runif(10, min = 0, max = 100),
                     alpha_div = rnorm(10, mean = 2, sd = 0.5)
                     )

plot_col <- alpha_data %>%  
    ggplot(aes(sample, otu_count)) +
  geom_col(fill = "#6297E770") + 
  scale_y_continuous(
    #expand = expand_scale(mult = c(0, 0.05)),
    position = "right"
  ) +
    scale_x_discrete() +
  theme_minimal_hgrid(11, rel_small = 1) +
  theme(
    panel.grid.major = element_line(color = "#6297E770"),
    axis.line.x = element_blank(),
    axis.text.x = element_blank(),
    axis.title.x = element_blank(),
    axis.ticks = element_blank(),
    axis.ticks.length = grid::unit(0, "pt"),
    axis.text.y = element_text(color = "#6297E7"),
    axis.title.y = element_text(color = "#6297E7")
  )

plot_point <- alpha_data %>% ggplot(aes(sample, alpha_div)) + 
  geom_point(size = 3, color = "#D5442D") + 
  scale_y_continuous(limits = c(0, 4.5)) +
  theme_half_open(11, rel_small = 1) +
    scale_x_discrete() +
  theme(
    axis.ticks.y = element_line(color = "#BB2D05"),
    axis.text.y = element_text(color = "#BB2D05"),
    axis.title.y = element_text(color = "#BB2D05"),
    axis.line.y = element_line(color = "#BB2D05")
  )

aligned_plots <- align_plots(plot_col, plot_point, align="hv", axis="tblr")
overlaid_plots <- ggdraw(aligned_plots[[1]]) + draw_plot(aligned_plots[[2]])
overlaid_plots
```

![](figures/overlay-1.png)<!-- -->

Source: <https://wilkelab.org/cowplot/articles/aligning_plots.html>

### Multi-panel figure

``` r
# plot 1
p1 <- ggplot(iris, aes(Sepal.Length, Sepal.Width, color = Species)) + 
  geom_point() + 
  stat_smooth(method = "lm") +
  facet_grid(. ~ Species) +
  theme_half_open(12) +
  background_grid(major = 'y', minor = "none") + 
  panel_border() + 
  theme(legend.position = "none")

# plot 2
p2 <- ggplot(iris, aes(Sepal.Length, fill = Species)) +
  geom_density(alpha = .7) + 
  scale_y_continuous(expand = expand_scale(mult = c(0, 0.05))) +
  theme_half_open(12) +
  theme(legend.justification = "top")
```

    ## Warning: `expand_scale()` is deprecated; use `expansion()` instead.

``` r
p2a <- p2 + theme(legend.position = "none")

# plot 3
p3 <- ggplot(iris, aes(Sepal.Width, fill = Species)) +
  geom_density(alpha = .7) + 
  scale_y_continuous(expand = c(0, 0)) +
  theme_half_open(12) +
  theme(legend.position = "none")

# legend
legend <- get_legend(p2)

# align all plots vertically
plots <- align_plots(p1, p2a, p3, align = 'v', axis = 'l')
```

    ## `geom_smooth()` using formula 'y ~ x'

``` r
# put together the bottom row and then everything
bottom_row <- plot_grid(
  plots[[2]], plots[[3]], legend,
  labels = c("B", "C"),
  rel_widths = c(1, 1, .3),
  nrow = 1
)
plot_grid(plots[[1]], bottom_row, labels = c("A"), ncol = 1)
```

![](figures/multi-panel-1.png)<!-- -->

Source: <https://wilkelab.org/cowplot/articles/shared_legends.html>

## Solutions

A link to the source or inspiration is given under each code chunk. See
the [key](https://github.com/SchlossLab/plot-recreation/tree/key) branch
for the exact solutions.

Many of these are taken directly from another source, in some cases with
slight modifications. The original creators’ copyrights still apply; all
are OSI-approved licenses.
