
# Visualizing Temperature Change of India (1901-2015)

### This work is inspired by blog post from dataquest.io (link: https://www.dataquest.io/blog/climate-temperature-spirals-python/)

### Data is collected from data.gov.in (link: https://data.gov.in/resources/monthly-seasonal-and-annual-mean-temperature-india-1901-2016)



```python
import json
import pandas as pd
import matplotlib.pyplot as plt
import numpy as np
```


```python
# Reading JSON File

with open('temp_india_1901_2015.json','r') as f:
    data = json.load(f)
```


```python
# checking field
for i in data['fields']:
    print (i)
```

    {'id': 'a', 'label': 'YEAR', 'type': 'string'}
    {'id': 'b', 'label': 'JAN', 'type': 'string'}
    {'id': 'c', 'label': 'FEB', 'type': 'string'}
    {'id': 'd', 'label': 'MAR', 'type': 'string'}
    {'id': 'e', 'label': 'APR', 'type': 'string'}
    {'id': 'f', 'label': 'MAY', 'type': 'string'}
    {'id': 'g', 'label': 'JUN', 'type': 'string'}
    {'id': 'h', 'label': 'JUL', 'type': 'string'}
    {'id': 'i', 'label': 'AUG', 'type': 'string'}
    {'id': 'j', 'label': 'SEP', 'type': 'string'}
    {'id': 'k', 'label': 'OCT', 'type': 'string'}
    {'id': 'l', 'label': 'NOV', 'type': 'string'}
    {'id': 'm', 'label': 'DEC', 'type': 'string'}
    {'id': 'n', 'label': 'ANNUAL', 'type': 'string'}
    {'id': 'o', 'label': 'JAN-FEB', 'type': 'string'}
    {'id': 'p', 'label': 'MAR-MAY', 'type': 'string'}
    {'id': 'q', 'label': 'JUN-SEP', 'type': 'string'}
    {'id': 'r', 'label': 'OCT-DEC', 'type': 'string'}



```python
# checking data
for i in data['data'][:5]:
    print (i)
```


```python
# converting data to usable form
data_dict = {}

for i in data['data']:
    # Adding Year to dict
    data_dict[i[0]] = {}
    # Adding 12 Month Data
    for j in range(12):
        data_dict[i[0]][j] = i[j+1]
```


```python
df = pd.DataFrame.from_dict(data_dict)
```


```python
df
```


```python
df['1901']
```


```python
fig = plt.figure(figsize=(10,10))
ax1 = plt.subplot(111, projection='polar')

# I am to lazy to change color scheme and  orginial color scheme used in blogpost is also nice. 
# SO, I used it without change. 
# setting up colors
fig.set_facecolor("#323331")
ax1.set_axis_bgcolor('#000100')

# removing labels
ax1.axes.get_yaxis().set_ticklabels([])
ax1.axes.get_xaxis().set_ticklabels([])


# giving title
ax1.set_title("Mean Temperature Change in India (1901-2015)", color='white', fontdict={'fontsize': 30})
ax1.text(0,0,"1901", color='white', size=30, ha='center')


# ploting yearly data one-by-one
for idx,i in enumerate(df.columns):
    r = list(df[i])
    ax1.grid(False)
    ax1.plot(theta, r, c=plt.cm.viridis(idx))
    theta = np.linspace(0,2*np.pi,12)
    ax1.plot(theta, r)

    full_circle_thetas = np.linspace(0, 2*np.pi, 1000)

# reducing down y limit
ax1.set_ylim(12.0, 35.0)
    
blue_line_one_radii = [15.0]*1000
red_line_one_radii = [20.0]*1000
red_line_two_radii = [25.0]*1000

# drawing temperature line
ax1.plot(full_circle_thetas, blue_line_one_radii, c='blue',linewidth=3)
ax1.plot(full_circle_thetas, orange_line_one_radii, c='yellow',linewidth=3)
ax1.plot(full_circle_thetas, red_line_one_radii, c='orange',linewidth=3)
ax1.plot(full_circle_thetas, red_line_two_radii, c = 'red' , linewidth=3)


ax1.text(np.pi/2, 15.0, "15.0 C", color="blue", ha='center', fontdict={'fontsize': 20})
ax1.text(np.pi/2, 20.0, "20.0 C", color="yellow", ha='center', fontdict={'fontsize': 20})
ax1.text(np.pi/2, 25.0, "25.0 C", color="orange", ha='center', fontdict={'fontsize': 20})
ax1.text(np.pi/2, 30.0, "30.0 C", color="red", ha='center', fontdict={'fontsize': 20})

ax1.plot()

```


```python

# this code is used to create gif
import sys
from matplotlib.animation import FuncAnimation

fig = plt.figure(figsize=(8,8))
ax1 = plt.subplot(111, projection='polar')

ax1.axes.get_yaxis().set_ticklabels([])
ax1.axes.get_xaxis().set_ticklabels([])

fig.set_facecolor("#323331")
ax1.set_axis_bgcolor('#000100')

ax1.set_title("Mean Temperature Change in India (1901-2015)", color='white', fontdict={'fontsize': 20})
ax1.text(0,0,"1901", color='white', size=30, ha='center')

ax1.set_ylim(12.0, 35.0)


blue_line_one_radii = [15.0]*1000
orange_line_one_radii = [20.0]*1000
red_line_one_radii = [25.0]*1000
red_line_two_radii = [30.0] * 1000


ax1.plot(full_circle_thetas, blue_line_one_radii, c='blue',linewidth=3)
ax1.plot(full_circle_thetas, orange_line_one_radii, c='yellow',linewidth=3)
ax1.plot(full_circle_thetas, red_line_one_radii, c='orange',linewidth=3)
ax1.plot(full_circle_thetas, red_line_two_radii, c = 'red' , linewidth=3)


ax1.text(np.pi/2, 15.0, "15.0 C", color="blue", ha='center', fontdict={'fontsize': 20})
ax1.text(np.pi/2, 20.0, "20.0 C", color="yellow", ha='center', fontdict={'fontsize': 20})
ax1.text(np.pi/2, 25.0, "25.0 C", color="orange", ha='center', fontdict={'fontsize': 20})
ax1.text(np.pi/2, 30.0, "30.0 C", color="red", ha='center', fontdict={'fontsize': 20})


year_text = ax1.text(0.5, 0.46,"", color="white", ha='center',  transform=ax1.transAxes,fontdict={'fontsize': 17})

def init():    
    year_text.set_text('')
    return year_text

def update(i):
    r = list(df[df.columns[i]])
    ax1.grid(False)
    theta = np.linspace(0,2*np.pi,12)
    print (df.columns[i])
    year_text.set_text(str(df.columns[i]))
    ax1.plot(theta, r)
    return year_text,

anim = FuncAnimation(fig, update, init_func=init,frames=len(df.columns), interval=150,blit=False)
# anim.save()
anim.save('India_Mean_Temp_change.gif', dpi=200, writer='imagemagick', savefig_kwargs={'facecolor': '#323331'})
```
