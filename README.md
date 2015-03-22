<!DOCTYPE html>

Boston | Boston
<link rel="shortcut icon" href="http://littlemousetravel.com/wp-content/uploads/2014/12/mouse47.png" />
<link rel="stylesheet" href="http://code.jquery.com/ui/1.11.1/themes/smoothness/jquery-ui.css">
<link rel="stylesheet" href="http://libs.cartocdn.com/cartodb.js/v3/themes/css/cartodb.css" />
<link rel="stylesheet" href="http://academy.cartodb.com/css/cdbui.css">

<style>
   html, body {
            height: 100%;
            padding: 0;
            margin: 0;
          }
          #map {
            position: absolute;
            z-index: 0;
            bottom: 0;
            top: 0;
            right: 0;
            left: 0;
          }

    #sublayer0 a:hover { 
      color: #ae1b21;
      font-weight: bold;
      }
    #sublayer1 a:hover { 
      color: #1baea8;
      font-weight: bold;
      }

    </style>
 
    <link rel="stylesheet" href="http://libs.cartocdn.com/cartodb.js/v3/themes/css/cartodb.css" />
  </head>
  <body>
   <div id="map"></div>


    <div class="cartodb-layer-selector-box" style="display: block; right: -50px; top: -51px">  

      <div class="cartodb-dropdown border vertical_bottom horizontal_right tick_right" style="width: 120px; top: 35.0000033378601px; left: -17.9999966621399px; display: block; opacity: 1;">
        <ul>
          <li id="sublayer0">        <a class="layer" href="#">Saturday</a> 
                      <a id="switch1" href="#switch" class="right switch enabled"><span class="handle"></span></a>      
                   </li>

          <li id="sublayer1">        <a class="layer" href="#">Sunday</a>        
                      <a id="switch2" href="#switch" class="right switch enabled"><span class="handle"></span></a>      
          </li>

        </ul>

      </div>
    </div>



<!-- include google maps library -->
<script type="text/javascript" src="http://www.maps.google.com/maps/api/js?sensor=false&v=3.12"></script>


<!-- include cartodb.js library -->
<script src="http://libs.cartocdn.com/cartodb.js/v3/3.12/cartodb.js"></script>

<script>
  var infowindow;
  var infowindowOpened = false;
  var sublayers;
  // js
  function main() {

    var stl = JSON.stringify( [
          {
            featureType: "road",
            elementType: "geometry",
            stylers: [
              { visibility: "on" }
            ]
          },
          {
            featureType: "road.highway",
            elementType: "geometry",
            stylers: [
              { lightness: 100 },
            ]
          },
          {
            featureType: "road",
            elementType: "labels",
            stylers: [
              { lightness: 20},
               {visibility: "off"  }

            ]
          },
          {
            featureType: "road.highway",
            elementType: "labels",
            stylers: [
              { visibility: "off"  }
            ]
          },

          {
            featureType: "poi.business",
            stylers: [
              { visibility: "off" }
            ]
          },
          {
            featureType: "poi.government",
            stylers: [
              { visibility: "off" }
            ]
          },
          {
            featureType: "poi.attraction",
            stylers: [
              { visibility: "off" }
            ]
          },
          {
            featureType: "poi.medical",
            stylers: [
              { visibility: "off" }
            ]
          },
          {
            featureType: "poi.place_of_worship",
            stylers: [
              { visibility: "off" }
            ]
          },
          {
            featureType: "poi.sports_complex",
            stylers: [
              { visibility: "off" }
            ]
          },
          {
            featureType: "poi.school",
            stylers: [
              { visibility: "off" }
            ]
          },
          {
            featureType: "poi.park",
            elementType: "labels.icon",
            stylers: [
              { visibility: "off" }
            ]
          },
           {
            featureType: "landscape",
            elementType: "labels.icon",
            stylers: [
              { visibility: "off" }
            ]
          },
          {
            featureType: "administrative",
            elementType: "labels.icon",
            stylers: [
              { visibility: "off" }
            ]
          },
          {
            featureType: "transit.station",
            stylers: [
              {visibility: "off" }
            ]
          },
          {
            featureType: "poi",
            elementType: "labels.icon",
            stylers: [
              { visibility: "off" }
            ]
          },
          {
            featureType: "road.local",
            elementType: "labels",
            stylers: [
              { visibility: "on" }
            ]
          }
        ]);

    cartodb.createVis('map', 
      'http://evalugo21.cartodb.com/api/v2/viz/100ff358-bbe8-11e4-ab27-0e9d821ea90d/viz.json', 
      {
        shareable: false,
        description: false,
        title: true,
        search: false,
        center_lat: 42.358,
        center_lon: -71.07,
        zoom: 14,
        cartodb_logo: false,
        mobile_layout: true,
        legends: false,
        tooltip: false,
        gmaps_base_type: 'roadmap',
        gmaps_style: stl
    })

    //Close infowindows when clicking on basemap


    .done(function(vis, layers) {
      // layer 0 is the base layer, layer 1 is cartodb layer
      var layer = layers[1];
      infowindow = layer.infowindow;
      infowindowOpened = false;

      $('#map').click(closeInfowindow);

      layer.on('featureClick',function(e, latlng, pos, data, subLayerIndex) {
        infowindowOpened = true;
      });

    })
    .error(function(err) {
      console.log(err);
    });
  }

  function closeInfowindow() {
    setTimeout(function() {
      if (!infowindowOpened) {
        infowindow.set('visibility', false);
      }
      infowindowOpened = false;
    }, 250);
  }

  //Layer selector part

 var layerSource = 'http://evalugo21.cartodb.com/api/v2/viz/100ff358-bbe8-11e4-ab27-0e9d821ea90d/viz.json';


            // For storing the sublayers
          var sublayers = [];

          // Add data layer to your map
          cartodb.createLayer(map,layerSource, cdbOptions)
              .addTo(map)
              .done(function(layer) {
                 //console.log(layer);   
                 var num_sublayers = layer.getSubLayerCount();
                  
                 for (var i = 0; i < num_sublayers; i++) {
                    sublayers[i] = layer.getSubLayer(i);
                    //console.log(sublayers[i]); 
                  } 

   var sublayer0Shown = true;
              $("#sublayer0").on('click', function() {
              if (sublayer0Shown) {
                  sublayers[0].hide();
                  sublayers[1].hide();
                  $("#switch1").removeClass('right switch enabled');
                  $("#switch1").addClass('right switch disabled');

                } else {
                  sublayers[0].show();
                  sublayers[1].show();
                  $("#switch1").removeClass('right switch disabled');
                  $("#switch1").addClass('right switch enabled');
                }
                sublayer0Shown = !sublayer0Shown; 
              });

            var sublayer1Shown = true;
            $("#sublayer1").on('click', function() {
                if (sublayer1Shown) {
                  sublayers[2].hide();
                  sublayers[3].hide();
                  $("#switch2").removeClass('right switch enabled');
                  $("#switch2").addClass('right switch disabled');
                } else {
                  sublayers[2].show();
                  sublayers[3].show();
                  $("#switch2").removeClass('right switch disabled');
                  $("#switch2").addClass('right switch enabled');
                }
                sublayer1Shown = !sublayer1Shown; 
            }).error(function(err) {
              console.log("error: " + err);
            });

  window.onload = main;
  // end
</script>
</body>
</html>
