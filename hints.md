# Hints

Take a peek if you get stuck!

Helpful resources:

- cowplot docs: https://wilkelab.org/cowplot/index.html
- ggtext docs: http://wilkelab.org/ggtext/index.html


### Italics

<details>
  <summary>Functions</summary>

  - ggplot2
    - ggplot
    - geom_col
    - coord_flip
    - theme
  - ggtext
    - element_markdown
</details>

### Color axis text

<details>
  <summary>Functions</summary>
  
  - ggplot2
    - ggplot
    - geom_col
    - scale_fill_identity
    - coord_flip
  - ggtext
    - element_markdown
</details>

### Color & bold axis label

<details>
  <summary>Functions</summary>
  
  - ggplot2
    - ggplot
    - geom_line
    - ylab
    - theme_classic
    - theme
  - ggtext
    - element_markdown
</details>

### Assign colors manually


<details>
  <summary>Functions</summary>
  
  - RColorBrewer
    - brewer.pal
  - ggplot2
    - ggplot
    - geom_col
    - scale_fill_identity
    - ylab
    - theme_classic
</details>

### Image on a plot

<details>
  <summary>Functions</summary>

  - ggplot2
    - ggplot
    - geom_col
    - ylab
    - theme_classic
  - cowplot
    - ggdraw
    - draw_plot
    - draw_image
</details>

### Plot on an image

<details>
  <summary>Functions</summary>

  - magick
    - image_read
    - image_colorize
  - ggplot2
    - ggplot
    - geom_point
  - cowplot
    - theme_cowplot
    - ggdraw
    - draw_image
    - draw_plot
</details>

<details>
  <summary>Theme</summary>
  
  All cowplot themes have a transparent background, while ggplot2 themes do not. That's important for layering things behind plots. Order matters!
</details>

<details>
  <summary>The Truth</summary>
  
  It's awesome that this is possible, but I think this feature's main utility is to create examples of bad plots...
</details>

### Plots in a grid

<details>
  <summary>Functions</summary>
  
  - ggplot2
    - ggplot
    - geom_boxplot
    - scale_color_discrete
    - facet_wrap
    - ylim
    - labs
    - theme
    - element_blank
  - cowplot
    - theme_cowplot
    - plot_grid
</details>


### One title for two plots

<details>
  <summary>Functions</summary>
  
  - ggplot2
    - ggplot
    - geom_point
    - theme
    - element_blank
    - margin
  - cowplot
    - theme_half_open
    - background_grid
    - ggdraw
    - draw_label
    - plot_grid
</details>


### Inset plots

<details>
  <summary>Functions</summary>
  
  - ggplot2
    - ggplot
    - geom_point
    - geom_bar
    - labs
    - scale_y_continuous
  - cowplot
    - theme_minimal_grid
    - theme_minimal_hgrid
    - ggdraw
    - draw_plot
    - draw_plot_label
    
</details>

<details>
  <summary>Theme</summary>
  
  All cowplot themes have a transparent background, while ggplot2 themes do not. That's important for layering things behind plots. Order matters!
</details>

### Overlaying plots

<details>
  <summary>Functions</summary>
  
  - ggplot2
    - ggplot
    - geom_col
    - geom_point
    - scale_y_continuous
    - scale_x_discrete
    - theme
    - element_line
    - element_blank
    - element_text
  - cowplot
    - theme_minimal_hgrid
    - theme_half_open
    - align_plots
    - ggdraw
    - draw_plot
    
</details>

<details>
  <summary>Theme</summary>
  
  All cowplot themes have a transparent background, while ggplot2 themes do not. That's important for layering things behind plots. Order matters!
</details>

### Multi-panel figure

<details>
  <summary>Functions</summary>
  
  - ggplot2
    - ggplot
    - geom_point
    - geom_density
    - stat_smooth
    - scale_y_continuous
    - expand_scale
    - facet_grid
    - theme
  - cowplot
    - background_grid
    - theme_half_open
    - get_legend
    - align_plots
    - plot_grid
    
</details>
