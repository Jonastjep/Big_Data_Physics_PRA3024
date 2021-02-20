```python
# Makes the python notebook cells centered
from IPython.display import IFrame, HTML

CSS = """
.output {
    align-items: center;
}
"""
HTML('<style>{}</style>'.format(CSS))
```




<style>
.output {
    align-items: center;
}
</style>




```python

IFrame("GWDA_assignment-2.pdf", width=800, height=400)

```





<iframe
    width="800"
    height="400"
    src="GWDA_assignment-2.pdf"
    frameborder="0"
    allowfullscreen
></iframe>





```python
import plotly
import plotly.graph_objects as go
import numpy as np
import pandas
import matplotlib.pyplot as plt
import pylab

from pycbc.filter import highpass, resample_to_delta_t
from pycbc.catalog import Merger
from pycbc.frame import read_frame
```


```python
merger = Merger("GW170817")
strain, stilde = {}, {}

for detector in ['H1', 'L1']:
    ts = read_frame(f"{detector[0]}-{detector}_LOSC_CLN_4_V1-1187007040-2048.gwf",
                    f"{detector}:LOSC-STRAIN",
                    start_time=merger.time-224,
                    end_time=merger.time + 32,
                    check_integrity=False)
    strain[detector] = resample_to_delta_t(highpass(ts, 15.0), 1.0/2048)
    strain[detector] = strain[detector].crop(4,4)
    
    stilde[detector] = strain[detector].to_frequencyseries()
```


```python
fig1 = go.Figure()
fig1.add_trace(go.Scattergl(x=strain['H1'].sample_times,
                          y=strain['H1'],
                          mode='lines',
                          name="LIGO_Hanford"))
fig1.update_layout(xaxis_title='Time (s)',
                   yaxis_title='Amplitude')
fig1.write_html("LIGO_Hanford_raw.html")
```


```python
IFrame(src='./LIGO_Hanford_raw.html', width=800, height=500)
```





<iframe
    width="800"
    height="500"
    src="./LIGO_Hanford_raw.html"
    frameborder="0"
    allowfullscreen
></iframe>





```python

```
