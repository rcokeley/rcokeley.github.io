```python
import pandas as pd
import numpy as np
import altair as alt
from altair import datum


def read_file(file):
    '''reads a csv file and creates a dataframe'''
    dataframe = pd.read_csv(file)
    return dataframe


#open the house csv file and convert to a dataframe
house_df=read_file('house_episodes.csv')

#create a mini dataframe by splitting delimiter of original_air_date with columns Year, Month, Day
y_m_d_df=house_df['original_air_date'].str.split('-', expand=True)
y_m_d_df.columns=['year', 'month', 'day']

#concatenate both dataframes together
house_df= pd.concat([house_original_df,y_m_d_df], axis=1)
```


```python
def create_most_season_df(df):
    ''' create a dataframe only including season and us_viewers'''
    most_season_df=house_df[['season','us_viewers']]
    most_season_df=most_season_df.groupby('season').sum().reset_index()
    return most_season_df

most_season_df=create_most_season_df(house_df)
most_season_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>season</th>
      <th>us_viewers</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>290740000.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>441880000.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>459880000.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>287860000.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>320280000.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6</td>
      <td>276430000.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>7</td>
      <td>228310000.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>8</td>
      <td>157920000.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
def season_with_most_views(df):
    '''create a grey barchart highlighting the season with the most views'''
    barchart = alt.Chart(df).mark_bar(size=30, color='#D3D3D3').encode(
            x=alt.X(
                'season:N',
            axis=alt.Axis(title='Seasons')),
            y=alt.Y(
                'us_viewers:Q',
            axis=alt.Axis(title='US Viewings')),
        color=alt.condition(
            alt.datum.us_viewers>450000000,
            alt.value("#187bc9"),  # The highlighted color
            alt.value("#D3D3D3")
        )
        ).properties(
            width=300,
            height=300,
            title={'text':["Average US Viewers of the Popular","TV Show 'House' per Season"]}
        )
    return barchart.configure_title(fontSize=17,subtitlePadding=8)

vis1=season_with_most_views(most_season_df)
vis1
```





<div id="altair-viz-da03899874fd44a3a92f7962903a045c"></div>
<script type="text/javascript">
  var VEGA_DEBUG = (typeof VEGA_DEBUG == "undefined") ? {} : VEGA_DEBUG;
  (function(spec, embedOpt){
    let outputDiv = document.currentScript.previousElementSibling;
    if (outputDiv.id !== "altair-viz-da03899874fd44a3a92f7962903a045c") {
      outputDiv = document.getElementById("altair-viz-da03899874fd44a3a92f7962903a045c");
    }
    const paths = {
      "vega": "https://cdn.jsdelivr.net/npm//vega@5?noext",
      "vega-lib": "https://cdn.jsdelivr.net/npm//vega-lib?noext",
      "vega-lite": "https://cdn.jsdelivr.net/npm//vega-lite@4.17.0?noext",
      "vega-embed": "https://cdn.jsdelivr.net/npm//vega-embed@6?noext",
    };

    function maybeLoadScript(lib, version) {
      var key = `${lib.replace("-", "")}_version`;
      return (VEGA_DEBUG[key] == version) ?
        Promise.resolve(paths[lib]) :
        new Promise(function(resolve, reject) {
          var s = document.createElement('script');
          document.getElementsByTagName("head")[0].appendChild(s);
          s.async = true;
          s.onload = () => {
            VEGA_DEBUG[key] = version;
            return resolve(paths[lib]);
          };
          s.onerror = () => reject(`Error loading script: ${paths[lib]}`);
          s.src = paths[lib];
        });
    }

    function showError(err) {
      outputDiv.innerHTML = `<div class="error" style="color:red;">${err}</div>`;
      throw err;
    }

    function displayChart(vegaEmbed) {
      vegaEmbed(outputDiv, spec, embedOpt)
        .catch(err => showError(`Javascript Error: ${err.message}<br>This usually means there's a typo in your chart specification. See the javascript console for the full traceback.`));
    }

    if(typeof define === "function" && define.amd) {
      requirejs.config({paths});
      require(["vega-embed"], displayChart, err => showError(`Error loading script: ${err.message}`));
    } else {
      maybeLoadScript("vega", "5")
        .then(() => maybeLoadScript("vega-lite", "4.17.0"))
        .then(() => maybeLoadScript("vega-embed", "6"))
        .catch(showError)
        .then(() => displayChart(vegaEmbed));
    }
  })({"config": {"view": {"continuousWidth": 400, "continuousHeight": 300}, "title": {"fontSize": 17, "subtitlePadding": 8}}, "data": {"name": "data-e6b02c69eecc7a260a922918010c0552"}, "mark": {"type": "bar", "color": "#D3D3D3", "size": 30}, "encoding": {"color": {"condition": {"value": "#187bc9", "test": "(datum.us_viewers > 450000000)"}, "value": "#D3D3D3"}, "x": {"axis": {"title": "Seasons"}, "field": "season", "type": "nominal"}, "y": {"axis": {"title": "US Viewings"}, "field": "us_viewers", "type": "quantitative"}}, "height": 300, "title": {"text": ["Average US Viewers of the Popular", "TV Show 'House' per Season"]}, "width": 300, "$schema": "https://vega.github.io/schema/vega-lite/v4.17.0.json", "datasets": {"data-e6b02c69eecc7a260a922918010c0552": [{"season": 1, "us_viewers": 290740000.0}, {"season": 2, "us_viewers": 441880000.0}, {"season": 3, "us_viewers": 459880000.0}, {"season": 4, "us_viewers": 287860000.0}, {"season": 5, "us_viewers": 320280000.0}, {"season": 6, "us_viewers": 276430000.0}, {"season": 7, "us_viewers": 228310000.0}, {"season": 8, "us_viewers": 157920000.0}]}}, {"mode": "vega-lite"});
</script>




```python
#What are the top 5 directors according to episode count?
def create_director_df(df):
    '''create a director dataframe that returns the top 5 directors for most episodes directed'''
    new_df=df[['directed_by','episode_num_in_season']]
    new_df=new_df.groupby('directed_by').count().reset_index()
    new_df2=df[['directed_by','us_viewers']]
    new_df2=new_df2.groupby('directed_by').mean().reset_index()
    newest_df=pd.merge(new_df,new_df2, on='directed_by')
    return newest_df

director_df=create_director_df(house_df)
director_df.head(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>directed_by</th>
      <th>episode_num_in_season</th>
      <th>us_viewers</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Andrew Bernstein</td>
      <td>5</td>
      <td>13636000.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Bill Johnson</td>
      <td>1</td>
      <td>17480000.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Bryan Singer</td>
      <td>2</td>
      <td>6690000.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Bryan Spicer</td>
      <td>2</td>
      <td>12855000.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Colin Bucksey</td>
      <td>1</td>
      <td>7080000.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
def director_bars(df):
    '''create the visual including bars for viewings and circles for episodes'''
    #create the episode circles
    episode_circles = alt.Chart(df).mark_circle(size=500,color="#187bc9").encode(
            x=alt.X(
                'directed_by:N'),
            y=alt.Y(
                'episode_num_in_season:Q',axis=None
            )
        ).properties(
        title={'text':['Average US Viewers per Director Compared to Total Episodes per Director']},
        height=300,
        width=900)
    
    #create the white episode text over top the circles
    episode_text=episode_circles.mark_text(
                baseline='middle',color='#ffffff').encode(alt.Text('episode_num_in_season:Q'))
    
    #create the grey US View bars
    viewing_bars = alt.Chart(df).mark_bar(size=15,color='#d3d3d3').encode(
            x=alt.X(
                'directed_by:N',axis=alt.Axis(title='Directors')),
            y=alt.Y(
                'us_viewers:Q',axis=alt.Axis(title='Average US Viewers',tickCount=6)
            )
        ).properties(
        height=300,
        width=900)
    #creates two y axis that are independent but displayed at the same time
    final=alt.layer(viewing_bars,episode_circles,episode_text).resolve_scale(
    y = 'independent').configure_view(stroke=None).configure_title(fontSize=17)
    
    return final

vis2=director_bars(director_df)
vis2
```





<div id="altair-viz-16bbd85dbedd4ef2b7a7c7d998b4323f"></div>
<script type="text/javascript">
  var VEGA_DEBUG = (typeof VEGA_DEBUG == "undefined") ? {} : VEGA_DEBUG;
  (function(spec, embedOpt){
    let outputDiv = document.currentScript.previousElementSibling;
    if (outputDiv.id !== "altair-viz-16bbd85dbedd4ef2b7a7c7d998b4323f") {
      outputDiv = document.getElementById("altair-viz-16bbd85dbedd4ef2b7a7c7d998b4323f");
    }
    const paths = {
      "vega": "https://cdn.jsdelivr.net/npm//vega@5?noext",
      "vega-lib": "https://cdn.jsdelivr.net/npm//vega-lib?noext",
      "vega-lite": "https://cdn.jsdelivr.net/npm//vega-lite@4.17.0?noext",
      "vega-embed": "https://cdn.jsdelivr.net/npm//vega-embed@6?noext",
    };

    function maybeLoadScript(lib, version) {
      var key = `${lib.replace("-", "")}_version`;
      return (VEGA_DEBUG[key] == version) ?
        Promise.resolve(paths[lib]) :
        new Promise(function(resolve, reject) {
          var s = document.createElement('script');
          document.getElementsByTagName("head")[0].appendChild(s);
          s.async = true;
          s.onload = () => {
            VEGA_DEBUG[key] = version;
            return resolve(paths[lib]);
          };
          s.onerror = () => reject(`Error loading script: ${paths[lib]}`);
          s.src = paths[lib];
        });
    }

    function showError(err) {
      outputDiv.innerHTML = `<div class="error" style="color:red;">${err}</div>`;
      throw err;
    }

    function displayChart(vegaEmbed) {
      vegaEmbed(outputDiv, spec, embedOpt)
        .catch(err => showError(`Javascript Error: ${err.message}<br>This usually means there's a typo in your chart specification. See the javascript console for the full traceback.`));
    }

    if(typeof define === "function" && define.amd) {
      requirejs.config({paths});
      require(["vega-embed"], displayChart, err => showError(`Error loading script: ${err.message}`));
    } else {
      maybeLoadScript("vega", "5")
        .then(() => maybeLoadScript("vega-lite", "4.17.0"))
        .then(() => maybeLoadScript("vega-embed", "6"))
        .catch(showError)
        .then(() => displayChart(vegaEmbed));
    }
  })({"config": {"view": {"continuousWidth": 400, "continuousHeight": 300, "stroke": null}, "title": {"fontSize": 17}}, "layer": [{"mark": {"type": "bar", "color": "#d3d3d3", "size": 15}, "encoding": {"x": {"axis": {"title": "Directors"}, "field": "directed_by", "type": "nominal"}, "y": {"axis": {"tickCount": 6, "title": "Average US Viewers"}, "field": "us_viewers", "type": "quantitative"}}, "height": 300, "width": 900}, {"mark": {"type": "circle", "color": "#187bc9", "size": 500}, "encoding": {"x": {"field": "directed_by", "type": "nominal"}, "y": {"axis": null, "field": "episode_num_in_season", "type": "quantitative"}}, "height": 300, "title": {"text": ["Average US Viewers per Director Compared to Total Episodes per Director"]}, "width": 900}, {"mark": {"type": "text", "baseline": "middle", "color": "#ffffff"}, "encoding": {"text": {"field": "episode_num_in_season", "type": "quantitative"}, "x": {"field": "directed_by", "type": "nominal"}, "y": {"axis": null, "field": "episode_num_in_season", "type": "quantitative"}}, "height": 300, "title": {"text": ["Average US Viewers per Director Compared to Total Episodes per Director"]}, "width": 900}], "data": {"name": "data-1a865c7d0bbd76c44dbe8e66d89d9c38"}, "resolve": {"scale": {"y": "independent"}}, "$schema": "https://vega.github.io/schema/vega-lite/v4.17.0.json", "datasets": {"data-1a865c7d0bbd76c44dbe8e66d89d9c38": [{"directed_by": "Andrew Bernstein", "episode_num_in_season": 5, "us_viewers": 13636000.0}, {"directed_by": "Bill Johnson", "episode_num_in_season": 1, "us_viewers": 17480000.0}, {"directed_by": "Bryan Singer", "episode_num_in_season": 2, "us_viewers": 6690000.0}, {"directed_by": "Bryan Spicer", "episode_num_in_season": 2, "us_viewers": 12855000.0}, {"directed_by": "Colin Bucksey", "episode_num_in_season": 1, "us_viewers": 7080000.0}, {"directed_by": "Dan Attias", "episode_num_in_season": 8, "us_viewers": 13448750.0}, {"directed_by": "Daniel Sackheim", "episode_num_in_season": 7, "us_viewers": 18877142.85714286}, {"directed_by": "David Platt", "episode_num_in_season": 5, "us_viewers": 14524000.0}, {"directed_by": "David Semel", "episode_num_in_season": 3, "us_viewers": 19236666.666666668}, {"directed_by": "David Shore", "episode_num_in_season": 2, "us_viewers": 17095000.0}, {"directed_by": "David Straiton", "episode_num_in_season": 16, "us_viewers": 13103125.0}, {"directed_by": "Deran Sarafian", "episode_num_in_season": 22, "us_viewers": 17379545.454545453}, {"directed_by": "Elodie Keene", "episode_num_in_season": 1, "us_viewers": 21570000.0}, {"directed_by": "Fred Gerber", "episode_num_in_season": 3, "us_viewers": 17476666.666666668}, {"directed_by": "Frederick King Keller", "episode_num_in_season": 2, "us_viewers": 16135000.0}, {"directed_by": "F\u00e9lix Alcal\u00e1", "episode_num_in_season": 1, "us_viewers": 22710000.0}, {"directed_by": "Gloria Muzio", "episode_num_in_season": 1, "us_viewers": 14720000.0}, {"directed_by": "Greg Yaitanes", "episode_num_in_season": 30, "us_viewers": 11414000.0}, {"directed_by": "Guy Ferland", "episode_num_in_season": 1, "us_viewers": 12370000.0}, {"directed_by": "Hugh Laurie", "episode_num_in_season": 2, "us_viewers": 8625000.0}, {"directed_by": "Jace Alexander", "episode_num_in_season": 1, "us_viewers": 14830000.0}, {"directed_by": "Jim Hayman", "episode_num_in_season": 2, "us_viewers": 13720000.0}, {"directed_by": "John F. Showalter", "episode_num_in_season": 1, "us_viewers": 24520000.0}, {"directed_by": "Juan J. Campanella", "episode_num_in_season": 5, "us_viewers": 15886000.0}, {"directed_by": "Julian Higgins", "episode_num_in_season": 1, "us_viewers": 6670000.0}, {"directed_by": "Kate Woods", "episode_num_in_season": 1, "us_viewers": 7550000.0}, {"directed_by": "Katie Jacobs", "episode_num_in_season": 6, "us_viewers": 17030000.0}, {"directed_by": "Keith Gordon", "episode_num_in_season": 1, "us_viewers": 15530000.0}, {"directed_by": "Laura Innes", "episode_num_in_season": 1, "us_viewers": 13670000.0}, {"directed_by": "Lesli Linka Glatter", "episode_num_in_season": 3, "us_viewers": 14996666.666666666}, {"directed_by": "Martha Mitchell", "episode_num_in_season": 2, "us_viewers": 21870000.0}, {"directed_by": "Matt Shakman", "episode_num_in_season": 5, "us_viewers": 14130000.0}, {"directed_by": "Matthew Penn", "episode_num_in_season": 1, "us_viewers": 12190000.0}, {"directed_by": "Miguel Sapochnik", "episode_num_in_season": 6, "us_viewers": 8615000.0}, {"directed_by": "Nelson McCormick", "episode_num_in_season": 1, "us_viewers": 14220000.0}, {"directed_by": "Newton Thomas Sigel", "episode_num_in_season": 2, "us_viewers": 10630000.0}, {"directed_by": "Nick Gomez", "episode_num_in_season": 1, "us_viewers": 12250000.0}, {"directed_by": "Paris Barclay", "episode_num_in_season": 1, "us_viewers": 17680000.0}, {"directed_by": "Paul McCrane", "episode_num_in_season": 1, "us_viewers": 20810000.0}, {"directed_by": "Peter Medak", "episode_num_in_season": 1, "us_viewers": 6730000.0}, {"directed_by": "Peter O'Fallon", "episode_num_in_season": 4, "us_viewers": 14547500.0}, {"directed_by": "Peter Weller", "episode_num_in_season": 1, "us_viewers": 6090000.0}, {"directed_by": "Randy Zisk", "episode_num_in_season": 1, "us_viewers": 17330000.0}, {"directed_by": "S. J. Clarkson", "episode_num_in_season": 1, "us_viewers": 11010000.0}, {"directed_by": "Sanford Bookstaver", "episode_num_in_season": 5, "us_viewers": 9646000.0}, {"directed_by": "Stefan Schwartz", "episode_num_in_season": 1, "us_viewers": 6490000.0}, {"directed_by": "Tim Hunter", "episode_num_in_season": 1, "us_viewers": 17340000.0}, {"directed_by": "Tim Southam", "episode_num_in_season": 2, "us_viewers": 7370000.0}, {"directed_by": "Tony To", "episode_num_in_season": 1, "us_viewers": 11770000.0}, {"directed_by": "Tucker Gates", "episode_num_in_season": 2, "us_viewers": 10130000.0}]}}, {"mode": "vega-lite"});
</script>




```python
def create_annotations():
    '''creates annotations to be used later'''
    annotations=[['Frozen','2008-02-03',32000000],
                 ['One Day,\n One Room','2007-01-30',34000000],
                 ['No Reason','2006-05-23',29000000],
                ['Honeymoon','2005-05-24',22000000]]
    a_df = pd.DataFrame(annotations, columns=['episode_name','date','viewings'])
    a_df =a_df[a_df['date'].notnull()].copy()
    a_df['date'] = a_df['date'].astype('datetime64[ns]')
    return a_df
create_annotations()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>episode_name</th>
      <th>date</th>
      <th>viewings</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Frozen</td>
      <td>2008-02-03</td>
      <td>32000000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>One Day,\n One Room</td>
      <td>2007-01-30</td>
      <td>34000000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>No Reason</td>
      <td>2006-05-23</td>
      <td>29000000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Honeymoon</td>
      <td>2005-05-24</td>
      <td>22000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
def create_viewings_by_time(df):
    '''create a viewings by time dataframe, converting date column to datetime'''
    viewings_by_time_df=house_df[['original_air_date','us_viewers']]
    viewings_by_time_df =viewings_by_time_df[viewings_by_time_df['original_air_date'].notnull()].copy()
    viewings_by_time_df['original_air_date'] = viewings_by_time_df['original_air_date'].astype('datetime64[ns]')
    return viewings_by_time_df

viewing_by_time_df=create_viewings_by_time(house_df)
viewing_by_time_df.head(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>original_air_date</th>
      <th>us_viewers</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2004-11-16</td>
      <td>7050000.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2004-11-23</td>
      <td>6090000.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2004-11-30</td>
      <td>6330000.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2004-12-07</td>
      <td>6740000.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2004-12-14</td>
      <td>6910000.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
#plot the entire us_viewings data from season 1 to season 8 as a time_series line chart
def viewings_by_time(df,a_df=create_annotations()):
    '''create a timeseries plot highlighting the peak increases in popular episodes'''
    line=alt.Chart(df).mark_line(color="#187bc9").encode(
        x=alt.X(
            'original_air_date:T',
            axis=alt.Axis(title=None,tickCount=8)
                    ),
        y=alt.Y(
            'us_viewers:Q',
            axis=alt.Axis(title="US Viewers",tickCount=5),
            scale=alt.Scale(domain=(0,50000000))
                    )
        ).properties(
            width=800,
            height=300,
            title={'text':'Popular TV Show "House" US Viewers from 2005 to 2012',
                   'subtitle':'Most popular episodes indicated on graph'}
        )
    #create the text for the annotations
    text=alt.Chart(a_df).mark_text(lineBreak='\n',fontSize=12).encode(x='date:T',y='viewings',text='episode_name')
    
    #layer the chart and text together
    final=(line+text).configure_title(fontSize=20,subtitleFontSize=17,subtitlePadding=8)
    return final

vis3=viewings_by_time(viewing_by_time_df)
vis3
```





<div id="altair-viz-f3d90409b4b04e93b749a50623457e74"></div>
<script type="text/javascript">
  var VEGA_DEBUG = (typeof VEGA_DEBUG == "undefined") ? {} : VEGA_DEBUG;
  (function(spec, embedOpt){
    let outputDiv = document.currentScript.previousElementSibling;
    if (outputDiv.id !== "altair-viz-f3d90409b4b04e93b749a50623457e74") {
      outputDiv = document.getElementById("altair-viz-f3d90409b4b04e93b749a50623457e74");
    }
    const paths = {
      "vega": "https://cdn.jsdelivr.net/npm//vega@5?noext",
      "vega-lib": "https://cdn.jsdelivr.net/npm//vega-lib?noext",
      "vega-lite": "https://cdn.jsdelivr.net/npm//vega-lite@4.17.0?noext",
      "vega-embed": "https://cdn.jsdelivr.net/npm//vega-embed@6?noext",
    };

    function maybeLoadScript(lib, version) {
      var key = `${lib.replace("-", "")}_version`;
      return (VEGA_DEBUG[key] == version) ?
        Promise.resolve(paths[lib]) :
        new Promise(function(resolve, reject) {
          var s = document.createElement('script');
          document.getElementsByTagName("head")[0].appendChild(s);
          s.async = true;
          s.onload = () => {
            VEGA_DEBUG[key] = version;
            return resolve(paths[lib]);
          };
          s.onerror = () => reject(`Error loading script: ${paths[lib]}`);
          s.src = paths[lib];
        });
    }

    function showError(err) {
      outputDiv.innerHTML = `<div class="error" style="color:red;">${err}</div>`;
      throw err;
    }

    function displayChart(vegaEmbed) {
      vegaEmbed(outputDiv, spec, embedOpt)
        .catch(err => showError(`Javascript Error: ${err.message}<br>This usually means there's a typo in your chart specification. See the javascript console for the full traceback.`));
    }

    if(typeof define === "function" && define.amd) {
      requirejs.config({paths});
      require(["vega-embed"], displayChart, err => showError(`Error loading script: ${err.message}`));
    } else {
      maybeLoadScript("vega", "5")
        .then(() => maybeLoadScript("vega-lite", "4.17.0"))
        .then(() => maybeLoadScript("vega-embed", "6"))
        .catch(showError)
        .then(() => displayChart(vegaEmbed));
    }
  })({"config": {"view": {"continuousWidth": 400, "continuousHeight": 300}, "title": {"fontSize": 20, "subtitleFontSize": 17, "subtitlePadding": 8}}, "layer": [{"data": {"name": "data-74c43ef059469991efa000c485a8794a"}, "mark": {"type": "line", "color": "#187bc9"}, "encoding": {"x": {"axis": {"tickCount": 8, "title": null}, "field": "original_air_date", "type": "temporal"}, "y": {"axis": {"tickCount": 5, "title": "US Viewers"}, "field": "us_viewers", "scale": {"domain": [0, 50000000]}, "type": "quantitative"}}, "height": 300, "title": {"text": "Popular TV Show \"House\" US Viewers from 2005 to 2012", "subtitle": "Most popular episodes indicated on graph"}, "width": 800}, {"data": {"name": "data-caa7a5a6ac2801895a52d57f488ce7a3"}, "mark": {"type": "text", "fontSize": 12, "lineBreak": "\n"}, "encoding": {"text": {"field": "episode_name", "type": "nominal"}, "x": {"field": "date", "type": "temporal"}, "y": {"field": "viewings", "type": "quantitative"}}}], "$schema": "https://vega.github.io/schema/vega-lite/v4.17.0.json", "datasets": {"data-74c43ef059469991efa000c485a8794a": [{"original_air_date": "2004-11-16T00:00:00", "us_viewers": 7050000.0}, {"original_air_date": "2004-11-23T00:00:00", "us_viewers": 6090000.0}, {"original_air_date": "2004-11-30T00:00:00", "us_viewers": 6330000.0}, {"original_air_date": "2004-12-07T00:00:00", "us_viewers": 6740000.0}, {"original_air_date": "2004-12-14T00:00:00", "us_viewers": 6910000.0}, {"original_air_date": "2004-12-21T00:00:00", "us_viewers": 6730000.0}, {"original_air_date": "2004-12-28T00:00:00", "us_viewers": 6910000.0}, {"original_air_date": "2005-01-25T00:00:00", "us_viewers": 12370000.0}, {"original_air_date": "2005-02-01T00:00:00", "us_viewers": 12750000.0}, {"original_air_date": "2005-02-08T00:00:00", "us_viewers": 14970000.0}, {"original_air_date": "2005-02-15T00:00:00", "us_viewers": 14220000.0}, {"original_air_date": "2005-02-22T00:00:00", "us_viewers": 15530000.0}, {"original_air_date": "2005-03-01T00:00:00", "us_viewers": 15530000.0}, {"original_air_date": "2005-03-15T00:00:00", "us_viewers": 17330000.0}, {"original_air_date": "2005-03-22T00:00:00", "us_viewers": 17340000.0}, {"original_air_date": "2005-03-29T00:00:00", "us_viewers": 18280000.0}, {"original_air_date": "2005-04-12T00:00:00", "us_viewers": 15040000.0}, {"original_air_date": "2005-04-19T00:00:00", "us_viewers": 17480000.0}, {"original_air_date": "2005-05-03T00:00:00", "us_viewers": 17140000.0}, {"original_air_date": "2005-05-10T00:00:00", "us_viewers": 18800000.0}, {"original_air_date": "2005-05-17T00:00:00", "us_viewers": 17680000.0}, {"original_air_date": "2005-05-24T00:00:00", "us_viewers": 19520000.0}, {"original_air_date": "2005-09-13T00:00:00", "us_viewers": 15910000.0}, {"original_air_date": "2005-09-20T00:00:00", "us_viewers": 13640000.0}, {"original_air_date": "2005-09-27T00:00:00", "us_viewers": 13370000.0}, {"original_air_date": "2005-11-01T00:00:00", "us_viewers": 12180000.0}, {"original_air_date": "2005-11-08T00:00:00", "us_viewers": 14150000.0}, {"original_air_date": "2005-11-15T00:00:00", "us_viewers": 12950000.0}, {"original_air_date": "2005-11-22T00:00:00", "us_viewers": 14720000.0}, {"original_air_date": "2005-11-29T00:00:00", "us_viewers": 14910000.0}, {"original_air_date": "2005-12-13T00:00:00", "us_viewers": 14520000.0}, {"original_air_date": "2006-01-10T00:00:00", "us_viewers": 14830000.0}, {"original_air_date": "2006-02-07T00:00:00", "us_viewers": 22240000.0}, {"original_air_date": "2006-02-14T00:00:00", "us_viewers": 19200000.0}, {"original_air_date": "2006-02-20T00:00:00", "us_viewers": 14180000.0}, {"original_air_date": "2006-03-07T00:00:00", "us_viewers": 20560000.0}, {"original_air_date": "2006-03-28T00:00:00", "us_viewers": 21440000.0}, {"original_air_date": "2006-04-04T00:00:00", "us_viewers": 22710000.0}, {"original_air_date": "2006-04-11T00:00:00", "us_viewers": 21200000.0}, {"original_air_date": "2006-04-18T00:00:00", "us_viewers": 22640000.0}, {"original_air_date": "2006-04-25T00:00:00", "us_viewers": 24520000.0}, {"original_air_date": "2006-05-02T00:00:00", "us_viewers": 22710000.0}, {"original_air_date": "2006-05-03T00:00:00", "us_viewers": 17160000.0}, {"original_air_date": "2006-05-09T00:00:00", "us_viewers": 24290000.0}, {"original_air_date": "2006-05-16T00:00:00", "us_viewers": 22380000.0}, {"original_air_date": "2006-05-23T00:00:00", "us_viewers": 25470000.0}, {"original_air_date": "2006-09-05T00:00:00", "us_viewers": 19550000.0}, {"original_air_date": "2006-09-12T00:00:00", "us_viewers": 15740000.0}, {"original_air_date": "2006-09-19T00:00:00", "us_viewers": 13670000.0}, {"original_air_date": "2006-09-26T00:00:00", "us_viewers": 14520000.0}, {"original_air_date": "2006-10-31T00:00:00", "us_viewers": 14180000.0}, {"original_air_date": "2006-11-07T00:00:00", "us_viewers": 16110000.0}, {"original_air_date": "2006-11-14T00:00:00", "us_viewers": 14600000.0}, {"original_air_date": "2006-11-21T00:00:00", "us_viewers": 15200000.0}, {"original_air_date": "2006-11-28T00:00:00", "us_viewers": 17300000.0}, {"original_air_date": "2006-12-12T00:00:00", "us_viewers": 11770000.0}, {"original_air_date": "2007-01-09T00:00:00", "us_viewers": 17780000.0}, {"original_air_date": "2007-01-30T00:00:00", "us_viewers": 27340000.0}, {"original_air_date": "2007-02-06T00:00:00", "us_viewers": 24880000.0}, {"original_air_date": "2007-02-13T00:00:00", "us_viewers": 25990000.0}, {"original_air_date": "2007-03-06T00:00:00", "us_viewers": 24400000.0}, {"original_air_date": "2007-03-27T00:00:00", "us_viewers": 20800000.0}, {"original_air_date": "2007-04-03T00:00:00", "us_viewers": 20350000.0}, {"original_air_date": "2007-04-10T00:00:00", "us_viewers": 21570000.0}, {"original_air_date": "2007-04-17T00:00:00", "us_viewers": 22410000.0}, {"original_air_date": "2007-04-24T00:00:00", "us_viewers": 20810000.0}, {"original_air_date": "2007-05-01T00:00:00", "us_viewers": 21130000.0}, {"original_air_date": "2007-05-08T00:00:00", "us_viewers": 21360000.0}, {"original_air_date": "2007-05-15T00:00:00", "us_viewers": 21190000.0}, {"original_air_date": "2007-05-29T00:00:00", "us_viewers": 17230000.0}, {"original_air_date": "2007-09-25T00:00:00", "us_viewers": 14520000.0}, {"original_air_date": "2007-10-02T00:00:00", "us_viewers": 17440000.0}, {"original_air_date": "2007-10-09T00:00:00", "us_viewers": 18030000.0}, {"original_air_date": "2007-10-23T00:00:00", "us_viewers": 18100000.0}, {"original_air_date": "2007-10-30T00:00:00", "us_viewers": 17290000.0}, {"original_air_date": "2007-11-06T00:00:00", "us_viewers": 18170000.0}, {"original_air_date": "2007-11-13T00:00:00", "us_viewers": 16950000.0}, {"original_air_date": "2007-11-20T00:00:00", "us_viewers": 16880000.0}, {"original_air_date": "2007-11-27T00:00:00", "us_viewers": 16960000.0}, {"original_air_date": "2008-01-29T00:00:00", "us_viewers": 22560000.0}, {"original_air_date": "2008-02-03T00:00:00", "us_viewers": 29040000.0}, {"original_air_date": "2008-02-05T00:00:00", "us_viewers": 23150000.0}, {"original_air_date": "2008-04-28T00:00:00", "us_viewers": 14510000.0}, {"original_air_date": "2008-05-05T00:00:00", "us_viewers": 13260000.0}, {"original_air_date": "2008-05-12T00:00:00", "us_viewers": 14840000.0}, {"original_air_date": "2008-05-19T00:00:00", "us_viewers": 16160000.0}, {"original_air_date": "2008-09-16T00:00:00", "us_viewers": 14770000.0}, {"original_air_date": "2008-09-23T00:00:00", "us_viewers": 12370000.0}, {"original_air_date": "2008-09-30T00:00:00", "us_viewers": 12970000.0}, {"original_air_date": "2008-10-14T00:00:00", "us_viewers": 13260000.0}, {"original_air_date": "2008-10-21T00:00:00", "us_viewers": 13080000.0}, {"original_air_date": "2008-10-28T00:00:00", "us_viewers": 13490000.0}, {"original_air_date": "2008-11-11T00:00:00", "us_viewers": 13060000.0}, {"original_air_date": "2008-11-18T00:00:00", "us_viewers": 13260000.0}, {"original_air_date": "2008-11-25T00:00:00", "us_viewers": 12870000.0}, {"original_air_date": "2008-12-02T00:00:00", "us_viewers": 12510000.0}, {"original_air_date": "2008-12-09T00:00:00", "us_viewers": 14050000.0}, {"original_air_date": "2009-01-19T00:00:00", "us_viewers": 15020000.0}, {"original_air_date": "2009-01-26T00:00:00", "us_viewers": 15690000.0}, {"original_air_date": "2009-02-02T00:00:00", "us_viewers": 14870000.0}, {"original_air_date": "2009-02-16T00:00:00", "us_viewers": 14190000.0}, {"original_air_date": "2009-02-23T00:00:00", "us_viewers": 14850000.0}, {"original_air_date": "2009-03-09T00:00:00", "us_viewers": 12380000.0}, {"original_air_date": "2009-03-16T00:00:00", "us_viewers": 13130000.0}, {"original_air_date": "2009-03-30T00:00:00", "us_viewers": 12510000.0}, {"original_air_date": "2009-04-06T00:00:00", "us_viewers": 13290000.0}, {"original_air_date": "2009-04-13T00:00:00", "us_viewers": 12190000.0}, {"original_air_date": "2009-04-27T00:00:00", "us_viewers": 11690000.0}, {"original_air_date": "2009-05-04T00:00:00", "us_viewers": 12040000.0}, {"original_air_date": "2009-05-11T00:00:00", "us_viewers": 12740000.0}, {"original_air_date": "2009-09-21T00:00:00", "us_viewers": 15760000.0}, {"original_air_date": "2009-09-21T00:00:00", "us_viewers": 15760000.0}, {"original_air_date": "2009-09-28T00:00:00", "us_viewers": 14440000.0}, {"original_air_date": "2009-10-05T00:00:00", "us_viewers": 13740000.0}, {"original_air_date": "2009-10-12T00:00:00", "us_viewers": 13500000.0}, {"original_air_date": "2009-10-19T00:00:00", "us_viewers": 11650000.0}, {"original_air_date": "2009-11-09T00:00:00", "us_viewers": 13310000.0}, {"original_air_date": "2009-11-16T00:00:00", "us_viewers": 12670000.0}, {"original_air_date": "2009-11-23T00:00:00", "us_viewers": 11950000.0}, {"original_air_date": "2009-11-30T00:00:00", "us_viewers": 13240000.0}, {"original_air_date": "2010-01-11T00:00:00", "us_viewers": 12250000.0}, {"original_air_date": "2010-01-25T00:00:00", "us_viewers": 14210000.0}, {"original_air_date": "2010-02-01T00:00:00", "us_viewers": 13380000.0}, {"original_air_date": "2010-02-08T00:00:00", "us_viewers": 13600000.0}, {"original_air_date": "2010-03-08T00:00:00", "us_viewers": 12810000.0}, {"original_air_date": "2010-03-15T00:00:00", "us_viewers": 11370000.0}, {"original_air_date": "2010-04-12T00:00:00", "us_viewers": 10800000.0}, {"original_air_date": "2010-04-19T00:00:00", "us_viewers": 10810000.0}, {"original_air_date": "2010-04-26T00:00:00", "us_viewers": 10850000.0}, {"original_air_date": "2010-05-03T00:00:00", "us_viewers": 9980000.0}, {"original_air_date": "2010-05-10T00:00:00", "us_viewers": 9290000.0}, {"original_air_date": "2010-05-17T00:00:00", "us_viewers": 11060000.0}, {"original_air_date": "2010-09-20T00:00:00", "us_viewers": 10540000.0}, {"original_air_date": "2010-09-27T00:00:00", "us_viewers": 10180000.0}, {"original_air_date": "2010-10-04T00:00:00", "us_viewers": 10780000.0}, {"original_air_date": "2010-10-11T00:00:00", "us_viewers": 9690000.0}, {"original_air_date": "2010-10-18T00:00:00", "us_viewers": 9650000.0}, {"original_air_date": "2010-11-08T00:00:00", "us_viewers": 9630000.0}, {"original_air_date": "2010-11-15T00:00:00", "us_viewers": 10770000.0}, {"original_air_date": "2010-11-22T00:00:00", "us_viewers": 9240000.0}, {"original_air_date": "2011-01-17T00:00:00", "us_viewers": 10520000.0}, {"original_air_date": "2011-01-24T00:00:00", "us_viewers": 10450000.0}, {"original_air_date": "2011-02-07T00:00:00", "us_viewers": 12330000.0}, {"original_air_date": "2011-02-14T00:00:00", "us_viewers": 9860000.0}, {"original_air_date": "2011-02-21T00:00:00", "us_viewers": 10410000.0}, {"original_air_date": "2011-02-28T00:00:00", "us_viewers": 11010000.0}, {"original_air_date": "2011-03-07T00:00:00", "us_viewers": 11080000.0}, {"original_air_date": "2011-03-14T00:00:00", "us_viewers": 10410000.0}, {"original_air_date": "2011-03-21T00:00:00", "us_viewers": 9490000.0}, {"original_air_date": "2011-04-11T00:00:00", "us_viewers": 8930000.0}, {"original_air_date": "2011-04-18T00:00:00", "us_viewers": 8800000.0}, {"original_air_date": "2011-05-02T00:00:00", "us_viewers": 8570000.0}, {"original_air_date": "2011-05-09T00:00:00", "us_viewers": 7940000.0}, {"original_air_date": "2011-05-16T00:00:00", "us_viewers": 8920000.0}, {"original_air_date": "2011-05-23T00:00:00", "us_viewers": 9110000.0}, {"original_air_date": "2011-10-03T00:00:00", "us_viewers": 9780000.0}, {"original_air_date": "2011-10-10T00:00:00", "us_viewers": 6850000.0}, {"original_air_date": "2011-10-17T00:00:00", "us_viewers": 8340000.0}, {"original_air_date": "2011-10-31T00:00:00", "us_viewers": 6650000.0}, {"original_air_date": "2011-11-07T00:00:00", "us_viewers": 7550000.0}, {"original_air_date": "2011-11-14T00:00:00", "us_viewers": 6630000.0}, {"original_air_date": "2011-11-21T00:00:00", "us_viewers": 7460000.0}, {"original_air_date": "2011-11-28T00:00:00", "us_viewers": 7410000.0}, {"original_air_date": "2012-01-23T00:00:00", "us_viewers": 8760000.0}, {"original_air_date": "2012-01-30T00:00:00", "us_viewers": 8730000.0}, {"original_air_date": "2012-02-06T00:00:00", "us_viewers": 7090000.0}, {"original_air_date": "2012-02-13T00:00:00", "us_viewers": 7160000.0}, {"original_air_date": "2012-02-20T00:00:00", "us_viewers": 7080000.0}, {"original_air_date": "2012-03-19T00:00:00", "us_viewers": 5940000.0}, {"original_air_date": "2012-04-02T00:00:00", "us_viewers": 6670000.0}, {"original_air_date": "2012-04-09T00:00:00", "us_viewers": 6010000.0}, {"original_air_date": "2012-04-16T00:00:00", "us_viewers": 5610000.0}, {"original_air_date": "2012-04-23T00:00:00", "us_viewers": 6490000.0}, {"original_air_date": "2012-04-30T00:00:00", "us_viewers": 6450000.0}, {"original_air_date": "2012-05-07T00:00:00", "us_viewers": 6090000.0}, {"original_air_date": "2012-05-14T00:00:00", "us_viewers": 6450000.0}, {"original_air_date": "2012-05-21T00:00:00", "us_viewers": 8720000.0}], "data-caa7a5a6ac2801895a52d57f488ce7a3": [{"episode_name": "Frozen", "date": "2008-02-03T00:00:00", "viewings": 32000000}, {"episode_name": "One Day,\n One Room", "date": "2007-01-30T00:00:00", "viewings": 34000000}, {"episode_name": "No Reason", "date": "2006-05-23T00:00:00", "viewings": 29000000}, {"episode_name": "Honeymoon", "date": "2005-05-24T00:00:00", "viewings": 22000000}]}}, {"mode": "vega-lite"});
</script>




```python

```


```python

```
