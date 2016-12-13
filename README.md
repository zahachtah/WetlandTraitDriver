# Wetland Trait Driver

This repository serves as a open source scientific platform on research linking environmental drivers and change to wetland processes and ecosystem services. A [live version](https://dl.dropboxusercontent.com/u/38371278/HelenFig/WetlandTraitDriver.html) of the figure!

# Data

The main data for the project is hosted on a public google sheet. While google sheets provides versioning, we still have one "frozen" version to see [here](https://docs.google.com/spreadsheets/d/1ck5ZCyX8FKwjJ36NOsbQFBuqsKweWUrhUh1sKBPoPqg/edit?usp=sharing).

The main working document is [here](https://docs.google.com/spreadsheets/d/1ck5ZCyX8FKwjJ36NOsbQFBuqsKweWUrhUh1sKBPoPqg/edit?usp=sharing). 

Working with the document needs some care due to the way the nodes and links interact:

    1) links in the links "sheet" can only occur between nodes registered on teh "nodes" sheet
    2) links cannot go "backwards" this will break the graph algorithm

Due to this we suggest to test major structural changes yourself according to the procedure below. Smaller edits are fine in the live document. Note however that the final figure is not automatically updated, but needs to be processed manually to produce an updated output.

# Processing of data to generate file for visualization

Once changes have been made in the google Data sheets, the file can be downloaded by exporting as excel file. Then do the following in python:

```python
import pandas as pd
import numpy.random as np
import sys
import json
import os
ExcelFile=r'PathToYourFile/Downloads/DriverTraitProcessData.xlsx'
links=pd.read_excel(ExcelFile,sheetname=1 )
nodes=pd.read_excel(ExcelFile,sheetname=0 )
linksjson=links.to_json(orient="records")
nodesjson=nodes.to_json(orient="records")
with open('PathToLiveFigureFolder/WetlandTraitDriver.json','w') as outfile:
    outfile.write('{"links":')
    outfile.write(dfLinks.to_json(orient="records"))
    outfile.write(',"nodes":')
    outfile.write(dfNodes.to_json(orient="records"))
    outfile.write('}')
```
    
# Look at the figure

make sure the path in the figure.html file points to the newly created json file:

```javascript
d3.json("PathToJsonFile/WetlandTraitDriver.json", function(error, graph) {
```
Due to the fact that most browsers like to load local files on a webpage you now need to either:

    1) open a terminal, cd to the folder and type ```python -m http.server 8000``` and then open localhost:8000 in a browser, or
    2) put the folder where you can access it via a http request
    
If everything works you should now be able to see the figure
