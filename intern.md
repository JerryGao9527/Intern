You will be working on improving and enhancing our Trade Promotions web application, mainly around key visualizations and data science process and how the data flows. Our main tools and stack consist of Python (pandas, numpy, etc.), visualizations (seaborn, bokeh), web app (flask, nginx), SQL, Linux, Asana. If you are not familiar with any of these tools, please familiarize yourself especially with seaborn, bokeh, flask and nginx.



## 1 Seaborn

```python
import seaborn as sns

# Apply the default theme
sns.set_theme()

fig, axs = plt.subplots(2, 2, figsize=(15, 15))
```

This uses the matplotlib rcParam system and will affect how all matplotlib plots look, even if you don’t make them with seaborn. Beyond the default theme, there are [several other options](https://seaborn.pydata.org/tutorial/aesthetics.html).

`seaborn.relplot`

Figure-level interface for drawing relational plots onto a FacetGrid.

This function provides access to several different axes-level functions that show the relationship between two variables with semantic mappings of subsets. The `kind` parameter selects the underlying axes-level function to use:

- [`scatterplot()`](https://seaborn.pydata.org/generated/seaborn.scatterplot.html#seaborn.scatterplot) (with `kind="scatter"`; the default)
- [`lineplot()`](https://seaborn.pydata.org/generated/seaborn.lineplot.html#seaborn.lineplot) (with `kind="line"`)

`seaborn.displot`

Figure-level interface for drawing distribution plots onto a FacetGrid.

This function provides access to several approaches for visualizing the univariate or bivariate distribution of data, including subsets of data defined by semantic mapping and faceting across multiple subplots. The `kind` parameter selects the approach to use:

- [`histplot()`](https://seaborn.pydata.org/generated/seaborn.histplot.html#seaborn.histplot) (with `kind="hist"`; the default)
- [`kdeplot()`](https://seaborn.pydata.org/generated/seaborn.kdeplot.html#seaborn.kdeplot) (with `kind="kde"`)
- [`ecdfplot()`](https://seaborn.pydata.org/generated/seaborn.ecdfplot.html#seaborn.ecdfplot) (with `kind="ecdf"`; univariate-only)

`seaborn.catplot`

Figure-level interface for drawing categorical plots onto a FacetGrid.

This function provides access to several axes-level functions that show the relationship between a numerical and one or more categorical variables using one of several visual representations. The `kind` parameter selects the underlying axes-level function to use:

Categorical scatterplots:

- [`stripplot()`](https://seaborn.pydata.org/generated/seaborn.stripplot.html#seaborn.stripplot) (with `kind="strip"`; the default)
- [`swarmplot()`](https://seaborn.pydata.org/generated/seaborn.swarmplot.html#seaborn.swarmplot) (with `kind="swarm"`)

Categorical distribution plots:

- [`boxplot()`](https://seaborn.pydata.org/generated/seaborn.boxplot.html#seaborn.boxplot) (with `kind="box"`)
- [`violinplot()`](https://seaborn.pydata.org/generated/seaborn.violinplot.html#seaborn.violinplot) (with `kind="violin"`)
- [`boxenplot()`](https://seaborn.pydata.org/generated/seaborn.boxenplot.html#seaborn.boxenplot) (with `kind="boxen"`)

Categorical estimate plots:

- [`pointplot()`](https://seaborn.pydata.org/generated/seaborn.pointplot.html#seaborn.pointplot) (with `kind="point"`)
- [`barplot()`](https://seaborn.pydata.org/generated/seaborn.barplot.html#seaborn.barplot) (with `kind="bar"`)
- [`countplot()`](https://seaborn.pydata.org/generated/seaborn.countplot.html#seaborn.countplot) (with `kind="count"`)

`seaborn.jointplot`

Draw a plot of two variables with bivariate and univariate graphs.



## 2 bokeh

Bokeh is a Python library for creating interactive visualizations for modern web browsers. It helps you build beautiful graphics, ranging from simple plots to complex dashboards with streaming datasets. With Bokeh, you can create JavaScript-powered visualizations without writing any JavaScript yourself.

`Interactive features`

- [![Icon representing the pan tool](https://docs.bokeh.org/en/latest/_images/Pan.png)](https://docs.bokeh.org/en/latest/_images/Pan.png) Use the **pan tool** to move the graph within your plot.
- [![Icon representing box zoom](https://docs.bokeh.org/en/latest/_images/BoxZoom.png)](https://docs.bokeh.org/en/latest/_images/BoxZoom.png) Use the **box zoom tool** to zoom into an area of your plot.
- [![Icon representing the wheel zoom](https://docs.bokeh.org/en/latest/_images/WheelZoom.png)](https://docs.bokeh.org/en/latest/_images/WheelZoom.png) Use the **wheel zoom tool** to zoom in and out with a mouse wheel.
- [![Icon representing the save tool](https://docs.bokeh.org/en/latest/_images/Save.png)](https://docs.bokeh.org/en/latest/_images/Save.png) Use the **save tool** to export the current view of your plot as a PNG file.
- [![Icon representing the reset tool](https://docs.bokeh.org/en/latest/_images/Reset.png)](https://docs.bokeh.org/en/latest/_images/Reset.png) Use the **reset tool** to return your view to the plot’s default settings.
- [![Help symbol](https://docs.bokeh.org/en/latest/_images/Help.png)](https://docs.bokeh.org/en/latest/_images/Help.png) Use the **help symbol** to learn more about the tools available in Bokeh.

```python
from bokeh.plotting import figure, show

# create a new plot with a title and axis labels
p = figure(title="Simple line example", x_axis_label='x', y_axis_label='y')
# add a line renderer with legend and line thickness to the plot
p.line(x, y, legend_label="Temp.", line_width=2)
# show the results
show(p)
```

Bokeh comes with five [built-in themes](https://docs.bokeh.org/en/latest/docs/reference/themes.html#bokeh-themes): `caliber`, `dark_minimal`, `light_minimal`, `night_sky`, and `contrast`. Additionally, you can define your own custom themes.

You can format the text that appears alongside your axes with Bokeh’s `TickFormatter` objects.

```python
from bokeh.models import NumeralTickFormatter

# create new plot
p = figure(
    title="Tick formatter example",
    sizing_mode="stretch_width",
    max_width=500,
    height=250,
)

# format axes ticks
p.yaxis[0].formatter = NumeralTickFormatter(format="$0.00")

# add renderers
p.circle(x, y, size=8)
p.line(x, y, color="navy", line_width=1)

# show the results
show(p)
```

To format the ticks of a `DatetimeAxis`, use the [`DatetimeTickFormatter`](https://docs.bokeh.org/en/latest/docs/reference/models/formatters.html#bokeh.models.DatetimeTickFormatter)

```python
p.xaxis[0].formatter = DatetimeTickFormatter(months="%b %Y")
```

Change what the horizontal and vertical lines of your grid look like by setting the various `grid_line` properties:

```python
# change things only on the x-grid
p.xgrid.grid_line_color = "red"

# change things only on the y-grid
p.ygrid.grid_line_alpha = 0.8
p.ygrid.grid_line_dash = [6, 4]
```

`Setting background colors`

```python
p.background_fill_color = (204, 255, 255)
p.border_fill_color = (102, 204, 255)
p.outline_line_color = (0, 0, 255)
```

The `gridplot` function builds a single toolbar for all the plots in the grid. `gridplot` is designed to layout a set of plots.

`Adding widgets`

Widgets are additional visual elements that you can include in your visualization. Use widgets to display additional information or to interactively control elements of your Bokeh document.

```python
from bokeh.layouts import layout
from bokeh.models import Div, RangeSlider, Spinner
from bokeh.plotting import figure, show

# prepare some data
x = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
y = [4, 5, 5, 7, 2, 6, 4, 9, 1, 3]

# create plot with circle glyphs
p = figure(x_range=(1, 9), width=500, height=250)
points = p.circle(x=x, y=y, size=30, fill_color="#21a7df")

# set up textarea (div)
div = Div(
    text="""
          <p>Select the circle's size using this control element:</p>
          """,
    width=200,
    height=30,
)

# set up spinner
spinner = Spinner(
    title="Circle size",
    low=0,
    high=60,
    step=5,
    value=points.glyph.size,
    width=200,
)
spinner.js_link("value", points.glyph, "size")

# set up RangeSlider
range_slider = RangeSlider(
    title="Adjust x-axis range",
    start=0,
    end=10,
    step=1,
    value=(p.x_range.start, p.x_range.end),
)
range_slider.js_link("value", p.x_range, "start", attr_selector=0)
range_slider.js_link("value", p.x_range, "end", attr_selector=1)

# create layout
layout = layout(
    [
        [div, spinner],
        [range_slider],
        [p],
    ]
)

# show result
show(layout)
```

`ColumnDataSource`

Creating your own [ColumnDataSource](https://docs.bokeh.org/en/latest/docs/user_guide/basic/data.html#ug-basic-data-cds) allows you to share data between multiple plots and widgets.

- Bokeh uses the dictionary’s keys as column names.
- The dictionary’s values are used as the data values for your ColumnDataSource.

To modify the data of an existing ColumnDataSource, update the `.data` property of your ColumnDataSource object:

```python
new_sequence = [8, 1, 4, 7, 3]
source.data["new_column"] = new_sequence
```

[ColumnDataSource](https://docs.bokeh.org/en/latest/docs/user_guide/basic/data.html#ug-basic-data-cds) streaming is an efficient way to append new data to a ColumnDataSource. When you use the `stream()` method, Bokeh only sends new data to the browser instead of sending the entire dataset.

`Filtering data`

Bokeh uses a concept called “view” to select subsets of data. Views are represented by Bokeh’s [`CDSView`](https://docs.bokeh.org/en/latest/docs/reference/models/sources.html#bokeh.models.CDSView) class. When you use a view, you can use one or more filters to select specific data points without changing the underlying data. You can also share those views between different plots.

To plot with a filtered subset of data, pass a [`CDSView`](https://docs.bokeh.org/en/latest/docs/reference/models/sources.html#bokeh.models.CDSView) to the `view` argument of any renderer method on a Bokeh plot.

```python
from bokeh.plotting import figure
from bokeh.models import ColumnDataSource, CDSView

filter1 = ... # IndexFilter(), BooleanFilter(), etc.
filter2 = ...

source = ColumnDataSource(some_data)
view = CDSView(filter=filter1 & filter2)

p = figure()
p.circle(x="x", y="y", source=source, view=view)
```

