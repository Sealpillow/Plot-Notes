# Plot-Notes
## Plotly
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
