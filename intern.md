# Enhancing Outpost's Data Science Flow

### Proposed updates:

1. Upload Input Files -> Choose Input Files 

- Choose Files -> Upload Files & Choose from a recent run

  Hard to change the displayed text. 

  ```html
  <style>
      .button-container {
          display: flex;
          gap: 10px; /* Adjust the value as needed for the desired spacing */
      }
  </style>
  
  <div class="button-container">
      <button onclick="document.getElementById('files').click()">Upload Files</button>
      <input type="file" name="files" id="files" multiple accept=".csv" style="display:none">
      <button type="submit">Confirm</button>
  </div>
  ```

- Button: Upload -> Confirm

- Pricing (cannibalization) on / off 

- Add a description overwrite

2. Change of upload files in document

- Either upload or from a recent run or both

3. Add jerry to config file

​	`User View`

![image-20230614210000987](/Users/gaojinglun/Library/Application Support/typora-user-images/image-20230614210000987.png)

4. change selection radio button to download button in data files

- Case with a large number of files (user has to scoll down)
- One click instead of two

5. Take auto refresh and status change out

- Confusing
- No need

6. Examples from bootstrap on table

7. Underline the file that is being edited now

8. Shows on the left if the table has been changed

9. save changes for one / multi files

10. clear changes button

avoid user going to the next table without saving or clearing (plz save or clear before choosing another table)



- outpost/outpost/program.py
  - 98-102: indentation for all_required_files_uploaded
- upload from a recent run lose information above

​		pass as a dictionary to the url_for



- Give user choice of either uploading files or point to previous run of their own in the $\log$

Q1:

1. Where are the previous data? Cloud? 

​		On virtual machine. Data drive/outpost/output

​		Each run has a run_id: data drive/outpost/output/mike_2023_06_08_103025

​		./input saves all the input files

​		./input_QA saves the input errors

​		./inputs_effective we fill in some entries for them

​		./Output saves the output files

1. What does the previous runs look like, how many rows and cols? If like the screenshot, how to find the original codes?

​		Without the button.

​		Go to a new page when click on select run. Like the run log. 

​		Codes are provided

1. Can user select multiple runs?

​		Just one.

1. What does the UI look like? Right below the `Upload file`?

​		Below. From csv or from a recent run.

- Once correct files are uploaded (by either of the two choices),
  - add the ability for the user to select each table
  - Ability to filter table per any of the fields (as in Excel) and edit cells in that table
  - Save changes button after editing each table
  - Run button if edits are saved

Q2:

1. Double click?

​		Use the current implementaion

1. Display the whole or the first few rows?

​		Sequence: parameters->scope->reference calendar->promo group constraint->Calendar constraints->Budget

​		->synchronous

​		Top 4 always there

1. Should the data all in .csv format?

​		Yes.

1. How to make gray button until first edit occurs? Bootstrap?

​		Use bootstrap CSS.

1. The path for this page? /home/...

​		Extends layout





- View Results button appearing in the Log, only for the two popular modes (sim and opt):
  - Remove Download metrics button (on the View Results page) once the View Results paqe for manufacturer is released

Q3:

1. What are planning accounts? Why multiple?

​		Planning accounts are stores group.  

1. What are brands? Are there going to be many? Select multiple brands?

​		Brands are group of product. 

1. Path for this page? A hyperlink goes to view results?
2. Take closer look on reference metrics and other resultsS?

- View Results Page - The main tab is for manufacturer/CLIENT consisting of (in order):
  - The button for this page is active if reference_metrics exists and non empty
  - Page Title: CLIENT, super_category, mode (Simulate or Optimize)
  - Selections:
    - Scope selection
      - Select Planning Account (drop down) from reference_metrics
      - Display two columns: (brand, manufacturer) from reference_metrics for the selected planning_account
      - Select/unselect any brand or all brands (like in MS Excel or Bokeh multiselector)
    - Date selection: Full Year OR any consecutive quarter(s)
      - To be implemented as double slider to select start and end quarters (inclusive of the start and end quarters)
      - Bokeh Range Slider: https://docs.bokeh.org/en/latest/docs/user guide/inter action/widgets.htm|\#rangeslider start $=0$, end $=4$, value $=(0,4)$, step $=1$
  - Generate charts button to generate/update Charts 1, 2, and 3

Q4:

1. Super category in the title?
2. Where are brand and manufacturer?
3. What does this chart called?



Chart 2 & 3 - same size and same scale

Chart 1 lists all product groups within selected brands, sentence of modifications.

If another planning account is selected, all selections must reset

Open text box when click on chart 1

Information displayed on chart 1

- edp
- promo price or discount
- feature
- Display



- Chart 1 - Title: Calendar View

  - Each row is {brand, group_description} (labeled to the left)

  - Chart 1 uses annotated calendar generated by

    tpo_api/optimize/calendar_compare.py

  - Legend for: Same, New, Moved, Deeper, Shallower

  - Moved + Deeper would comprise 2 colors

  - Moved + Shallower would comprise 2 colors

  - Use scroll down window when needed

  - All events are the SAME (only gray boxes) IF one of the following:

    - ●  If mode = Simulate
    - ●  If mode = Optimize AND planning_account is infeasible

Q5:

1. If too many rows? Limit of 20 or 25 groups.
2. Assume full size of 52 weeks



Reference metrics:





TPO

price, volume, baseline (lift)

volume soukes/price drops

category (PL2)

Retailer hierarchy (channel)

trade spend = bill back * units sold

The impact of linear optimization on promotion planning **S: minimal gap** 

- No maximum gap
- Single item
- EDP stays the same
- Promotion offer does not change
- One week promotion

how they linearize the problem

Input:

- Parameters
- Scope
- Reference calendar

Pricing elasticity



- Sales unit: Units

- Sales dollar: Dollas
- Cogs (Cost of manufacture): COGS
- Wholesale price: WSP
- Baseline unit: Baseline
- discount: d
- lift coefficient: Lift
- Trade cost ratio: Ratio
- Every day price: EDP



- Shelf units (U) = Baseline( 1 + Lift(d) )

- `manufacturer_revenue` (MR) = U * WSP

- `retailer_revenue` (Shelf dollar): SD = U*EDP*(1-d)

- `lift dollar`: Lift units * promo price

- `baseline_dollars`: Baseline * promo price

- `total_cogs`: total costs

- `gisp_spend`: General in store promotion

- `edlp_spend`: Every day low price promotion

  trade_spend = gisp_spend + edlp_spend

- `trade spend` (TS) = Ratio * U * EDP * d

- `retailor_profit` (RP) = RR - U * WSP + trade_spend

- `manufature_profit` (MP) = U * WSP - trade_spend - total_cogs * U

- `buget_rate`: trade_spend / manufacturer_revenue

- `retailer_rebate`: profit by comparing to min retailer margin

`Trade rate` = trade_spend / manufacturer_revenue (typically between 10% - 20%)

`Manufaturer ROI` = (profit w/ trade - profit w/o trade) / TS = （manufature_profit - (baseline / (lift + baseline)(MR - total_cogs)) / TS = incremental_manufaturer_profit / gisp_cost

`manufature_margin`: MP / MR

`retailor_marigin`: RP / RR



Git

`Remove file from staging area`

```bash
git rm --cashed <file>
```

`Shows the commit logs`

```bash
git log --oneline
```

`Undoing Things`

- **git checkout**: A checkout is an operation that moves the `HEAD` ref pointer to a specified commit.
- **git revert**: A revert is an operation that takes a specified commit and creates a new commit which inverses the specified commit.
- **git reset**: A reset is an operation that takes a specified commit and resets the "three trees" to match the state of the repository at that specified commit.



azure

python3.7 run.py > output.txt

Remote connections

- ssh vm

- scp vm:path

- nohup python3.7 run.py > output.txt &

Tmux

Cat output.txt | grep Error

commit message: add/fix/solve this

:wq in vim

git merge --squash <message>

pandas cookbook

Front end

- HTML
- Boostrap
- CDN

Back end

- Flask
- Cloud service
- Web server



Outpost flow

Require users to upload input files

want: 

- select input files from a recent run 

- once the files are uploaded , we would like to give the ability to edit the input tables in a simple excel like fashion (save changes button)

Develop View Results Page

View results: select planning account, then brand, and time period. In calendar view.

Reference view:
Shelf sales to retrailer profit, trade spend...(end of the last bar is the start of the next bar)

optimized view

**always use one significant digit if number > 1M**







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

The [`figure()`](https://docs.bokeh.org/en/latest/docs/reference/plotting/figure.html#bokeh.plotting.figure) function accepts `x_axis_type` and `y_axis_type` as arguments. To specify a datetime axis, pass `"datetime"` for the value of either of these parameters.

```python
import pandas as pd

from bokeh.plotting import figure, show
from bokeh.sampledata.stocks import AAPL

df = pd.DataFrame(AAPL)
df['date'] = pd.to_datetime(df['date'])

# create a new plot with a datetime axis type
p = figure(width=800, height=250, x_axis_type="datetime")

p.line(df['date'], df['close'], color='navy', alpha=0.5)

show(p)
```

