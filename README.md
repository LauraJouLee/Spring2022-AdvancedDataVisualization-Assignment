#Spring2022-AdvancedDataVisualization-Assignment

<html>
  <head>
    <title>Assignment 2-Tree Explore-Jou</title>
    <script src="https://cdn.jsdelivr.net/npm/vega@5.21.0"></script>
    <script src="https://cdn.jsdelivr.net/npm/vega-lite@5.2.0"></script>
    <script src="https://cdn.jsdelivr.net/npm/vega-embed@6.20.2"></script>
    <script src="https://cdn.jsdelivr.net/npm/d3@6"></script>
</head>

<body>
  <h1>Assignment 2-Tree Explore</h1>
  <h2>Jou Lee</h2>
  <p>Brush the first bar graph to narrow down tree data.</p> 
  <p>Zoom in for tree details in the scatter plot below!</p>

  <div id="vis"></div>

<script type="text/javascript">
  var yourVlSpec = {

  "$schema": "https://vega.github.io/schema/vega-lite/v5.json",
  "config": {"concat": {"spacing": 20}},

  "data": {
    "url": "https://gis-cityofchampaign.opendata.arcgis.com/datasets/979bbeefffea408e8f1cb7a397196c64_22.csv?outSR=%7B%22latestWkid%22%3A3857%2C%22wkid%22%3A102100%7D", 
    "format": {"type": "csv"}
  },

  "transform": [{"filter": {"field": "COND", "valid": false}}],

  "vconcat":[
  {
    "hconcat": [
    {
      "layer": [
        {
          "params": [{"name": "brush", "select": {"type": "interval", "encodings": ["x"]}}],
          "mark": "bar",
          "width": 300, "height": 300,
          "encoding": {
            "x": {
              "timeUnit": "year", 
              "field": "INSPECT_DT", 
              "type": "temporal", 
              "scale": {"domain": [2011, 2023]}
            },
            "y": {"field": "OBJECTID", "aggregate": "count"}
          }
        },
        {
          "transform": [{"filter": {"param": "brush"}}],
          "mark": "bar",
          "encoding": {
            "x": {
              "timeUnit": "year", 
              "field": "INSPECT_DT", 
              "bin": true, 
              "title": "Inspect Year"
            },
            "y": {"aggregate": "count", "title": "Counts of Record"},
            "color": {"value": "goldenrod"}
          }
        }
      ]
    },
    {
      "width": 300, "height": 300,
      "mark": "bar",
      "transform": [{"filter": {"param": "brush"}}],
      "encoding": {
        "x": {
          "timeUnit": "month", 
          "field": "INSPECT_DT", 
          "type": "temporal", 
          "title": "Inspect Month"
        },
        "y": {
          "field": "OBJECTID", 
          "aggregate": "count", 
          "title": null
        },
        "color": {
          "field": "COND", 
          "type": "nominal", 
          "scale": {"scheme": "redyellowgreen"},
          "sort": {"field": "COND", "order": ["N/A", "Dead", "Critical", "Poor", "Fair", "Good", "Very Good", "Excellent"]}
        }
      }
    },
    {
      "height": 300,
      "mark": "bar",
      "transform": [{"filter": {"param": "brush"}}],
      "encoding": {
        "x": {
          "field": "COND", 
          "type": "nominal", 
          "sort":["N/A", "Dead", "Critical", "Poor", "Fair", "Good", "Very Good", "Excellent"],
        },
        "y": {
          "field": "OBJECTID", 
          "aggregate": "count", 
          "title": null
        },
        "color": {"field": "COND", "type": "nominal"}
      }
    }
  ]},
  {
    "params": [{
      "name": "grid",
      "select": "interval",
      "bind": "scales"
    }],

    "transform": [{"filter": {"field": "COND", "valid": false}}],

    "width": 900, 
    "height": 500,
    "mark": "circle",
    "transform": [{"filter": {"param": "brush"}}],
    "encoding": {
      "x": {
        "field": "X", "type": "quantitative", 
        "axis": {"labelOverlap": true},
        "scale": {"domain": [-9840000, -9820000]}
      },
      "y": {
        "field": "Y", "type": "quantitative", 
        "axis": {"labelOverlap": true},
        "scale": {"domain": [4875000, 4890000]}
      },
      "color": {"field": "COND", "type": "nominal"},

      "tooltip": [
        {"field": "INSPECT_DT", "title": "Inspect Date"},
        {"field": "STREET", "title": "Street"},
        {"field":"LOCTYPE", "title": "Location"},
        {"field":"TREETYPE", "title": "Tree Type"},
        {"field": "COND", "title": "State"}
      ]
    }
  }]

}

vegaEmbed('#vis', yourVlSpec);
    </script>
  </body>
</html>
