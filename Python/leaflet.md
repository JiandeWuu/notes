# leaflet (javascript)

An open-source JavaScript library for mobile-friendly interactive maps

## strat

You need:

- Include Leaflet CSS file in the you html `<head>`:
  
    ```html
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.6.0/dist/leaflet.css" integrity="sha512-xwE/Az9zrjBIphAcBb3F6JVqxf46+CDLwfLMHloNu6KEQCAWi6HcDUbeOfBIptF7tcCzusKFjFw2yuvEpDL9wQ==" crossorigin=""/>
    ```

- Include Leaflet JavaScript file **after** Leaflet's CSS:

    ```html
    <!-- Make sure you put this AFTER Leaflet's CSS -->
    <script src="https://unpkg.com/leaflet@1.6.0/dist/leaflet.js" integrity="sha512-gZwIG9x3wUXg2hdXF6+rVkLF/0Vi9U8D2Ntg4Ga5I5BZpVkVxlJWbSQtXPSiUTtC0TjtGOmxa1AJPuV0CPthew==" crossorigin=""></script>
    ```

- Put map `div` where you want your map to be:

    ```html
    <div id="mapdivid"></div>
    ```

- You need setting map `div` in CSS:
  
    ```css
    #mapdivid{
        height: 180px;
    }
    ```

## Setting map

### setView

Setting map center and map view, `L.map(<map div id>).setView(<LatLng>, <view level>)`:

```javascript
var mymap = L.map('mapdivid').setView([51.505, -0.09], 13);
```

### Marker

Add marker in your map, `L.marker(<LatLng>).addTo(<your map>)`:

```javascript
var marker = L.marker([51.5, -0.09]).addTo(mymap);
```
