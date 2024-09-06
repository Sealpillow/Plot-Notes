# Plot-Notes
## Plotly


```
import plotly.graph_objects as go

# Sample data for experience levels and salaries
experience_levels = ['Entry Level', 'Mid Level', 'Senior Level', 'Executive-Level']
salaries = [7000, 9500, 12350, 14750]
percentage_increases = [36.39, 29.97, 19.31]  # Percentage increases between levels

# Create bar chart
fig = go.Figure()

# Add bars
fig.add_trace(go.Bar(
    x=experience_levels,
    y=salaries,
    text=['', f"+{percentage_increases[0]}%", f"+{percentage_increases[1]}%", f"+{percentage_increases[2]}%"],
    textposition='outside',
    marker_color='blue'
))

# Add annotations (arrows) and text above arrows
annotations = []
for i in range(len(salaries) - 1):
    # Start and end points for arrows
    x0 = experience_levels[i]
    x1 = experience_levels[i + 1]
    y0 = salaries[i]
    y1 = salaries[i + 1]

    # Add arrow annotation with adjusted length (shorter arrows by changing `ay`)
    annotations.append(dict(
        x=x1,
        y=y1 + 1000,  # End of the arrow at the height of the next bar
        ax=x0,
        ay=y0 + (y1 - y0) * 0.75,  # Make the arrow shorter by reducing the difference between y0 and y1
        xref='x',
        yref='y',
        axref='x',
        ayref='y',
        showarrow=True,
        arrowhead=3,
        arrowsize=1,
        arrowwidth=2,
        arrowcolor='green'
    ))

    # Add text annotation above the arrow
    annotations.append(dict(
        x=(i + 0.5),  # Position in the middle of the two bars
        y=(y0 + y1) / 2 + 500 + 1000,  # Slightly above the midpoint of the arrow
        text=f"+{percentage_increases[i]}%",  # Percentage text
        showarrow=False,  # Only text, no arrow
        font=dict(size=12, color="black"),
        align="center"
    ))

# Update layout to add annotations (arrows and text)
fig.update_layout(
    annotations=annotations,
    title='Average Salary (USD) Based on Experience Level',
    xaxis_title='Experience Level',
    yaxis_title='Average Salary (USD)',
    plot_bgcolor='rgba(245, 246, 249, 1)'
)

# Show figure
fig.show()

```
### Creating a Pivot Table and Plotting with Plotly
```
import pandas as pd
import plotly.express as px

# Example data
data = {
    'year': [2020, 2020, 2021, 2021, 2022, 2022, 2023, 2023],
    'course': ['Engineering', 'Business', 'Engineering', 'Business', 'Engineering', 'Business', 'Engineering', 'Business'],
    'enrolment': [300, 500, 320, 520, 310, 530, 330, 540]
}

df = pd.DataFrame(data)

# Create a pivot table with years as index and courses as columns
pivot = df.pivot_table(index='year', columns='course', values='enrolment', aggfunc='sum')

```
Plot Line Plot with pivot
```
import plotly.graph_objects as go

# Create a figure
fig = go.Figure()

# Add traces (lines) for each course
for course in pivot.columns:
    fig.add_trace(go.Scatter(x=pivot.index, y=pivot[course], mode='lines+markers', name=course))

# Customize the layout
fig.update_layout(title='Enrolments Over the Years', xaxis_title='Year', yaxis_title='Enrolment')

# Show the figure
fig.show()
```
Plot Bar Chart with pivot
```
# Reset index to make 'year' a column again for easier plotting
pivot_reset = pivot.reset_index()

# Melt the pivot table for easier plotting with Plotly Express
pivot_melted = pivot_reset.melt(id_vars='year', value_vars=pivot.columns, var_name='course', value_name='enrolment')

# Plot using Plotly Express
fig = px.bar(pivot_melted, x='year', y='enrolment', color='course', barmode='group',
             title='Enrolments by Year and Course')

# Show the figure
fig.show()
```

Line Plot with Values Displayed
```python
import plotly.graph_objects as go

# Example pivot table data
pivot = pd.DataFrame({
    'year': [2020, 2021, 2022, 2023],
    'Engineering': [300, 320, 310, 330],
    'Business': [500, 520, 530, 540]
})
pivot.set_index('year', inplace=True)

# Create a figure
fig = go.Figure()

# Add traces (lines) for each course with value labels
for course in pivot.columns:
    fig.add_trace(go.Scatter(
        x=pivot.index,
        y=pivot[course],
        mode='lines+markers+text',
        name=course,
        text=pivot[course],  # Display values
        textposition='top center'  # Position of the text
    ))

# Customize the layout
fig.update_layout(title='Enrolments Over the Years', xaxis_title='Year', yaxis_title='Enrolment')

# Show the figure
fig.show()

```

Bar Chart with Values Displayed
```python
import plotly.express as px

# Example pivot table data
pivot_reset = pd.DataFrame({
    'year': [2020, 2021, 2022, 2023],
    'Engineering': [300, 320, 310, 330],
    'Business': [500, 520, 530, 540]
})

# Melt the data for easy plotting
pivot_melted = pivot_reset.melt(id_vars='year', value_vars=pivot_reset.columns[1:], var_name='course', value_name='enrolment')

# Create a bar chart with value labels
fig = px.bar(pivot_melted, x='year', y='enrolment', color='course', barmode='group',
             text='enrolment', title='Enrolments by Year and Course')

# Customize the layout
fig.update_traces(textposition='outside')  # Position the text outside the bars

fig.show()
```

Pie Chart with Values Displayed
```python
import plotly.graph_objects as go

# Example data
labels = ['Engineering Sciences', 'Business & Administration', 'Humanities & Social Sciences']
sizes = [11669, 10098, 10154]

# Create a pie chart with value labels
fig = go.Figure(data=[go.Pie(labels=labels, values=sizes, textinfo='label+percent+value')])

fig.update_layout(title_text='Enrolment Distribution by Course')

fig.show()
```
### Adding text to histogram/Create histogram/ count plot:
Set text_auto parameter to True </br>
```
fig = px.histogram(pd.DataFrame({"A":[1,1,1,2,2,3,3,3,4,4,4,5]}),x="A", text_auto=True)
# to Edit axis title
fig.update_layout(
        xaxis_title="Town", yaxis_title="Count"
    )
fig.show()

```
![unnamed](https://github.com/Sealpillow/WebDevNotes/assets/51332449/fd5e4e7a-fcdd-4bb2-84a8-59a37ed9e4c1)

### To set angle of text
textangle - angle of the text value </br>
```
import plotly.graph_objs as go

# Sample data
x = [1, 2, 3, 4, 5]
y = [2, 4, 1, 3, 5]
text = ['Text 1', 'Text 2', 'Text 3', 'Text 4', 'Text 5']

# Create the scatter trace
fig = go.Figure()
fig.add_trace(go.Bar(x=x, y=y, name="bar", text=text, textangle=0))  
fig.update_layout(
    autosize=True,
    height=500,
    yaxis=dict(
        title_text="title",
    ),
    xaxis=dict(
        title_text="Town",
        tickangle=45  # to set text angle for x-axis
    )
)
# Show the plot
fig.show()
```
<img height="150" src="https://github.com/Sealpillow/WebDevNotes/assets/51332449/34c73290-640c-4583-b72d-fd965d73446c"/>

### Create Plotly Correlation Matrix/Heatmap
```
import plotly.express as px
import pandas as pd
import numpy as np

# Create a sample correlation matrix
corr_matrix = pd.DataFrame(np.random.rand(5, 5), columns=['A', 'B', 'C', 'D', 'E'])

# Create the correlation matrix plot
fig = px.imshow(corr_matrix, text_auto=True, height=500, aspect="auto")

# Update the figure layout
fig.update_layout(
    title="Correlation Matrix",
    xaxis_title="Features",
    yaxis_title="Features",
)
# Show the plot
fig.show()
```
![image](https://github.com/Sealpillow/WebDevNotes/assets/51332449/51015d4b-ea2b-4a5a-b300-487d42fc033d)

### Create line plot
Visualizing the relationship between two continuous variables. </br>
They show the trend or pattern in the data over a continuous range of values </br>
```
import plotly.graph_objects as go
import numpy as np

# Generate random data
x = np.linspace(0, 10, 100)
y1 = np.sin(x)
y2 = np.cos(x)
y3 = np.tan(x)

# Create a figure with multiple traces
fig = go.Figure()

# Add the first trace (sin(x)) with text annotations
fig.add_trace(go.Scatter(x=x, y=y1, mode='lines+text', name='sin(x)', text=np.round(y1, 2), textposition='top center'))

# Add the second trace (cos(x)) with text annotations
fig.add_trace(go.Scatter(x=x, y=y2, mode='lines+text', name='cos(x)', text=np.round(y2, 2), textposition='bottom center'))

# Add the third trace (tan(x)) with text annotations
fig.add_trace(go.Scatter(x=x, y=y3, mode='lines+text', name='tan(x)', text=np.round(y3, 2), textposition='top right'))

# Update the layout of the figure
fig.update_layout(
    title='Trigonometric Functions',
    xaxis_title='x',
    yaxis_title='y'
)

# Show the plot
fig.show()
```
![image](https://github.com/Sealpillow/WebDevNotes/assets/51332449/ab3441b8-1b60-4236-ba26-06dd32f1de87)

### Adjust size of figure
By setting the width and height </br>
```
import plotly.express as px

df = px.data.tips()
fig = px.scatter(df, x="total_bill", y="tip", facet_col="sex", width=1000, height=400)
fig.show()
```
Or u can update through fig update: </br.
```
fig.update_layout(height=500)
```
![image](https://github.com/Sealpillow/WebDevNotes/assets/51332449/46535713-f098-47da-9b5e-67831f4b3668)

### Convert Plotly Fig to html
The html_code variable will contain the HTML representation of the plot, which you can use as desired, such as embedding it in a webpage or saving it to an HTML file.
```
import plotly.graph_objects as go

# Create a Plotly figure
fig = go.Figure(data=go.Scatter(x=[1, 2, 3], y=[4, 5, 6]))

# Convert the figure to HTML code
html_code = fig.to_html()

# Print or use the HTML code as needed
print(html_code)
```

### Create CandleStick Chart
Source: https://plotly.com/python/candlestick-charts/ </br>
Code:
```
import yfinance as yf
import plotly.graph_objects as go

import pandas as pd
from datetime import datetime

data = yf.Ticker("MSFT").history(period="1mo")

dateData =  data.index
openData = list(data['Open'])
closeData = list(data['Close'])
highData = list(data['High'])
lowData = list(data['Low'])

fig = go.Figure(data=[go.Candlestick(x=dateData,
                open=openData,
                high=highData,
                low=lowData,
                close=closeData )])
fig.show()
```
Output: </br>
![image](https://github.com/Sealpillow/Plot-Notes/assets/51332449/16e16da9-9fb8-479d-b221-7be4bcbf6408)
