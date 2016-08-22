

```python
import pandas as pd, numpy as np
```

# FCC Internet Speed Data

Internet providers report speeds to the FCC via form 477. 

* Data: https://www.fcc.gov/general/broadband-deployment-data-fcc-form-477
* Glossary: https://www.fcc.gov/general/explanation-broadband-deployment-data

# Part 0: Importing data


```python
tracts = pd.read_csv("data/tracts_to_towns.csv")
tracts.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>tract</th>
      <th>town</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>9001010101</td>
      <td>Greenwich</td>
    </tr>
    <tr>
      <th>1</th>
      <td>9001010102</td>
      <td>Greenwich</td>
    </tr>
    <tr>
      <th>2</th>
      <td>9001010201</td>
      <td>Greenwich</td>
    </tr>
    <tr>
      <th>3</th>
      <td>9001010202</td>
      <td>Greenwich</td>
    </tr>
    <tr>
      <th>4</th>
      <td>9001010300</td>
      <td>Greenwich</td>
    </tr>
  </tbody>
</table>
</div>




```python
providers = pd.read_excel("data/Provider_List_Jun2015_v3.xlsx")
providers.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>FRN</th>
      <th>ProviderName</th>
      <th>StateAbbr</th>
      <th>TechCode</th>
      <th>records</th>
      <th>blocks</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>10827</td>
      <td>Veracity Networks</td>
      <td>UT</td>
      <td>50</td>
      <td>4008</td>
      <td>4008</td>
    </tr>
    <tr>
      <th>1</th>
      <td>12542</td>
      <td>SERVICE ONE CABLE</td>
      <td>LA</td>
      <td>41</td>
      <td>726</td>
      <td>726</td>
    </tr>
    <tr>
      <th>2</th>
      <td>12781</td>
      <td>NEU VENTURES, INC.</td>
      <td>TX</td>
      <td>41</td>
      <td>921</td>
      <td>921</td>
    </tr>
    <tr>
      <th>3</th>
      <td>12781</td>
      <td>NEU VENTURES, INC.</td>
      <td>TX</td>
      <td>70</td>
      <td>1093</td>
      <td>1093</td>
    </tr>
    <tr>
      <th>4</th>
      <td>12849</td>
      <td>MUTUAL COMMUNICATIONS SERVICES, INC.</td>
      <td>IA</td>
      <td>41</td>
      <td>21</td>
      <td>21</td>
    </tr>
  </tbody>
</table>
</div>




```python
ct_speeds = pd.read_csv("data/CT-Fixed-Jun2015-v3.csv")
print len(ct_speeds["BlockCode"].unique())
ct_speeds.head()
```

    66103





<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>LogRecNo</th>
      <th>Provider_Id</th>
      <th>FRN</th>
      <th>ProviderName</th>
      <th>DBAName</th>
      <th>HoldingCompanyName</th>
      <th>HocoNum</th>
      <th>HocoFinal</th>
      <th>StateAbbr</th>
      <th>BlockCode</th>
      <th>TechCode</th>
      <th>Consumer</th>
      <th>MaxAdDown</th>
      <th>MaxAdUp</th>
      <th>Business</th>
      <th>MaxCIRDown</th>
      <th>MaxCIRUp</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>192364</td>
      <td>10990</td>
      <td>8797680</td>
      <td>Thames Valley Communications, Inc.</td>
      <td>Thames Valley Communications</td>
      <td>Thames Valley Communications, Inc.</td>
      <td>130576</td>
      <td>Thames Valley Communications, Inc.</td>
      <td>CT</td>
      <td>90117051013000</td>
      <td>40</td>
      <td>1</td>
      <td>110</td>
      <td>20</td>
      <td>1</td>
      <td>105</td>
      <td>20</td>
    </tr>
    <tr>
      <th>1</th>
      <td>192365</td>
      <td>10990</td>
      <td>8797680</td>
      <td>Thames Valley Communications, Inc.</td>
      <td>Thames Valley Communications</td>
      <td>Thames Valley Communications, Inc.</td>
      <td>130576</td>
      <td>Thames Valley Communications, Inc.</td>
      <td>CT</td>
      <td>90117051011006</td>
      <td>40</td>
      <td>1</td>
      <td>110</td>
      <td>20</td>
      <td>1</td>
      <td>105</td>
      <td>20</td>
    </tr>
    <tr>
      <th>2</th>
      <td>192366</td>
      <td>10990</td>
      <td>8797680</td>
      <td>Thames Valley Communications, Inc.</td>
      <td>Thames Valley Communications</td>
      <td>Thames Valley Communications, Inc.</td>
      <td>130576</td>
      <td>Thames Valley Communications, Inc.</td>
      <td>CT</td>
      <td>90117051011038</td>
      <td>40</td>
      <td>1</td>
      <td>110</td>
      <td>20</td>
      <td>1</td>
      <td>105</td>
      <td>20</td>
    </tr>
    <tr>
      <th>3</th>
      <td>192367</td>
      <td>10990</td>
      <td>8797680</td>
      <td>Thames Valley Communications, Inc.</td>
      <td>Thames Valley Communications</td>
      <td>Thames Valley Communications, Inc.</td>
      <td>130576</td>
      <td>Thames Valley Communications, Inc.</td>
      <td>CT</td>
      <td>90117051011020</td>
      <td>40</td>
      <td>1</td>
      <td>110</td>
      <td>20</td>
      <td>1</td>
      <td>105</td>
      <td>20</td>
    </tr>
    <tr>
      <th>4</th>
      <td>192368</td>
      <td>10990</td>
      <td>8797680</td>
      <td>Thames Valley Communications, Inc.</td>
      <td>Thames Valley Communications</td>
      <td>Thames Valley Communications, Inc.</td>
      <td>130576</td>
      <td>Thames Valley Communications, Inc.</td>
      <td>CT</td>
      <td>90117051011018</td>
      <td>40</td>
      <td>1</td>
      <td>110</td>
      <td>20</td>
      <td>1</td>
      <td>105</td>
      <td>20</td>
    </tr>
  </tbody>
</table>
</div>



# Part 0-1: Joining data

I'm going to add a town column to the ct_speeds dataframe, translating each tract to the town its a part of.


```python
def list_of(x, n):
    return [x] * n

# Example usage 
print list_of(False, 5) + (list_of(False, 5))
print list_of(False, 10)

```

    [False, False, False, False, False, False, False, False, False, False]
    [False, False, False, False, False, False, False, False, False, False]



```python
# Clean town names
tracts["town"] = tracts.apply(lambda x: x["town"].title().strip(), axis=1)
tracts.head()

```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>tract</th>
      <th>town</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>9001010101</td>
      <td>Greenwich</td>
    </tr>
    <tr>
      <th>1</th>
      <td>9001010102</td>
      <td>Greenwich</td>
    </tr>
    <tr>
      <th>2</th>
      <td>9001010201</td>
      <td>Greenwich</td>
    </tr>
    <tr>
      <th>3</th>
      <td>9001010202</td>
      <td>Greenwich</td>
    </tr>
    <tr>
      <th>4</th>
      <td>9001010300</td>
      <td>Greenwich</td>
    </tr>
  </tbody>
</table>
</div>




```python

# Add a column of census tracts, shorter than the block codes
def code_to_tract(code):
    sample_tract = "9001010101"
    tract_length = len(sample_tract)
    # return int(str(code)[:tract_length])
    return int('{0:d}'.format(int(code))[:tract_length])

code_to_tract(90117051013000)

ct_speeds["tract"] = ct_speeds.apply(lambda x: code_to_tract(x["BlockCode"]), axis=1)
ct_speeds.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>LogRecNo</th>
      <th>Provider_Id</th>
      <th>FRN</th>
      <th>ProviderName</th>
      <th>DBAName</th>
      <th>HoldingCompanyName</th>
      <th>HocoNum</th>
      <th>HocoFinal</th>
      <th>StateAbbr</th>
      <th>BlockCode</th>
      <th>TechCode</th>
      <th>Consumer</th>
      <th>MaxAdDown</th>
      <th>MaxAdUp</th>
      <th>Business</th>
      <th>MaxCIRDown</th>
      <th>MaxCIRUp</th>
      <th>tract</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>192364</td>
      <td>10990</td>
      <td>8797680</td>
      <td>Thames Valley Communications, Inc.</td>
      <td>Thames Valley Communications</td>
      <td>Thames Valley Communications, Inc.</td>
      <td>130576</td>
      <td>Thames Valley Communications, Inc.</td>
      <td>CT</td>
      <td>90117051013000</td>
      <td>40</td>
      <td>1</td>
      <td>110</td>
      <td>20</td>
      <td>1</td>
      <td>105</td>
      <td>20</td>
      <td>9011705101</td>
    </tr>
    <tr>
      <th>1</th>
      <td>192365</td>
      <td>10990</td>
      <td>8797680</td>
      <td>Thames Valley Communications, Inc.</td>
      <td>Thames Valley Communications</td>
      <td>Thames Valley Communications, Inc.</td>
      <td>130576</td>
      <td>Thames Valley Communications, Inc.</td>
      <td>CT</td>
      <td>90117051011006</td>
      <td>40</td>
      <td>1</td>
      <td>110</td>
      <td>20</td>
      <td>1</td>
      <td>105</td>
      <td>20</td>
      <td>9011705101</td>
    </tr>
    <tr>
      <th>2</th>
      <td>192366</td>
      <td>10990</td>
      <td>8797680</td>
      <td>Thames Valley Communications, Inc.</td>
      <td>Thames Valley Communications</td>
      <td>Thames Valley Communications, Inc.</td>
      <td>130576</td>
      <td>Thames Valley Communications, Inc.</td>
      <td>CT</td>
      <td>90117051011038</td>
      <td>40</td>
      <td>1</td>
      <td>110</td>
      <td>20</td>
      <td>1</td>
      <td>105</td>
      <td>20</td>
      <td>9011705101</td>
    </tr>
    <tr>
      <th>3</th>
      <td>192367</td>
      <td>10990</td>
      <td>8797680</td>
      <td>Thames Valley Communications, Inc.</td>
      <td>Thames Valley Communications</td>
      <td>Thames Valley Communications, Inc.</td>
      <td>130576</td>
      <td>Thames Valley Communications, Inc.</td>
      <td>CT</td>
      <td>90117051011020</td>
      <td>40</td>
      <td>1</td>
      <td>110</td>
      <td>20</td>
      <td>1</td>
      <td>105</td>
      <td>20</td>
      <td>9011705101</td>
    </tr>
    <tr>
      <th>4</th>
      <td>192368</td>
      <td>10990</td>
      <td>8797680</td>
      <td>Thames Valley Communications, Inc.</td>
      <td>Thames Valley Communications</td>
      <td>Thames Valley Communications, Inc.</td>
      <td>130576</td>
      <td>Thames Valley Communications, Inc.</td>
      <td>CT</td>
      <td>90117051011018</td>
      <td>40</td>
      <td>1</td>
      <td>110</td>
      <td>20</td>
      <td>1</td>
      <td>105</td>
      <td>20</td>
      <td>9011705101</td>
    </tr>
  </tbody>
</table>
</div>




```python
ct_speeds = ct_speeds.merge(tracts, left_on="tract", right_on="tract")
ct_speeds.head()

```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>LogRecNo</th>
      <th>Provider_Id</th>
      <th>FRN</th>
      <th>ProviderName</th>
      <th>DBAName</th>
      <th>HoldingCompanyName</th>
      <th>HocoNum</th>
      <th>HocoFinal</th>
      <th>StateAbbr</th>
      <th>BlockCode</th>
      <th>TechCode</th>
      <th>Consumer</th>
      <th>MaxAdDown</th>
      <th>MaxAdUp</th>
      <th>Business</th>
      <th>MaxCIRDown</th>
      <th>MaxCIRUp</th>
      <th>tract</th>
      <th>town</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>192364</td>
      <td>10990</td>
      <td>8797680</td>
      <td>Thames Valley Communications, Inc.</td>
      <td>Thames Valley Communications</td>
      <td>Thames Valley Communications, Inc.</td>
      <td>130576</td>
      <td>Thames Valley Communications, Inc.</td>
      <td>CT</td>
      <td>90117051013000</td>
      <td>40</td>
      <td>1</td>
      <td>110</td>
      <td>20</td>
      <td>1</td>
      <td>105</td>
      <td>20</td>
      <td>9011705101</td>
      <td>Stonington</td>
    </tr>
    <tr>
      <th>1</th>
      <td>192365</td>
      <td>10990</td>
      <td>8797680</td>
      <td>Thames Valley Communications, Inc.</td>
      <td>Thames Valley Communications</td>
      <td>Thames Valley Communications, Inc.</td>
      <td>130576</td>
      <td>Thames Valley Communications, Inc.</td>
      <td>CT</td>
      <td>90117051011006</td>
      <td>40</td>
      <td>1</td>
      <td>110</td>
      <td>20</td>
      <td>1</td>
      <td>105</td>
      <td>20</td>
      <td>9011705101</td>
      <td>Stonington</td>
    </tr>
    <tr>
      <th>2</th>
      <td>192366</td>
      <td>10990</td>
      <td>8797680</td>
      <td>Thames Valley Communications, Inc.</td>
      <td>Thames Valley Communications</td>
      <td>Thames Valley Communications, Inc.</td>
      <td>130576</td>
      <td>Thames Valley Communications, Inc.</td>
      <td>CT</td>
      <td>90117051011038</td>
      <td>40</td>
      <td>1</td>
      <td>110</td>
      <td>20</td>
      <td>1</td>
      <td>105</td>
      <td>20</td>
      <td>9011705101</td>
      <td>Stonington</td>
    </tr>
    <tr>
      <th>3</th>
      <td>192367</td>
      <td>10990</td>
      <td>8797680</td>
      <td>Thames Valley Communications, Inc.</td>
      <td>Thames Valley Communications</td>
      <td>Thames Valley Communications, Inc.</td>
      <td>130576</td>
      <td>Thames Valley Communications, Inc.</td>
      <td>CT</td>
      <td>90117051011020</td>
      <td>40</td>
      <td>1</td>
      <td>110</td>
      <td>20</td>
      <td>1</td>
      <td>105</td>
      <td>20</td>
      <td>9011705101</td>
      <td>Stonington</td>
    </tr>
    <tr>
      <th>4</th>
      <td>192368</td>
      <td>10990</td>
      <td>8797680</td>
      <td>Thames Valley Communications, Inc.</td>
      <td>Thames Valley Communications</td>
      <td>Thames Valley Communications, Inc.</td>
      <td>130576</td>
      <td>Thames Valley Communications, Inc.</td>
      <td>CT</td>
      <td>90117051011018</td>
      <td>40</td>
      <td>1</td>
      <td>110</td>
      <td>20</td>
      <td>1</td>
      <td>105</td>
      <td>20</td>
      <td>9011705101</td>
      <td>Stonington</td>
    </tr>
  </tbody>
</table>
</div>




```python
# checksum
print ct_speeds["town"].value_counts().sum()
print len(ct_speeds.index)
```

    427788
    427788


# Part 3: Analysis


```python
def gen_report(town_df):
    print "There are " + str(len(town_df["BlockCode"].unique())) + " census blocks and "\
    + str(len(town_df["tract"].unique())) + " tracts in __."
    print str(len(town_df["DBAName"].unique())) + " companies provide service."
    
    print "Advertised max speeds range from "\
    + str(town_df["MaxAdUp"].min()) + "/" + str(town_df["MaxAdDown"].min())\
    + " to "\
    + str(town_df["MaxAdUp"].max()) + "/" + str(town_df["MaxAdDown"].max())
    
    return town_df

def town_report(town_name):
    print "Generating report for " + town_name
    return gen_report(ct_speeds[ct_speeds["town"] == town_name.title()])

town_report("Greenwich").head()
```

    Generating report for Greenwich
    There are 1246 census blocks and 15 tracts in __.
    23 companies provide service.
    Advertised max speeds range from 0.0/0.0 to 100.0/101.0





<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>LogRecNo</th>
      <th>Provider_Id</th>
      <th>FRN</th>
      <th>ProviderName</th>
      <th>DBAName</th>
      <th>HoldingCompanyName</th>
      <th>HocoNum</th>
      <th>HocoFinal</th>
      <th>StateAbbr</th>
      <th>BlockCode</th>
      <th>TechCode</th>
      <th>Consumer</th>
      <th>MaxAdDown</th>
      <th>MaxAdUp</th>
      <th>Business</th>
      <th>MaxCIRDown</th>
      <th>MaxCIRUp</th>
      <th>tract</th>
      <th>town</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>10181</th>
      <td>199770</td>
      <td>11012</td>
      <td>10296853</td>
      <td>Broadview Networks Holdings, Inc.</td>
      <td>Broadview Networks, Inc.</td>
      <td>Broadview Networks Holdings, Inc.</td>
      <td>130166</td>
      <td>Broadview Networks Holdings, Inc.</td>
      <td>CT</td>
      <td>90010102013008</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>3</td>
      <td>0.786</td>
      <td>9001010201</td>
      <td>Greenwich</td>
    </tr>
    <tr>
      <th>10182</th>
      <td>1562991</td>
      <td>11593</td>
      <td>18589226</td>
      <td>Lightower Fiber Networks I, LLC (formerly know...</td>
      <td>Lightower</td>
      <td>LTS Group Holdings LLC</td>
      <td>131095</td>
      <td>LTS Group Holdings LLC</td>
      <td>CT</td>
      <td>90010102011000</td>
      <td>50</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1000</td>
      <td>1000.000</td>
      <td>9001010201</td>
      <td>Greenwich</td>
    </tr>
    <tr>
      <th>10183</th>
      <td>1562992</td>
      <td>11593</td>
      <td>18589226</td>
      <td>Lightower Fiber Networks I, LLC (formerly know...</td>
      <td>Lightower</td>
      <td>LTS Group Holdings LLC</td>
      <td>131095</td>
      <td>LTS Group Holdings LLC</td>
      <td>CT</td>
      <td>90010102011011</td>
      <td>50</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1000</td>
      <td>1000.000</td>
      <td>9001010201</td>
      <td>Greenwich</td>
    </tr>
    <tr>
      <th>10184</th>
      <td>1562993</td>
      <td>11593</td>
      <td>18589226</td>
      <td>Lightower Fiber Networks I, LLC (formerly know...</td>
      <td>Lightower</td>
      <td>LTS Group Holdings LLC</td>
      <td>131095</td>
      <td>LTS Group Holdings LLC</td>
      <td>CT</td>
      <td>90010102011012</td>
      <td>50</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1000</td>
      <td>1000.000</td>
      <td>9001010201</td>
      <td>Greenwich</td>
    </tr>
    <tr>
      <th>10185</th>
      <td>1562994</td>
      <td>11593</td>
      <td>18589226</td>
      <td>Lightower Fiber Networks I, LLC (formerly know...</td>
      <td>Lightower</td>
      <td>LTS Group Holdings LLC</td>
      <td>131095</td>
      <td>LTS Group Holdings LLC</td>
      <td>CT</td>
      <td>90010102011018</td>
      <td>50</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1000</td>
      <td>1000.000</td>
      <td>9001010201</td>
      <td>Greenwich</td>
    </tr>
  </tbody>
</table>
</div>




```python
def summary_table(df, col):
    return ct_speeds.groupby(col)\
    .agg({
            "MaxAdDown":max,
            "MaxAdUp":max,
            "MaxCIRDown":max,
            "MaxCIRUp":max,
            "DBAName":lambda x: x.nunique(),
#             "BlockCode":lambda x: x.nunique(),   
         })\
    .reset_index()

town_speeds = summary_table(ct_speeds, "town")

town_speeds.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>town</th>
      <th>DBAName</th>
      <th>MaxAdDown</th>
      <th>MaxAdUp</th>
      <th>MaxCIRDown</th>
      <th>MaxCIRUp</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Ansonia</td>
      <td>12</td>
      <td>150</td>
      <td>35</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Ashford</td>
      <td>7</td>
      <td>100</td>
      <td>5</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Avon</td>
      <td>14</td>
      <td>150</td>
      <td>20</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Barkhamsted</td>
      <td>7</td>
      <td>100</td>
      <td>5</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Beacon Falls</td>
      <td>9</td>
      <td>150</td>
      <td>20</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
  </tbody>
</table>
</div>



#### Top business speeds: you can get 1 Gb everywhere

Every town offers 1000 Mbps up/down for businesses.

MaxCIRDown/Up - Maximum contractual downstream[/upstream] bandwidth offered by the provider in the block for Business service (filer directed to report 0 if the contracted service is sold on a "best efforts" basis without a guaranteed data-throughput rate)




```python
town_speeds["MaxCIRUp"].value_counts()
```




    1000    167
    Name: MaxCIRUp, dtype: int64




```python
town_speeds["MaxCIRDown"].value_counts()
```




    1000    167
    Name: MaxCIRDown, dtype: int64



#### Max advertised speed per town


```python
town_speeds[["town","MaxAdUp","MaxAdDown"]].sort_values(by="MaxAdDown")
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>town</th>
      <th>MaxAdUp</th>
      <th>MaxAdDown</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>83</th>
      <td>Montville</td>
      <td>6</td>
      <td>75</td>
    </tr>
    <tr>
      <th>149</th>
      <td>Waterford</td>
      <td>6</td>
      <td>75</td>
    </tr>
    <tr>
      <th>135</th>
      <td>Sterling</td>
      <td>6</td>
      <td>75</td>
    </tr>
    <tr>
      <th>66</th>
      <td>Killingly</td>
      <td>6</td>
      <td>75</td>
    </tr>
    <tr>
      <th>91</th>
      <td>New London</td>
      <td>6</td>
      <td>75</td>
    </tr>
    <tr>
      <th>43</th>
      <td>Eastford</td>
      <td>5</td>
      <td>100</td>
    </tr>
    <tr>
      <th>60</th>
      <td>Hampton</td>
      <td>5</td>
      <td>100</td>
    </tr>
    <tr>
      <th>75</th>
      <td>Mansfield</td>
      <td>6</td>
      <td>100</td>
    </tr>
    <tr>
      <th>92</th>
      <td>New Milford</td>
      <td>6</td>
      <td>100</td>
    </tr>
    <tr>
      <th>94</th>
      <td>Newmilford</td>
      <td>6</td>
      <td>100</td>
    </tr>
    <tr>
      <th>111</th>
      <td>Pomfret</td>
      <td>5</td>
      <td>100</td>
    </tr>
    <tr>
      <th>26</th>
      <td>Colebrook</td>
      <td>5</td>
      <td>100</td>
    </tr>
    <tr>
      <th>115</th>
      <td>Putnam</td>
      <td>6</td>
      <td>100</td>
    </tr>
    <tr>
      <th>119</th>
      <td>Roxbury</td>
      <td>6</td>
      <td>100</td>
    </tr>
    <tr>
      <th>122</th>
      <td>Scotland</td>
      <td>5</td>
      <td>100</td>
    </tr>
    <tr>
      <th>126</th>
      <td>Sherman</td>
      <td>6</td>
      <td>100</td>
    </tr>
    <tr>
      <th>140</th>
      <td>Thompson</td>
      <td>6</td>
      <td>100</td>
    </tr>
    <tr>
      <th>160</th>
      <td>Windham</td>
      <td>6</td>
      <td>100</td>
    </tr>
    <tr>
      <th>108</th>
      <td>Plainfield</td>
      <td>6</td>
      <td>100</td>
    </tr>
    <tr>
      <th>21</th>
      <td>Chaplin</td>
      <td>5</td>
      <td>100</td>
    </tr>
    <tr>
      <th>166</th>
      <td>Woodstock</td>
      <td>6</td>
      <td>100</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Canterbury</td>
      <td>6</td>
      <td>100</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Barkhamsted</td>
      <td>5</td>
      <td>100</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Bridgewater</td>
      <td>6</td>
      <td>100</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Ashford</td>
      <td>5</td>
      <td>100</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Brooklyn</td>
      <td>6</td>
      <td>100</td>
    </tr>
    <tr>
      <th>84</th>
      <td>Morris</td>
      <td>35</td>
      <td>101</td>
    </tr>
    <tr>
      <th>81</th>
      <td>Milford</td>
      <td>100</td>
      <td>101</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Bethlehem</td>
      <td>35</td>
      <td>101</td>
    </tr>
    <tr>
      <th>87</th>
      <td>New Canaan</td>
      <td>35</td>
      <td>101</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>96</th>
      <td>Norfolk</td>
      <td>35</td>
      <td>150</td>
    </tr>
    <tr>
      <th>95</th>
      <td>Newtown</td>
      <td>35</td>
      <td>150</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Bozrah</td>
      <td>20</td>
      <td>150</td>
    </tr>
    <tr>
      <th>20</th>
      <td>Canton</td>
      <td>20</td>
      <td>150</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Branford</td>
      <td>20</td>
      <td>150</td>
    </tr>
    <tr>
      <th>90</th>
      <td>New Haven</td>
      <td>35</td>
      <td>150</td>
    </tr>
    <tr>
      <th>89</th>
      <td>New Hartford</td>
      <td>35</td>
      <td>150</td>
    </tr>
    <tr>
      <th>88</th>
      <td>New Fairfield</td>
      <td>20</td>
      <td>150</td>
    </tr>
    <tr>
      <th>86</th>
      <td>New Britain</td>
      <td>20</td>
      <td>150</td>
    </tr>
    <tr>
      <th>85</th>
      <td>Naugatuck</td>
      <td>20</td>
      <td>150</td>
    </tr>
    <tr>
      <th>82</th>
      <td>Monroe</td>
      <td>35</td>
      <td>150</td>
    </tr>
    <tr>
      <th>80</th>
      <td>Middletown</td>
      <td>20</td>
      <td>150</td>
    </tr>
    <tr>
      <th>100</th>
      <td>North Stonington</td>
      <td>20</td>
      <td>150</td>
    </tr>
    <tr>
      <th>79</th>
      <td>Middlefield</td>
      <td>20</td>
      <td>150</td>
    </tr>
    <tr>
      <th>77</th>
      <td>Meriden</td>
      <td>20</td>
      <td>150</td>
    </tr>
    <tr>
      <th>76</th>
      <td>Marlborough</td>
      <td>20</td>
      <td>150</td>
    </tr>
    <tr>
      <th>74</th>
      <td>Manchester</td>
      <td>20</td>
      <td>150</td>
    </tr>
    <tr>
      <th>73</th>
      <td>Madison</td>
      <td>20</td>
      <td>150</td>
    </tr>
    <tr>
      <th>72</th>
      <td>Lyme</td>
      <td>20</td>
      <td>150</td>
    </tr>
    <tr>
      <th>70</th>
      <td>Lisbon</td>
      <td>20</td>
      <td>150</td>
    </tr>
    <tr>
      <th>69</th>
      <td>Ledyard</td>
      <td>20</td>
      <td>150</td>
    </tr>
    <tr>
      <th>68</th>
      <td>Lebanon</td>
      <td>20</td>
      <td>150</td>
    </tr>
    <tr>
      <th>67</th>
      <td>Killingworth</td>
      <td>20</td>
      <td>150</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Bristol</td>
      <td>20</td>
      <td>150</td>
    </tr>
    <tr>
      <th>64</th>
      <td>Hebron</td>
      <td>20</td>
      <td>150</td>
    </tr>
    <tr>
      <th>63</th>
      <td>Harwinton</td>
      <td>35</td>
      <td>150</td>
    </tr>
    <tr>
      <th>62</th>
      <td>Hartland</td>
      <td>20</td>
      <td>150</td>
    </tr>
    <tr>
      <th>61</th>
      <td>Hartford</td>
      <td>20</td>
      <td>150</td>
    </tr>
    <tr>
      <th>78</th>
      <td>Middlebury</td>
      <td>35</td>
      <td>150</td>
    </tr>
    <tr>
      <th>0</th>
      <td>Ansonia</td>
      <td>35</td>
      <td>150</td>
    </tr>
  </tbody>
</table>
<p>167 rows × 3 columns</p>
</div>




```python
summary_table(ct_speeds[(ct_speeds["Consumer"] == 1)\
                       &(ct_speeds["MaxAdDown"] > 0)],
              "BlockCode")["MaxAdDown"].value_counts()
```




    150    28114
    101    14407
    15     10486
    100     8593
    75      2549
    0        543
    110      528
    18       465
    55       414
    Name: MaxAdDown, dtype: int64




```python
town_speeds.sort_values(by="MaxAdDown")
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>town</th>
      <th>DBAName</th>
      <th>MaxAdDown</th>
      <th>MaxAdUp</th>
      <th>MaxCIRDown</th>
      <th>MaxCIRUp</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>83</th>
      <td>Montville</td>
      <td>9</td>
      <td>75</td>
      <td>6</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>149</th>
      <td>Waterford</td>
      <td>16</td>
      <td>75</td>
      <td>6</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>135</th>
      <td>Sterling</td>
      <td>8</td>
      <td>75</td>
      <td>6</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>66</th>
      <td>Killingly</td>
      <td>11</td>
      <td>75</td>
      <td>6</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>91</th>
      <td>New London</td>
      <td>15</td>
      <td>75</td>
      <td>6</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>43</th>
      <td>Eastford</td>
      <td>8</td>
      <td>100</td>
      <td>5</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>60</th>
      <td>Hampton</td>
      <td>8</td>
      <td>100</td>
      <td>5</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>75</th>
      <td>Mansfield</td>
      <td>9</td>
      <td>100</td>
      <td>6</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>92</th>
      <td>New Milford</td>
      <td>12</td>
      <td>100</td>
      <td>6</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>94</th>
      <td>Newmilford</td>
      <td>9</td>
      <td>100</td>
      <td>6</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>111</th>
      <td>Pomfret</td>
      <td>12</td>
      <td>100</td>
      <td>5</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>26</th>
      <td>Colebrook</td>
      <td>8</td>
      <td>100</td>
      <td>5</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>115</th>
      <td>Putnam</td>
      <td>13</td>
      <td>100</td>
      <td>6</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>119</th>
      <td>Roxbury</td>
      <td>8</td>
      <td>100</td>
      <td>6</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>122</th>
      <td>Scotland</td>
      <td>7</td>
      <td>100</td>
      <td>5</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>126</th>
      <td>Sherman</td>
      <td>8</td>
      <td>100</td>
      <td>6</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>140</th>
      <td>Thompson</td>
      <td>10</td>
      <td>100</td>
      <td>6</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>160</th>
      <td>Windham</td>
      <td>12</td>
      <td>100</td>
      <td>6</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>108</th>
      <td>Plainfield</td>
      <td>10</td>
      <td>100</td>
      <td>6</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>21</th>
      <td>Chaplin</td>
      <td>8</td>
      <td>100</td>
      <td>5</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>166</th>
      <td>Woodstock</td>
      <td>12</td>
      <td>100</td>
      <td>6</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Canterbury</td>
      <td>9</td>
      <td>100</td>
      <td>6</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Barkhamsted</td>
      <td>7</td>
      <td>100</td>
      <td>5</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Bridgewater</td>
      <td>8</td>
      <td>100</td>
      <td>6</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Ashford</td>
      <td>7</td>
      <td>100</td>
      <td>5</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Brooklyn</td>
      <td>10</td>
      <td>100</td>
      <td>6</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>84</th>
      <td>Morris</td>
      <td>8</td>
      <td>101</td>
      <td>35</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>81</th>
      <td>Milford</td>
      <td>19</td>
      <td>101</td>
      <td>100</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Bethlehem</td>
      <td>9</td>
      <td>101</td>
      <td>35</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>87</th>
      <td>New Canaan</td>
      <td>16</td>
      <td>101</td>
      <td>35</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>96</th>
      <td>Norfolk</td>
      <td>9</td>
      <td>150</td>
      <td>35</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>95</th>
      <td>Newtown</td>
      <td>15</td>
      <td>150</td>
      <td>35</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Bozrah</td>
      <td>10</td>
      <td>150</td>
      <td>20</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>20</th>
      <td>Canton</td>
      <td>11</td>
      <td>150</td>
      <td>20</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Branford</td>
      <td>15</td>
      <td>150</td>
      <td>20</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>90</th>
      <td>New Haven</td>
      <td>21</td>
      <td>150</td>
      <td>35</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>89</th>
      <td>New Hartford</td>
      <td>10</td>
      <td>150</td>
      <td>35</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>88</th>
      <td>New Fairfield</td>
      <td>8</td>
      <td>150</td>
      <td>20</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>86</th>
      <td>New Britain</td>
      <td>18</td>
      <td>150</td>
      <td>20</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>85</th>
      <td>Naugatuck</td>
      <td>14</td>
      <td>150</td>
      <td>20</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>82</th>
      <td>Monroe</td>
      <td>14</td>
      <td>150</td>
      <td>35</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>80</th>
      <td>Middletown</td>
      <td>20</td>
      <td>150</td>
      <td>20</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>100</th>
      <td>North Stonington</td>
      <td>9</td>
      <td>150</td>
      <td>20</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>79</th>
      <td>Middlefield</td>
      <td>14</td>
      <td>150</td>
      <td>20</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>77</th>
      <td>Meriden</td>
      <td>19</td>
      <td>150</td>
      <td>20</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>76</th>
      <td>Marlborough</td>
      <td>9</td>
      <td>150</td>
      <td>20</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>74</th>
      <td>Manchester</td>
      <td>19</td>
      <td>150</td>
      <td>20</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>73</th>
      <td>Madison</td>
      <td>10</td>
      <td>150</td>
      <td>20</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>72</th>
      <td>Lyme</td>
      <td>8</td>
      <td>150</td>
      <td>20</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>70</th>
      <td>Lisbon</td>
      <td>12</td>
      <td>150</td>
      <td>20</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>69</th>
      <td>Ledyard</td>
      <td>15</td>
      <td>150</td>
      <td>20</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>68</th>
      <td>Lebanon</td>
      <td>9</td>
      <td>150</td>
      <td>20</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>67</th>
      <td>Killingworth</td>
      <td>8</td>
      <td>150</td>
      <td>20</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Bristol</td>
      <td>18</td>
      <td>150</td>
      <td>20</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>64</th>
      <td>Hebron</td>
      <td>7</td>
      <td>150</td>
      <td>20</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>63</th>
      <td>Harwinton</td>
      <td>10</td>
      <td>150</td>
      <td>35</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>62</th>
      <td>Hartland</td>
      <td>9</td>
      <td>150</td>
      <td>20</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>61</th>
      <td>Hartford</td>
      <td>24</td>
      <td>150</td>
      <td>20</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>78</th>
      <td>Middlebury</td>
      <td>12</td>
      <td>150</td>
      <td>35</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>0</th>
      <td>Ansonia</td>
      <td>12</td>
      <td>150</td>
      <td>35</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
  </tbody>
</table>
<p>167 rows × 6 columns</p>
</div>



#### How much "competition?"

There were an average of 12 providers, with as many as 24 and as few as 7 providers in each CT town.


```python
town_speeds["DBAName"].describe()
```




    count    167.000000
    mean      12.616766
    std        4.235161
    min        7.000000
    25%        9.000000
    50%       12.000000
    75%       15.500000
    max       24.000000
    Name: DBAName, dtype: float64




```python
summary_table(ct_speeds, "tract")["DBAName"].describe()
```




    count    829.000000
    mean      10.382388
    std        2.622778
    min        6.000000
    25%        8.000000
    50%       10.000000
    75%       12.000000
    max       20.000000
    Name: DBAName, dtype: float64



#### Businesses have a lot of choices
That's a surprisingly high number of providers, when I know that in practice it's nearly impossible as a consumer to get more than one company to provide internet access to an address, let alone 12. What if we exclude business providers?


```python
summary_table(ct_speeds[ct_speeds["Consumer"] == 1], "BlockCode")["DBAName"].describe()
```




    count    66099.000000
    mean         6.015991
    std          1.398897
    min          1.000000
    25%          5.000000
    50%          6.000000
    75%          7.000000
    max         18.000000
    Name: DBAName, dtype: float64



#### Still higher than expected...

I still didn't expect to see an average of 6 companies providing consumer internet acess in a given census block.

Despite what the data has checked off, I don't believe these are all truly consumer ISPs. https://www.megapath.com/ describes itself as a business internet provider.


```python
def list_providers(town_name):
    return ct_speeds[(ct_speeds["town"] == town_name.title())
                    & (ct_speeds["Consumer"] == 1)]["DBAName"].unique()

list_providers("Bethel")


```




    array(['Frontier Communications Corporation', 'Cablevision', 'Comcast',
           'MegaPath Corporation', 'dishNET Satellite Broadband LLC',
           'Global Capacity LLC', 'ViaSat Inc', 'HughesNet', 'Skycasters',
           'Charter Communications Inc'], dtype=object)




```python
providers[providers["StateAbbr"] == "CT"]\
.drop_duplicates(subset=["ProviderName"]).sort_values(by="blocks", ascending=False)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>FRN</th>
      <th>ProviderName</th>
      <th>StateAbbr</th>
      <th>TechCode</th>
      <th>records</th>
      <th>blocks</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>64</th>
      <td>1568880</td>
      <td>GCI Communication Corp.</td>
      <td>CT</td>
      <td>60</td>
      <td>64389</td>
      <td>64389</td>
    </tr>
    <tr>
      <th>5587</th>
      <td>12369286</td>
      <td>HNS License Sub, LLC</td>
      <td>CT</td>
      <td>60</td>
      <td>64389</td>
      <td>64389</td>
    </tr>
    <tr>
      <th>7159</th>
      <td>18756155</td>
      <td>VSAT Systems, LLC</td>
      <td>CT</td>
      <td>60</td>
      <td>64389</td>
      <td>64389</td>
    </tr>
    <tr>
      <th>3849</th>
      <td>4963088</td>
      <td>ViaSat, Inc.</td>
      <td>CT</td>
      <td>60</td>
      <td>64389</td>
      <td>64389</td>
    </tr>
    <tr>
      <th>4422</th>
      <td>6797849</td>
      <td>Fiber Technologies Networks, L.L.C.</td>
      <td>CT</td>
      <td>50</td>
      <td>32726</td>
      <td>32726</td>
    </tr>
    <tr>
      <th>1242</th>
      <td>3576352</td>
      <td>Frontier Communications Corporation</td>
      <td>CT</td>
      <td>10</td>
      <td>23499</td>
      <td>23499</td>
    </tr>
    <tr>
      <th>2565</th>
      <td>3768165</td>
      <td>COMCAST CABLE COMMUNICATIONS, LLC</td>
      <td>CT</td>
      <td>42</td>
      <td>23000</td>
      <td>23000</td>
    </tr>
    <tr>
      <th>7276</th>
      <td>19027440</td>
      <td>CSC Holdings LLC</td>
      <td>CT</td>
      <td>42</td>
      <td>14652</td>
      <td>14652</td>
    </tr>
    <tr>
      <th>2227</th>
      <td>3746468</td>
      <td>Charter Communications, Inc.</td>
      <td>CT</td>
      <td>42</td>
      <td>8926</td>
      <td>8926</td>
    </tr>
    <tr>
      <th>529</th>
      <td>1834696</td>
      <td>Cox Communications, Inc</td>
      <td>CT</td>
      <td>42</td>
      <td>6024</td>
      <td>6024</td>
    </tr>
    <tr>
      <th>6887</th>
      <td>18589226</td>
      <td>Lightower Fiber Networks I, LLC (formerly know...</td>
      <td>CT</td>
      <td>50</td>
      <td>2754</td>
      <td>2754</td>
    </tr>
    <tr>
      <th>5972</th>
      <td>15005861</td>
      <td>MetroCast Communications of Connecticut, LLC</td>
      <td>CT</td>
      <td>42</td>
      <td>2574</td>
      <td>2574</td>
    </tr>
    <tr>
      <th>4979</th>
      <td>8797680</td>
      <td>Thames Valley Communications, Inc.</td>
      <td>CT</td>
      <td>40</td>
      <td>1529</td>
      <td>1529</td>
    </tr>
    <tr>
      <th>1184</th>
      <td>3469442</td>
      <td>Verizon New York Inc.</td>
      <td>CT</td>
      <td>10</td>
      <td>579</td>
      <td>579</td>
    </tr>
    <tr>
      <th>7627</th>
      <td>20748331</td>
      <td>GC Pivotal LLC d/b/a Global Capacity</td>
      <td>CT</td>
      <td>10</td>
      <td>544</td>
      <td>544</td>
    </tr>
    <tr>
      <th>2370</th>
      <td>3753787</td>
      <td>GC Pivotal, LLC d/b/a Global Capacity</td>
      <td>CT</td>
      <td>10</td>
      <td>544</td>
      <td>544</td>
    </tr>
    <tr>
      <th>8054</th>
      <td>22757645</td>
      <td>dishNET Satellite Broadband, L.L.C.</td>
      <td>CT</td>
      <td>60</td>
      <td>500</td>
      <td>500</td>
    </tr>
    <tr>
      <th>5343</th>
      <td>10856284</td>
      <td>Verizon Business Global LLC dba Verizon Business</td>
      <td>CT</td>
      <td>30</td>
      <td>495</td>
      <td>495</td>
    </tr>
    <tr>
      <th>7981</th>
      <td>22322481</td>
      <td>Cablevision Lightpath CT LLC</td>
      <td>CT</td>
      <td>50</td>
      <td>341</td>
      <td>341</td>
    </tr>
    <tr>
      <th>1492</th>
      <td>3720471</td>
      <td>EarthLink Business, LLC</td>
      <td>CT</td>
      <td>10</td>
      <td>251</td>
      <td>251</td>
    </tr>
    <tr>
      <th>5885</th>
      <td>14942668</td>
      <td>tw telecom holdings, llc</td>
      <td>CT</td>
      <td>30</td>
      <td>199</td>
      <td>199</td>
    </tr>
    <tr>
      <th>5437</th>
      <td>11017795</td>
      <td>PAETEC Communications, Inc.</td>
      <td>CT</td>
      <td>30</td>
      <td>81</td>
      <td>81</td>
    </tr>
    <tr>
      <th>4818</th>
      <td>8307910</td>
      <td>Advanced Corporate Networking, Inc.</td>
      <td>CT</td>
      <td>50</td>
      <td>23</td>
      <td>23</td>
    </tr>
    <tr>
      <th>1772</th>
      <td>3723822</td>
      <td>Level  3 Communications, LLC</td>
      <td>CT</td>
      <td>30</td>
      <td>22</td>
      <td>22</td>
    </tr>
    <tr>
      <th>4043</th>
      <td>5059233</td>
      <td>DSCI LLC</td>
      <td>CT</td>
      <td>30</td>
      <td>15</td>
      <td>15</td>
    </tr>
    <tr>
      <th>5118</th>
      <td>9645201</td>
      <td>Universal Connectivity, Inc.</td>
      <td>CT</td>
      <td>50</td>
      <td>11</td>
      <td>11</td>
    </tr>
    <tr>
      <th>7933</th>
      <td>22098099</td>
      <td>Signal Point Telecommunications Corp.</td>
      <td>CT</td>
      <td>50</td>
      <td>9</td>
      <td>9</td>
    </tr>
    <tr>
      <th>5137</th>
      <td>9815234</td>
      <td>CornerStone Telephone Company, LLC</td>
      <td>CT</td>
      <td>40</td>
      <td>9</td>
      <td>9</td>
    </tr>
    <tr>
      <th>1397</th>
      <td>3716073</td>
      <td>McLeodUSA Telecommunications Services, L.L.C.</td>
      <td>CT</td>
      <td>30</td>
      <td>7</td>
      <td>7</td>
    </tr>
    <tr>
      <th>4289</th>
      <td>6275945</td>
      <td>XO Communications Services, LLC</td>
      <td>CT</td>
      <td>20</td>
      <td>3</td>
      <td>3</td>
    </tr>
    <tr>
      <th>5233</th>
      <td>10296853</td>
      <td>Broadview Networks Holdings, Inc.</td>
      <td>CT</td>
      <td>10</td>
      <td>2</td>
      <td>2</td>
    </tr>
    <tr>
      <th>6806</th>
      <td>18547828</td>
      <td>Telefonica USA, Inc.</td>
      <td>CT</td>
      <td>30</td>
      <td>2</td>
      <td>2</td>
    </tr>
    <tr>
      <th>7294</th>
      <td>19066034</td>
      <td>Cogent Communications Group</td>
      <td>CT</td>
      <td>30</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1896</th>
      <td>3729340</td>
      <td>Cincinnati Bell Any Distance Inc.</td>
      <td>CT</td>
      <td>10</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>7856</th>
      <td>21737515</td>
      <td>APX Net, Inc</td>
      <td>CT</td>
      <td>10</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3283</th>
      <td>4333845</td>
      <td>Spectrotel, Inc.</td>
      <td>CT</td>
      <td>10</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>7544</th>
      <td>20419545</td>
      <td>S-Net Communications, Inc.</td>
      <td>CT</td>
      <td>30</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4229</th>
      <td>6097711</td>
      <td>TVC Albany, Inc.</td>
      <td>CT</td>
      <td>50</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3453</th>
      <td>4350930</td>
      <td>BullsEye Telecom, Inc.</td>
      <td>CT</td>
      <td>10</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>6922</th>
      <td>18602458</td>
      <td>Access One, Inc.</td>
      <td>CT</td>
      <td>30</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>6389</th>
      <td>16555849</td>
      <td>Zayo Group, LLC</td>
      <td>CT</td>
      <td>50</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>6219</th>
      <td>16095432</td>
      <td>Believe Wireless LLC</td>
      <td>CT</td>
      <td>70</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4668</th>
      <td>7651219</td>
      <td>iLOKA Inc. dba NewCloud Networks</td>
      <td>CT</td>
      <td>30</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4763</th>
      <td>8262784</td>
      <td>Inmarsat Mobile Networks, Inc.</td>
      <td>CT</td>
      <td>20</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4985</th>
      <td>8797995</td>
      <td>THE T1 COMPANY, LLC</td>
      <td>CT</td>
      <td>30</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>8354</th>
      <td>24866964</td>
      <td>Sky Fiber Internet</td>
      <td>CT</td>
      <td>70</td>
      <td>1</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
ct_consumer_speeds = ct_speeds[ct_speeds["Consumer"] == 1]

def dbas_in(provider_name, col="tract", df=ct_consumer_speeds):
    return ct_consumer_speeds[(df["DBAName"] == provider_name)]\
.drop_duplicates(subset=[col])

def count_in(provider_name,col="tract"):
    return len(dbas_in('Frontier Communications Corporation',
                       col=col)[col].index)


print count_in('Frontier Communications Corporation',"BlockCode")
print count_in('Frontier Communications Corporation',"tract")
print count_in('Frontier Communications Corporation',"town")
```

    42041
    818
    167



```python

```


```python

```


```python
def test():
    ct_consumer_speeds = ct_speeds[ct_speeds["Consumer"] == 1]
    ct_non_consumer_speeds = ct_speeds[ct_speeds["Consumer"] == 0]
    
    print "Consumer records:" + str(len(ct_consumer_speeds.index))
    print "Non-consumer records:" + str(len(ct_non_consumer_speeds.index))
test()
```

    Consumer records:322480
    Non-consumer records:105308



```python
def blocks_in_state(dba):
    return ct_consumer_speeds[(ct_consumer_speeds["DBAName"] == dba)]

def blocks_in_town(dba):
    df = blocks_in_state(dba)
    return df[(df["town"] == 'Bethel')]

def tracts_in_state(dba):
    return blocks_in_state(dba).drop_duplicates(subset=["tract"])

def towns_in_state(dba):
    return blocks_in_state(dba).drop_duplicates(subset=["town"])

def max_upload(df):
    return df["MaxAdUp"].max()

def max_download(df):
    return df["MaxAdDown"].max()

def row_count(df):
    return len(df.index)

row_count(blocks_in_state('Charter Communications Inc'))
```




    8926




```python
def all_providers ():
    provider_names = []
    provider_count_in_towns = []
    provider_count_in_blocks = []
    ret = []
    for dba in ct_consumer_speeds\
    .drop_duplicates(subset="DBAName")["DBAName"]:
        ret.append([
                dba,
                row_count(towns_in_state(dba)),
                row_count(tracts_in_state(dba)),
                row_count(blocks_in_state(dba)),
                max_upload(blocks_in_state(dba)),
                max_download(blocks_in_state(dba))
            ])
        
#         provider_names.append(dba)
#         provider_count_in_towns.append(count_in(dba, "town"))
#         provider_count_in_blocks.append(count_in(dba,"BlockCode"))
#     return [provider_names,provider_count_in_towns,provider_count_in_blocks]
    return pd.DataFrame(ret,columns=["Provider",
                                     "Consumer Towns",
                                     "Consumer Tracts",
                                     "Consumer Blocks",
                                    "Max Upstream",
                                    "Max Downstream"])

all_providers().sort_values(by="Consumer Blocks",
                           ascending=False)

```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Provider</th>
      <th>Consumer Towns</th>
      <th>Consumer Tracts</th>
      <th>Consumer Blocks</th>
      <th>Max Upstream</th>
      <th>Max Downstream</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>Frontier Communications Corporation</td>
      <td>167</td>
      <td>818</td>
      <td>71349</td>
      <td>6.000</td>
      <td>55.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>ViaSat Inc</td>
      <td>167</td>
      <td>829</td>
      <td>64389</td>
      <td>3.000</td>
      <td>12.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>HughesNet</td>
      <td>167</td>
      <td>829</td>
      <td>64389</td>
      <td>2.000</td>
      <td>15.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Skycasters</td>
      <td>167</td>
      <td>829</td>
      <td>64389</td>
      <td>1.300</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Comcast</td>
      <td>106</td>
      <td>463</td>
      <td>22348</td>
      <td>20.000</td>
      <td>150.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Cablevision</td>
      <td>49</td>
      <td>242</td>
      <td>14648</td>
      <td>35.000</td>
      <td>101.0</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Charter Communications Inc</td>
      <td>51</td>
      <td>93</td>
      <td>8926</td>
      <td>5.000</td>
      <td>100.0</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Cox Communications</td>
      <td>30</td>
      <td>121</td>
      <td>5810</td>
      <td>20.000</td>
      <td>150.0</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Metrocast</td>
      <td>20</td>
      <td>43</td>
      <td>2574</td>
      <td>5.000</td>
      <td>75.0</td>
    </tr>
    <tr>
      <th>0</th>
      <td>Thames Valley Communications</td>
      <td>3</td>
      <td>16</td>
      <td>1529</td>
      <td>20.000</td>
      <td>110.0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Verizon New York Inc.</td>
      <td>2</td>
      <td>15</td>
      <td>1054</td>
      <td>100.000</td>
      <td>100.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>dishNET Satellite Broadband LLC</td>
      <td>132</td>
      <td>311</td>
      <td>500</td>
      <td>1.000</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>MegaPath Corporation</td>
      <td>30</td>
      <td>126</td>
      <td>285</td>
      <td>1.000</td>
      <td>8.0</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Global Capacity LLC</td>
      <td>30</td>
      <td>126</td>
      <td>285</td>
      <td>1.000</td>
      <td>8.0</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Sky Fiber Internet</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>100.000</td>
      <td>100.0</td>
    </tr>
    <tr>
      <th>15</th>
      <td>XO Communications</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>2.000</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Cincinnati Bell Any Distance Inc.</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>0.384</td>
      <td>1.5</td>
    </tr>
    <tr>
      <th>17</th>
      <td>NewCloudNetworks</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>10.000</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>18</th>
      <td>TheT1CompanyLLC</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>12.000</td>
      <td>12.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
ct_speeds[ct_speeds["DBAName"]=="Frontier Communications Corporation"]["MaxAdUp"].max()
ct_speeds[ct_speeds["DBAName"]=="Frontier Communications Corporation"]["MaxAdDown"].max()
```




    55.0




```python

```

#### Differences by blocks and tracks


```python
blocks = ct_speeds["BlockCode"].drop_duplicates().sort_values().to_frame()
tracts_data = ct_speeds["tract"].drop_duplicates().sort_values().to_frame()
```


```python
def tract_df (tract):
    return ct_speeds[ct_speeds["tract"] == tract]
def block_df(block):
    return ct_speeds[ct_speeds["BlockCode"] == block]

def max_download_in_block(block):
    return max_download(block_df(block))

def max_download_in_tract(tract):
    return max_download(tract_df(tract))
```


```python
tracts_data["MaxAdDown"] = tracts_data.apply(lambda x: max_download_in_tract(x["tract"]),axis=1)
tracts_data["MaxAdDown"].value_counts()
```




    150    553
    101    199
    100     50
    75      26
    55       1
    Name: MaxAdDown, dtype: int64




```python
blocks["MaxAdDown"] = blocks.apply(lambda x: max_download_in_block(x["BlockCode"]),axis=1)
blocks["MaxAdUp"] = blocks.apply(lambda x: max_download_in_block(x["BlockCode"]),axis=1)
blocks["MaxAdDown"].value_counts()
```




    150    28114
    101    14407
    15     10486
    100     8593
    75      2549
    0        543
    110      528
    18       465
    55       414
    Name: MaxAdDown, dtype: int64




```python
row_count(blocks[blocks["MaxAdDown"] < 25])
```




    11494




```python
row_count(blocks[blocks["MaxAdDown"] >= 25])
```




    54605




```python
blocks.columns
```




    Index([u'BlockCode', u'MaxAdDown'], dtype='object')




```python
#blocks.apply(lambda x: "{0:d}".format(int(x["BlockCode"])),axis=1).to_frame()
blocks["tract"] = blocks.apply(lambda x: code_to_tract(x["BlockCode"]),axis=1).to_frame()
blocks.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>BlockCode</th>
      <th>MaxAdDown</th>
      <th>tract</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>423588</th>
      <td>90010101011000</td>
      <td>101</td>
      <td>9001010101</td>
    </tr>
    <tr>
      <th>423589</th>
      <td>90010101011001</td>
      <td>101</td>
      <td>9001010101</td>
    </tr>
    <tr>
      <th>423755</th>
      <td>90010101011002</td>
      <td>101</td>
      <td>9001010101</td>
    </tr>
    <tr>
      <th>423653</th>
      <td>90010101011003</td>
      <td>101</td>
      <td>9001010101</td>
    </tr>
    <tr>
      <th>423824</th>
      <td>90010101011004</td>
      <td>101</td>
      <td>9001010101</td>
    </tr>
  </tbody>
</table>
</div>




```python
#blocks[blocks["MaxAdDown"] < 25].to_csv("output/slow_blocks.csv")
```


```python
#tracts["town"] = tracts.apply(lambda x: x["town"].title().strip(), axis=1)

```


```python
blocks = blocks.merge(tracts,left_on="tract",right_on="tract")
blocks.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>BlockCode</th>
      <th>MaxAdDown</th>
      <th>tract</th>
      <th>town_x</th>
      <th>town_y</th>
      <th>MaxAdUp</th>
      <th>town</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>90010101011000</td>
      <td>101</td>
      <td>9001010101</td>
      <td>Greenwich</td>
      <td>Greenwich</td>
      <td>101</td>
      <td>Greenwich</td>
    </tr>
    <tr>
      <th>1</th>
      <td>90010101011001</td>
      <td>101</td>
      <td>9001010101</td>
      <td>Greenwich</td>
      <td>Greenwich</td>
      <td>101</td>
      <td>Greenwich</td>
    </tr>
    <tr>
      <th>2</th>
      <td>90010101011002</td>
      <td>101</td>
      <td>9001010101</td>
      <td>Greenwich</td>
      <td>Greenwich</td>
      <td>101</td>
      <td>Greenwich</td>
    </tr>
    <tr>
      <th>3</th>
      <td>90010101011003</td>
      <td>101</td>
      <td>9001010101</td>
      <td>Greenwich</td>
      <td>Greenwich</td>
      <td>101</td>
      <td>Greenwich</td>
    </tr>
    <tr>
      <th>4</th>
      <td>90010101011004</td>
      <td>101</td>
      <td>9001010101</td>
      <td>Greenwich</td>
      <td>Greenwich</td>
      <td>101</td>
      <td>Greenwich</td>
    </tr>
  </tbody>
</table>
</div>




```python

```


```python
# Identify each town's percentage of slow blocks
# A slow block fails to meet the definition of broadband of 25 Mbps down / 3 up
def pct_slow(df):
    total_blocks = len((df["BlockCode"].unique()))
    #print "total blocks:", total_blocks
    tmpdf = df[(df["MaxAdUp"] > 0) | (df["MaxAdDown"] > 0)]
    tmpdf = tmpdf[(df["MaxAdUp"] < 3) | (df["MaxAdDown"] < 25)]
    slow_blocks = len(tmpdf.index)
    #print "slow_blocks",slow_blocks
    try:
        return [slow_blocks, total_blocks, slow_blocks * 100 / total_blocks]
    except:
        return [0,0,0]

def pct_slow_town(town_name):
    return pct_slow(blocks[blocks["town"] == town_name])

def slow_towns():
    ret = []
    for t in tracts["town"].unique():
        ret.append([t] + pct_slow_town(t))
    return pd.DataFrame(ret, columns=["town","slow blocks","total blocks","percent slow"])

print slow_towns()[["town","percent slow","slow blocks","total blocks"]]\
.sort_values(by="percent slow", ascending=False)\
.to_csv(sep="\t",index=False)
```

    /Library/Python/2.7/site-packages/pandas/core/frame.py:1997: UserWarning: Boolean Series key will be reindexed to match DataFrame index.
      "DataFrame index.", UserWarning)


    town	percent slow	slow blocks	total blocks
     North Stonington	52	82	155
    Salisbury	45	132	293
    Hartland	43	41	94
     Norfolk	42	126	298
     Chester	42	68	161
     Madison	42	207	482
    Sharon	41	108	260
     Haddam	38	127	332
     Lyme	38	64	165
     Portland	38	89	234
    Windsor	37	187	497
     Preston	37	69	186
     Cromwell	37	99	265
     Salem	37	47	124
     Franklin	36	27	73
    Bloomfield	36	193	525
     Colchester	36	120	332
     East Hampton	35	96	274
    East Granby	35	48	135
     Middlebury	35	94	263
    Windsor Locks	35	102	285
     Essex	34	58	168
     Bozrah	34	43	126
    Marlborough	34	54	156
     Branford	33	174	526
    Berlin	33	139	421
     New London	32	163	503
    Rocky Hill	31	100	317
    Suffield	31	28	88
     Middlefield	31	72	229
     Middletown	31	211	662
     Deep River	30	39	126
    Avon	30	107	351
    Farmington	30	134	439
     East Haddam	30	104	341
    North Canaan	30	36	119
     Killingly	30	169	554
    Burlington	29	52	176
    Glastonbury	29	177	600
     North Haven	28	144	507
     North Branford	28	81	289
     Stafford	28	135	468
    East Hartford	28	209	746
    Hartford	27	318	1151
     Sterling	27	33	118
     Waterford	27	143	518
     Lisbon	27	29	105
     East Lyme	27	145	531
     Clinton	27	72	263
     Guilford	26	119	446
     Durham	26	42	157
    Simsbury	26	130	494
    Danbury	25	231	917
    South Windsor	25	117	459
     Prospect	25	31	124
     Ledyard	25	133	513
     Wallingford	25	183	713
     Plainfield	25	100	387
     Old Lyme	24	35	142
     Old Saybrook	24	79	321
     Montville	24	71	294
     New Haven	23	343	1474
     Sprague	23	21	90
    Southington	23	143	607
    Enfield	23	62	268
     Vernon	23	145	623
    Bethel	23	63	273
     Cheshire	23	106	448
     Meriden	22	177	798
     Norwich	22	174	762
    Wethersfield	22	110	488
    New Britain	22	191	852
    Ridgefield	22	98	445
     Ellington	22	68	299
     Naugatuck	22	92	414
     Beacon Falls	22	23	102
     Seymour	21	66	302
    Canton	21	43	204
     Killingworth	21	34	157
     Griswold	21	67	308
     Tolland	21	61	290
     OldLyme	20	42	210
     Westbrook	20	48	238
     Oxford	20	47	234
    East Windsor	19	46	235
     East Haven	19	78	410
     Waterbury	19	292	1486
     Bethany	19	25	130
     Bolton	19	16	82
     Hamden	18	163	884
     Hebron	18	33	180
     Somers	18	51	276
    Manchester	18	168	902
    West Hartford	17	158	892
    Plainville	17	58	324
    Newington	17	76	438
     Wolcott	17	44	252
     Putnam	17	50	283
     Derby	16	32	195
    Shelton	15	86	554
    Somers	15	100	633
    Plymouth	15	38	243
    Granby	14	30	210
     Stonington	13	118	845
     Ansonia	13	40	288
     West Haven	13	91	682
    Bristol	12	104	862
    Colebrook	10	11	103
    New Hartford	8	20	239
    Thomaston	7	15	204
    Barkhamsted	7	14	186
     Groton	6	49	801
    Warren	4	6	121
     Willington	4	7	161
    Cornwall	3	9	246
    Harwinton	3	6	193
     Scotland	2	2	79
    Litchfield	2	11	426
     Lebanon	2	7	246
     Ashford	2	5	205
     Thompson	2	9	385
    Morris	1	2	141
    Redding	1	4	258
    Kent	1	5	269
     Windham	1	5	461
     Woodstock	1	5	323
    Watertown	1	7	461
    Brookfield	1	3	251
     Columbia	0	1	105
    Bridgewater	0	0	90
    NewMilford	0	0	22
    New Milford	0	0	593
     Coventry	0	2	280
     Eastford	0	1	131
     Mansfield	0	0	408
     Canterbury	0	0	142
    New Canaan	0	0	369
     Brooklyn	0	0	177
     Pomfret	0	0	171
     Hampton	0	0	124
    Darien	0	0	476
     Chaplin	0	0	127
    Sherman	0	0	117
     OldSaybrook	0	0	0
    Norwalk	0	10	1270
     Milford	0	1	1017
     Orange	0	0	339
     Woodbridge	0	1	207
    Stamford	0	4	1292
    Woodbury	0	2	294
    Newtown	0	0	615
    New Fairfield	0	0	250
    Bethlehem	0	0	120
    Winchester	0	2	297
    Torrington	0	0	728
     Southbury	0	1	368
    Easton	0	0	188
    Monroe	0	0	337
    Goshen	0	1	190
    Trumbull	0	6	696
    Stratford	0	5	906
    Roxbury	0	0	117
    Bridgeport	0	15	1715
    Washington	0	0	251
    Fairfield	0	0	1167
    Weston	0	0	207
    Westport	0	3	634
    Wilton	0	0	387
    Greenwich	0	7	1246
    



```python
slow_pct_towns = slow_towns()[["town","percent slow","slow blocks","total blocks"]]
slow_pct_towns.sort_values("percent slow", ascending=False)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>town</th>
      <th>percent slow</th>
      <th>slow blocks</th>
      <th>total blocks</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>132</th>
      <td>North Stonington</td>
      <td>52</td>
      <td>82</td>
      <td>155</td>
    </tr>
    <tr>
      <th>57</th>
      <td>Salisbury</td>
      <td>45</td>
      <td>132</td>
      <td>293</td>
    </tr>
    <tr>
      <th>23</th>
      <td>Hartland</td>
      <td>43</td>
      <td>41</td>
      <td>94</td>
    </tr>
    <tr>
      <th>78</th>
      <td>Norfolk</td>
      <td>42</td>
      <td>126</td>
      <td>298</td>
    </tr>
    <tr>
      <th>87</th>
      <td>Chester</td>
      <td>42</td>
      <td>68</td>
      <td>161</td>
    </tr>
    <tr>
      <th>111</th>
      <td>Madison</td>
      <td>42</td>
      <td>207</td>
      <td>482</td>
    </tr>
    <tr>
      <th>58</th>
      <td>Sharon</td>
      <td>41</td>
      <td>108</td>
      <td>260</td>
    </tr>
    <tr>
      <th>85</th>
      <td>Haddam</td>
      <td>38</td>
      <td>127</td>
      <td>332</td>
    </tr>
    <tr>
      <th>121</th>
      <td>Lyme</td>
      <td>38</td>
      <td>64</td>
      <td>165</td>
    </tr>
    <tr>
      <th>82</th>
      <td>Portland</td>
      <td>38</td>
      <td>89</td>
      <td>234</td>
    </tr>
    <tr>
      <th>37</th>
      <td>Windsor</td>
      <td>37</td>
      <td>187</td>
      <td>497</td>
    </tr>
    <tr>
      <th>128</th>
      <td>Preston</td>
      <td>37</td>
      <td>69</td>
      <td>186</td>
    </tr>
    <tr>
      <th>83</th>
      <td>Cromwell</td>
      <td>37</td>
      <td>99</td>
      <td>265</td>
    </tr>
    <tr>
      <th>139</th>
      <td>Salem</td>
      <td>37</td>
      <td>47</td>
      <td>124</td>
    </tr>
    <tr>
      <th>136</th>
      <td>Franklin</td>
      <td>36</td>
      <td>27</td>
      <td>73</td>
    </tr>
    <tr>
      <th>36</th>
      <td>Bloomfield</td>
      <td>36</td>
      <td>193</td>
      <td>525</td>
    </tr>
    <tr>
      <th>138</th>
      <td>Colchester</td>
      <td>36</td>
      <td>120</td>
      <td>332</td>
    </tr>
    <tr>
      <th>81</th>
      <td>East Hampton</td>
      <td>35</td>
      <td>96</td>
      <td>274</td>
    </tr>
    <tr>
      <th>35</th>
      <td>East Granby</td>
      <td>35</td>
      <td>48</td>
      <td>135</td>
    </tr>
    <tr>
      <th>114</th>
      <td>Middlebury</td>
      <td>35</td>
      <td>94</td>
      <td>263</td>
    </tr>
    <tr>
      <th>38</th>
      <td>Windsor Locks</td>
      <td>35</td>
      <td>102</td>
      <td>285</td>
    </tr>
    <tr>
      <th>90</th>
      <td>Essex</td>
      <td>34</td>
      <td>58</td>
      <td>168</td>
    </tr>
    <tr>
      <th>137</th>
      <td>Bozrah</td>
      <td>34</td>
      <td>43</td>
      <td>126</td>
    </tr>
    <tr>
      <th>52</th>
      <td>Marlborough</td>
      <td>34</td>
      <td>54</td>
      <td>156</td>
    </tr>
    <tr>
      <th>108</th>
      <td>Branford</td>
      <td>33</td>
      <td>174</td>
      <td>526</td>
    </tr>
    <tr>
      <th>24</th>
      <td>Berlin</td>
      <td>33</td>
      <td>139</td>
      <td>421</td>
    </tr>
    <tr>
      <th>124</th>
      <td>New London</td>
      <td>32</td>
      <td>163</td>
      <td>503</td>
    </tr>
    <tr>
      <th>44</th>
      <td>Rocky Hill</td>
      <td>31</td>
      <td>100</td>
      <td>317</td>
    </tr>
    <tr>
      <th>40</th>
      <td>Suffield</td>
      <td>31</td>
      <td>28</td>
      <td>88</td>
    </tr>
    <tr>
      <th>80</th>
      <td>Middlefield</td>
      <td>31</td>
      <td>72</td>
      <td>229</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>156</th>
      <td>Hampton</td>
      <td>0</td>
      <td>0</td>
      <td>124</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Darien</td>
      <td>0</td>
      <td>0</td>
      <td>476</td>
    </tr>
    <tr>
      <th>155</th>
      <td>Chaplin</td>
      <td>0</td>
      <td>0</td>
      <td>127</td>
    </tr>
    <tr>
      <th>22</th>
      <td>Sherman</td>
      <td>0</td>
      <td>0</td>
      <td>117</td>
    </tr>
    <tr>
      <th>142</th>
      <td>OldSaybrook</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Norwalk</td>
      <td>0</td>
      <td>10</td>
      <td>1270</td>
    </tr>
    <tr>
      <th>98</th>
      <td>Milford</td>
      <td>0</td>
      <td>1</td>
      <td>1017</td>
    </tr>
    <tr>
      <th>100</th>
      <td>Orange</td>
      <td>0</td>
      <td>0</td>
      <td>339</td>
    </tr>
    <tr>
      <th>101</th>
      <td>Woodbridge</td>
      <td>0</td>
      <td>1</td>
      <td>207</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Stamford</td>
      <td>0</td>
      <td>4</td>
      <td>1292</td>
    </tr>
    <tr>
      <th>76</th>
      <td>Woodbury</td>
      <td>0</td>
      <td>2</td>
      <td>294</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Newtown</td>
      <td>0</td>
      <td>0</td>
      <td>615</td>
    </tr>
    <tr>
      <th>18</th>
      <td>New Fairfield</td>
      <td>0</td>
      <td>0</td>
      <td>250</td>
    </tr>
    <tr>
      <th>73</th>
      <td>Bethlehem</td>
      <td>0</td>
      <td>0</td>
      <td>120</td>
    </tr>
    <tr>
      <th>72</th>
      <td>Winchester</td>
      <td>0</td>
      <td>2</td>
      <td>297</td>
    </tr>
    <tr>
      <th>71</th>
      <td>Torrington</td>
      <td>0</td>
      <td>0</td>
      <td>728</td>
    </tr>
    <tr>
      <th>118</th>
      <td>Southbury</td>
      <td>0</td>
      <td>1</td>
      <td>368</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Easton</td>
      <td>0</td>
      <td>0</td>
      <td>188</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Monroe</td>
      <td>0</td>
      <td>0</td>
      <td>337</td>
    </tr>
    <tr>
      <th>66</th>
      <td>Goshen</td>
      <td>0</td>
      <td>1</td>
      <td>190</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Trumbull</td>
      <td>0</td>
      <td>6</td>
      <td>696</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Stratford</td>
      <td>0</td>
      <td>5</td>
      <td>906</td>
    </tr>
    <tr>
      <th>63</th>
      <td>Roxbury</td>
      <td>0</td>
      <td>0</td>
      <td>117</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Bridgeport</td>
      <td>0</td>
      <td>15</td>
      <td>1715</td>
    </tr>
    <tr>
      <th>62</th>
      <td>Washington</td>
      <td>0</td>
      <td>0</td>
      <td>251</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Fairfield</td>
      <td>0</td>
      <td>0</td>
      <td>1167</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Weston</td>
      <td>0</td>
      <td>0</td>
      <td>207</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Westport</td>
      <td>0</td>
      <td>3</td>
      <td>634</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Wilton</td>
      <td>0</td>
      <td>0</td>
      <td>387</td>
    </tr>
    <tr>
      <th>0</th>
      <td>Greenwich</td>
      <td>0</td>
      <td>7</td>
      <td>1246</td>
    </tr>
  </tbody>
</table>
<p>169 rows × 4 columns</p>
</div>




```python
print slow_pct_towns["total blocks"].sum()
print slow_pct_towns["slow blocks"].sum()
print slow_pct_towns["slow blocks"].sum() * 100 / slow_pct_towns["total blocks"].sum()
```

    66099
    10951
    16



```python

```
