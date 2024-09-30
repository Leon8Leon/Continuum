<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
  	<meta name="viewport" content="width=device-width, initial-scale=1.0">
  	<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
 	<script src="https://unpkg.com/leaflet/dist/leaflet.js"></script>
    <script src="https://unpkg.com/leaflet-gpx/dist/leaflet-gpx.min.js"></script>
  	<script src="https://cdnjs.cloudflare.com/ajax/libs/leaflet-gpx/1.6.0/gpx.min.js"></script>
  	<script src="https://unpkg.com/leaflet-omnivore@0.4.1/dist/leaflet-omnivore.min.js"></script>
  	<script src="https://cdn.jsdelivr.net/npm/gpxparser/dist/gpxparser.min.js"></script>
  
    <title>Mapa z markerami</title>
    <style>
        #map {
            height: 1250px;
            width: 1750px;
        }
      
 		.custom-text {
            display: none; /* Domyślnie ukryte */
            background-color: white;
            padding: 5px;
            border: 1px solid black;
            font-size: 18px;
          
        }  
        .leaflet-tooltip {
            background: gray ; /* Tło tooltipa ustawione na przezroczyste */
            border: none !important; /* Bez obramowania */
            box-shadow: gray ; /* Bez cienia */
            font-size: 14px; /* Rozmiar czcionki */
            color: #000; /* Kolor tekstu */
            padding: 0; /* Brak paddingu */
            margin: 0; /* Brak marginesów */
            white-space: nowrap; /* Zapobiega zawijaniu tekstu */
        }
        .leaflet-tooltip:after {
            display: none; /* Usunięcie strzałek w tooltipie */
        }
        .text-icon-left {
            background: transparent; /* Przezroczyste tło */
            border: none; /* Brak obramowania */
            box-shadow: none; /* Brak cienia */
            font-size: 14px; /* Rozmiar czcionki */
            color: #000; /* Kolor tekstu */
            padding: 0; /* Brak paddingu */
            margin: 0; /* Brak marginesów */
            text-align: left; /* Wyrównanie tekstu do lewej */
            white-space: nowrap; /* Zapobiega zawijaniu tekstu */
        }
        .text-icon-right {
            background: transparent; /* Przezroczyste tło */
            border: none; /* Brak obramowania */
            box-shadow: none; /* Brak cienia */
            font-size: 18px; /* Rozmiar czcionki */
            color: #000; /* Kolor tekstu */
            padding: 0; /* Brak paddingu */
            margin: 0; /* Brak marginesów */
            text-align: right; /* Wyrównanie tekstu do prawej */
            white-space: nowrap; /* Zapobiega zawijaniu tekstu */
        }
        .text-icon-rotate {
             background: transparent; /* Przezroczyste tło */
   			 border: none; /* Brak obramowania */
    			box-shadow: none; /* Brak cienia */
    			font-size: 14px; /* Rozmiar czcionki */
    			color: #000; /* Kolor tekstu */
    			padding: 0; /* Brak paddingu */
    			margin: 0; /* Brak marginesów */
    			display: inline-block; /* Ważne dla poprawnego działania transform */
           		transform: rotate(-45deg) !important; /* Obrót o -45 stopni */
            	transform-origin: center; /* Ustaw punkt obrotu na środek */
    			
        }
      
      .rotated-text {
            display: inline-block !important;
            transform: rotate(45deg) !important; /* Rotacja tekstu o 45 stopni */
            transform-origin: center center !important; /* Punkt obrotu na środku */
            font-size: 25px !important; /* Rozmiar tekstu */
            color: red !important; /* Kolor tekstu */
            white-space: nowrap !important; /* Zapobiega zawijaniu tekstu */
        	 
        }
      
 
      
    </style>
  
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
    <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
</head>
<body>
  
  <h1>Wybierz lata:</h1>
<div class="menu">
    <label><input type="checkbox" id="year2023" value="2023" checked> Rok 2023</label><br>
    <label><input type="checkbox" id="year2024" value="2024" checked> Rok 2024</label><br>
    <label><input type="checkbox" id="year2023se" value="2023se" checked> Rok 2023se</label><br>
  <div id="map"></div>
    <input type="checkbox" id="toggleOpacity" />
    <label for="toggleOpacity">Zmień przezroczystość</label>
  
    <div id="map"></div>

    <script>
        // Inicjalizacja mapy
        var map = L.map('map').setView([46.5, 8], 7); // Ustawienie widoku mapy
      	// Dodanie skali do mapy
      	L.control.scale({
        imperial: false, // Wyłączamy mile
        metric: true,    // Włączamy kilometry
        position: 'bottomleft' // Umieszczamy skalę w lewym dolnym rogu
    	}).addTo(map);

        // Wstawienie warstwy OpenStreetMap
        L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
            maxZoom: 18,
            attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
        }).addTo(map);

      
      
       // Funkcja do tworzenia ikony SVG
    function createSvgIcon2(svgUrl) {
        return L.divIcon({
            className: 'svg-icon',
            html: `<img src="${svgUrl}" class="svg-icon" />`,
            iconSize: [20, 20], // Ustawienie wielkości ikony
            iconAnchor: [10, 10] // Punkt zakotwiczenia ikony
        });
     }
        // Funkcja do tworzenia ikony SVG
    function createSvgIcon3(svgUrl) {
        return L.divIcon({
            className: 'svg-icon',
            html: `<img src="${svgUrl}" class="svg-icon" />`,
            iconSize: [24, 24], // Ustawienie wielkości ikony
            iconAnchor: [12, 12] // Punkt zakotwiczenia ikony
        
        });
      }
          // Funkcja do tworzenia ikony SVG
    function createSvgIcon4(svgUrl) {
        return L.divIcon({
            className: 'svg-icon',
            html: `<img src="${svgUrl}" class="svg-icon" />`,
            iconSize: [32, 32], // Ustawienie wielkości ikony
            iconAnchor: [16, 16] // Punkt zakotwiczenia ikony
        
        });
    }

    // Ścieżki do SVG emoji z OpenMoji
        var upsideDownTriangleEmoji = "https://raw.githubusercontent.com/hfg-gmuend/openmoji/master/color/svg/1F53B.svg"; // 🔻
        var triangleEmoji = "https://raw.githubusercontent.com/hfg-gmuend/openmoji/master/color/svg/1F53A.svg"; // 🔺
      	var RedDiamondemoji = "https://raw.githubusercontent.com/hfg-gmuend/openmoji/master/color/svg/1F538.svg"; // 🔸
      	var blueDiamondemoji = "https://raw.githubusercontent.com/hfg-gmuend/openmoji/master/color/svg/1F539.svg"; // 🔹
      	var POLflagemoji = "https://raw.githubusercontent.com/hfg-gmuend/openmoji/master/color/svg/1F1F5-1F1F1.svg"; // 🇵🇱 Polska
		var GERflagemoji = "https://raw.githubusercontent.com/hfg-gmuend/openmoji/master/color/svg/1F1E9-1F1EA.svg"; // 🇩🇪 Niemcy
		var CZEflagemoji = "https://raw.githubusercontent.com/hfg-gmuend/openmoji/master/color/svg/1F1E8-1F1FF.svg"; // 🇨🇿 Czechy
		var SUIflagemoji = "https://raw.githubusercontent.com/hfg-gmuend/openmoji/master/color/svg/1F1E8-1F1ED.svg"; // 🇨🇭 Szwajcaria
		var ITAflagemoji = "https://raw.githubusercontent.com/hfg-gmuend/openmoji/master/color/svg/1F1EE-1F1F9.svg"; // 🇮🇹 Włochy
		var FRAflagemoji = "https://raw.githubusercontent.com/hfg-gmuend/openmoji/master/color/svg/1F1EB-1F1F7.svg"; // 🇫🇷 Francja
		var AUTflagemoji = "https://raw.githubusercontent.com/hfg-gmuend/openmoji/master/color/svg/1F1E6-1F1F9.svg"; // 🇦🇹 Austria
      	var svgGreenDiamondurl = "https://drive.google.com/uc?id=1Khr7WKJ6bybSsTP_sidjhuGHx0ItGkbd"; // Zielony Diament
        var svgUYellowDiamondurl = "https://drive.google.com/uc?id=1KhXUodObPkxZAUr4nQkcklbyBRZe0sxJ"; // Żółty Diament
     	var eight_spoked_asteriskemoji = "https://raw.githubusercontent.com/hfg-gmuend/openmoji/master/color/svg/2734.svg"; // ✳️
      	var eight_pointed_black_staremoji = "https://raw.githubusercontent.com/hfg-gmuend/openmoji/master/color/svg/2736.svg"; // ✴️


        // Ikony markerów z SVG emoji
        var upsideDownTriangleSvg = createSvgIcon2(upsideDownTriangleEmoji);
        var triangleSvg = createSvgIcon2(triangleEmoji);
      	var reddiamondSvg = createSvgIcon3(RedDiamondemoji);
      	var bluediamondSvg = createSvgIcon3(blueDiamondemoji);
      	var POLflagSVG = createSvgIcon4(POLflagemoji);
		var GERflagSVG = createSvgIcon4(GERflagemoji);
		var CZEflagSVG = createSvgIcon4(CZEflagemoji);
		var SUIflagSVG = createSvgIcon4(SUIflagemoji);
		var ITAflagSVG = createSvgIcon4(ITAflagemoji);
		var FRAflagSVG = createSvgIcon4(FRAflagemoji);
		var AUTflagSVG = createSvgIcon4(AUTflagemoji);
      	var svgUGreenDiamond = createSvgIcon3(svgGreenDiamondurl);
		var svgUYellowDiamond = createSvgIcon3(svgUYellowDiamondurl);
      	var eight_spoked_asterisk = createSvgIcon2(eight_spoked_asteriskemoji);
 		var eight_pointed_black_star = createSvgIcon2(eight_pointed_black_staremoji);
 
      
      
        // Ikony markerów
      
      	var blueDiamondIcon = L.icon({
            iconUrl: 'https://upload.wikimedia.org/wikipedia/commons/6/6f/Bright-blue_pog.svg',
            iconSize: [12, 12],
            iconAnchor: [6, 6],
        });
      
      var blueDiamondIconEMOJI1 = L.icon({
            iconUrl: 'https://upload.wikimedia.org/wikipedia/commons/e/e8/Emoji_u1f537.svg',
            iconSize: [15, 15],
            iconAnchor: [7, 7],
        });
      
      var blueDiamondIconEMOJI2 = L.icon({
            iconUrl: 'https://upload.wikimedia.org/wikipedia/commons/8/82/Fluent_Emoji_high_contrast_1f537.svg',
             iconSize: [15, 15],
            iconAnchor: [7, 7],
        });
     
      	var blueDiamondIcon10 = L.icon({
            iconUrl: 'https://upload.wikimedia.org/wikipedia/commons/6/6f/Bright-blue_pog.svg',
            iconSize: [10, 10],
            iconAnchor: [5, 5],
        });
      
      
     	var redDotIconSmall = L.icon({
            iconUrl: 'https://upload.wikimedia.org/wikipedia/commons/0/0c/Red_pog.svg',
            iconSize: [8, 8],
            iconAnchor: [4, 4],
        });
      
      	var redDotIcon10 = L.icon({
            iconUrl: 'https://upload.wikimedia.org/wikipedia/commons/0/0c/Red_pog.svg',
            iconSize: [10, 10],
            iconAnchor: [5, 5],
        });
      
        var redDotIcon = L.icon({
            iconUrl: 'https://upload.wikimedia.org/wikipedia/commons/0/0c/Red_pog.svg',
            iconSize: [12, 12],
            iconAnchor: [6, 6],
        });
      
       	var redDotIconsurplus = L.icon({
            iconUrl: 'https://upload.wikimedia.org/wikipedia/commons/0/0c/Red_pog.svg',
            iconSize: [16, 16],
            iconAnchor: [8, 8],
        });
      
     	var redDotIconMedium = L.icon({
            iconUrl: 'https://upload.wikimedia.org/wikipedia/commons/0/0c/Red_pog.svg',
            iconSize: [20, 20],
            iconAnchor: [10, 10],
        });

        var redSquareIcon = L.icon({
            iconUrl: 'https://upload.wikimedia.org/wikipedia/commons/0/0c/Red_pog.svg',
            iconSize: [24, 24],
            iconAnchor: [12, 12],
        });
      
        var redSquareIcon30 = L.icon({
            iconUrl: 'https://upload.wikimedia.org/wikipedia/commons/0/0c/Red_pog.svg',
            iconSize: [28, 28],
            iconAnchor: [14, 14],
        });
      
		var goldDotIcon = L.icon({
            iconUrl: ' https://upload.wikimedia.org/wikipedia/commons/c/cc/Gold_pog.svg',
            iconSize: [10, 10],
            iconAnchor: [5, 5],
        });
      
      	var darkgreenDotIcon = L.icon({
            iconUrl: 'https://upload.wikimedia.org/wikipedia/commons/7/7a/Dark_Green_004040_pog.svg',
            iconSize: [10, 10],
            iconAnchor: [5, 5],
        });
      
        var greenDotIcon = L.icon({
            iconUrl: 'https://upload.wikimedia.org/wikipedia/commons/8/8b/Green_008000_pog.svg',
            iconSize: [10, 10],
            iconAnchor: [5, 5],
        });
      
      	var greenDotIcon12 = L.icon({
            iconUrl: 'https://upload.wikimedia.org/wikipedia/commons/8/8b/Green_008000_pog.svg',
            iconSize: [12, 12],
            iconAnchor: [6, 6],
 		});
      
       
        var blackTriangleIcon = L.icon({
            iconUrl: 'https://upload.wikimedia.org/wikipedia/commons/8/80/OpenMoji-color_1F53A.svg',
            iconSize: [18, 18],
            iconAnchor: [9, 9],
        });

		var upsideDownBlackTriangleIcon = L.icon({
            iconUrl: 'https://upload.wikimedia.org/wikipedia/commons/0/04/Nuvola_apps_important_cover_upside_down.svg',
            iconSize: [12, 12],
            iconAnchor: [6, 6],
          
        });
      
        var steelPogeIcon = L.icon({
            iconUrl: 'https://upload.wikimedia.org/wikipedia/commons/7/71/Steel_pog.svg',
            iconSize: [12, 12],
            iconAnchor: [6, 6],

        });
      
       	var Black_square_45degree_angle = L.icon({
            iconUrl: 'https://upload.wikimedia.org/wikipedia/commons/7/70/Black_square_45degree_angle.svg',
            iconSize: [12, 12],
            iconAnchor: [6, 6],
        });
      
/* 
      function drawCirclePOZNAN(map, center, radius = 100000, color = '#008B8B', weight = 3, opacity = 0.8) {
    L.circle(center, {   // przekazujemy tablicę `center`
        color: color,      // Kolor krawędzi okręgu
        weight: weight,    // Grubość linii krawędzi
        opacity: opacity,  // Przezroczystość linii
        fillOpacity: 0.0,  // Przezroczystość wypełnienia (jeśli chcesz mieć pusty środek)
        radius: radius     // Promień w metrach
    }).addTo(map);
}

// Przykładowe współrzędne środka okręgu
var centerPointPOZNAN = [52.40163, 16.91163];

// Rysowanie okręgu na mapie o promieniu 100 km
drawCirclePOZNAN(map, centerPointPOZNAN, 50000);
*/
      var lineOpacity = 0.8; // Przezroczystość linii
      
        // Definiowanie współrzędnych prostokąta
        var boundsIT2024 = [[44.0, 6.0], [48.0, 11.0]];
        // Dodanie prostokąta Włochy 2024
        var rectangleIT2024 = L.rectangle(boundsIT2024, {
            color: '#B8860B', // Kolor ciemnego złota
            weight: 3, // Waga linii
            opacity: 0.8, // Przezroczystość linii
            fillOpacity: 0.0 // Przezroczystość wypełnienia
        }).addTo(map);
      
      // Definiowanie współrzędnych prostokąta Czeska Szwajcaria
        var boundsCZSA2023 = [[50.568, 13.653], [51.126, 14.669]];
      // Dodanie prostokąta
        var rectangleCZSA2023 = L.rectangle(boundsCZSA2023, {
            color: '#4682B4', // Kolor ciemnego złota
            weight: 3, // Waga linii
            opacity: 0.8, // Przezroczystość linii
            fillOpacity: 0.0 // Przezroczystość wypełnienia
        }).addTo(map);
      
      
       // Definiowanie współrzędnych prostokąta Nowogród Bobrzański
        var boundsNB24 = [[51.191, 15.032], [52.064, 15.908]];
      // Dodanie prostokąta
        var rectangleNB24 = L.rectangle(boundsNB24, {
            color: '#4682B4', // Kolor ciemnego złota
            weight: 3, // Waga linii
            opacity: 0.8, // Przezroczystość linii
            fillOpacity: 0.0 // Przezroczystość wypełnienia
        }).addTo(map);
      
      
        // Definiowanie współrzędnych prostokąta Sokołów Podlaski
        var boundsSOK2324 = [[52.090, 21.771], [52.488, 22.475]];
      // Dodanie prostokąta
        var rectangleSOK2324 = L.rectangle(boundsSOK2324, {
            color: '#4682B4', // Kolor ciemnego złota
            weight: 3, // Waga linii
            opacity: 0.8, // Przezroczystość linii
            fillOpacity: 0.0 // Przezroczystość wypełnienia
        }).addTo(map);
      // Definiowanie współrzędnych linii Sokołów-Łódź a
		var lineCoords = [[52.090, 21.771], [52.090, 19.44606]];
		// Dodanie linii
		var linesoklodz24a = L.polyline(lineCoords, {
    	color: '#4682B4', // KOLOR
    	weight: 3, // waga linii
    	opacity: 0.8 // przezroczystość
      	}).addTo(map);
      // Definiowanie współrzędnych linii Sokołów-Łódź b
		var lineCoords = [[51.77024, 19.44606], [52.090, 19.44606]];
		// Dodanie linii
		var linesoklodz24b = L.polyline(lineCoords, {
    	color: '#4682B4', // KOLOR
    	weight: 3, // waga linii
    	opacity: 0.8 // przezroczystość
      	}).addTo(map);
      // Definiowanie współrzędnych linii Sokołów-Warszawa b
		var lineCoords = [[52.22977, 21.01178], [52.090, 21.01178]];
		// Dodanie linii
		var linesokwaw24b = L.polyline(lineCoords, {
    	color: '#4682B4', // KOLOR
    	weight: 3, // waga linii
    	opacity: 0.8 // przezroczystość
      	}).addTo(map);
      // Definiowanie współrzędnych linii Sokołów-Białystok a
		var lineCoords = [[52.488, 22.475], [53.131021, 22.475]];
		// Dodanie linii
		var linesokb24a = L.polyline(lineCoords, {
    	color: '#4682B4', // KOLOR
    	weight: 3, // waga linii
    	opacity: 0.8 // przezroczystość
      	}).addTo(map);
      // Definiowanie współrzędnych linii Sokołów-Białystok b
		var lineCoords = [[53.131021, 23.155264], [53.131021, 22.475]];
		// Dodanie linii
		var linesokb242 = L.polyline(lineCoords, {
    	color: '#4682B4', // KOLOR
    	weight: 3, // waga linii
    	opacity: 0.8 // przezroczystość
      	}).addTo(map);
          // Definiowanie współrzędnych linii Sokołów-Lublin a
		var lineCoords = [[52.090, 22.475], [51.24645, 22.475]];
		// Dodanie linii
		var linesoklub24a = L.polyline(lineCoords, {
    	color: '#4682B4', // KOLOR
    	weight: 3, // waga linii
    	opacity: 0.8 // przezroczystość
      	}).addTo(map);
      // Definiowanie współrzędnych linii Sokołów-Lublin b
		var lineCoords = [[51.24645, 22.56844], [51.24645, 22.475]];
		// Dodanie linii
		var linesoklub24b = L.polyline(lineCoords, {
    	color: '#4682B4', // KOLOR
    	weight: 3, // waga linii
    	opacity: 0.8 // przezroczystość
      	}).addTo(map);
      
      
      // Definiowanie współrzędnych prostokąta Praga
        var boundsPrag23 = [[49.820, 14.043], [50.345, 14.969]];
      // Dodanie prostokąta
        var rectanglePrag23 = L.rectangle(boundsPrag23, {
            color: '#4682B4', // Kolor ciemnego złota
            weight: 3, // Waga linii
            opacity: 0.8, // Przezroczystość linii
            fillOpacity: 0.0 // Przezroczystość wypełnienia
        }).addTo(map);
       // Definiowanie współrzędnych prostokąta Kotlina Kłodzka
        var boundsKK23 = [[50.3507, 16.3628], [50.4636, 16.5797]];
      // Dodanie prostokąta
        var rectangleKK23 = L.rectangle(boundsKK23, {
            color: '#4682B4', // Kolor ciemnego złota
            weight: 3, // Waga linii
            opacity: 0.8, // Przezroczystość linii
            fillOpacity: 0.0 // Przezroczystość wypełnienia
        }).addTo(map);
     	 // Definiowanie współrzędnych linii KŁODZKO+pRAGA
		var lineCoords = [[50.345, 14.969], [50.3507, 16.3628]];
		// Dodanie linii
		var linePRAGAKLODZKO = L.polyline(lineCoords, {
    	color: '#4682B4', // KOLOR
    	weight: 3, // waga linii
    	opacity: 0.8 // przezroczystość
      	}).addTo(map);
      // Definiowanie współrzędnych linii KŁODZKO+jAWOR1
		var lineCoords = [[50.4636, 16.5797], [51.05349, 16.5797]];
		// Dodanie linii
		var linePRAGAKLODZKO2 = L.polyline(lineCoords, {
    	color: '#4682B4', // KOLOR
    	weight: 3, // waga linii
    	opacity: 0.8 // przezroczystość
      	}).addTo(map);
      // Definiowanie współrzędnych linii KŁODZKO+jAWOR2
		var lineCoords = [[51.05349, 16.18947], [51.05349, 16.5797]];
		// Dodanie linii
		var linePRAGAKLODZKO3 = L.polyline(lineCoords, {
    	color: '#4682B4', // KOLOR
    	weight: 3, // waga linii
    	opacity: 0.8 // przezroczystość
      	}).addTo(map);
      
      // Definiowanie współrzędnych prostokąta Wyspa RUGIA
        var boundsRUG23 = [[54.188, 12.876], [54.578, 13.859]];
      // Dodanie prostokąta
        var rectangleRUG23 = L.rectangle(boundsRUG23, {
            color: '#4682B4', // Kolor ciemnego złota
            weight: 3, // Waga linii
            opacity: 0.8, // Przezroczystość linii
            fillOpacity: 0.0 // Przezroczystość wypełnienia
        }).addTo(map);
      // Definiowanie współrzędnych linii RugiaSchwerin
		var lineCoords = [[54.188, 12.876], [54.188, 11.41235]];
		// Dodanie linii
		var linerug231 = L.polyline(lineCoords, {
    	color: '#4682B4', // KOLOR
    	weight: 3, // waga linii
    	opacity: 0.8 // przezroczystość
      	}).addTo(map);
      // Definiowanie współrzędnych linii RugiaSchwerin2
		var lineCoords = [[53.62358, 11.41235], [54.188, 11.41235]];
		// Dodanie linii
		var linerug232 = L.polyline(lineCoords, {
    	color: '#4682B4', // KOLOR
    	weight: 3, // waga linii
    	opacity: 0.8 // przezroczystość
      	}).addTo(map);
 
      // Definiowanie współrzędnych prostokąta Mrzeżyno
        var boundsmrz24 = [[54.016, 15.123], [54.320, 15.737]];
      // Dodanie prostokąta
        var rectanglemrz24 = L.rectangle(boundsmrz24, {
            color: '#4682B4', // Kolor ciemnego złota
            weight: 3, // Waga linii
            opacity: 0.8, // Przezroczystość linii
            fillOpacity: 0.0 // Przezroczystość wypełnienia
        }).addTo(map);
      
      // Definiowanie współrzędnych prostokąta POZNAŃ ROWEROWY OBSZAR DZIAŁALNOŚCI
        var bounds = [[51.416, 15.331], [53.298, 18.880]];
      // Dodanie prostokąta POZNAŃ ROWEROWY OBSZAR DZIAŁALNOŚCI
        var rectanglePoznańrower24 = L.rectangle(bounds, {
            color: '#008080', // Kolor TEAL
            weight: 3, // Waga linii
            opacity: 0.7, // Przezroczystość linii
            fillOpacity: 0.0 // Przezroczystość wypełnienia
        }).addTo(map);
      
      
      // Definiowanie współrzędnych prostokąta IZERY I KARKONOSZE
      var rectangleik23 = L.rectangle([
    [50.6434, 15.2504], // Lewy dolny róg
    [50.9368, 15.9714]  // Prawy górny róg
], {
    color: '#4682B4',   // Kolor turkusowy
    weight: 3,          // Waga linii
    opacity: 0.8,       // Przezroczystość linii
    fillOpacity: 0.0    // Przezroczystość wypełnienia
}).addTo(map);
      // Definiowanie współrzędnych linii Wrocław Karkonosze 1
		var lineCoords = [[51.1079, 17.0385], [50.9368, 17.0385]];
		// Dodanie linii
		var linewrockark231 = L.polyline(lineCoords, {
    	color: '#4682B4', // KOLOR
    	weight: 3, // waga linii
    	opacity: 0.8 // przezroczystość
      	}).addTo(map);
   // Definiowanie współrzędnych linii Wrocław Karkonosze 2
		var lineCoords = [[50.9368, 15.9714], [50.9368, 17.0385]];
		// Dodanie linii
		var linewrockar232 = L.polyline(lineCoords, {
    	color: '#4682B4', // KOLOR
    	weight: 3, // waga linii
    	opacity: 0.8 // przezroczystość
      	}).addTo(map);

       // Definiowanie współrzędnych prostokąta TATRY
      var rectangletatry23 = L.rectangle([
    [49.0190, 19.5982], // Lewy dolny róg
    [49.3985, 20.3123]  // Prawy górny róg
], {
    color: '#4682B4',   // Kolor turkusowy
    weight: 3,          // Waga linii
    opacity: 0.8,       // Przezroczystość linii
    fillOpacity: 0.0    // Przezroczystość wypełnienia
}).addTo(map);

      
      
      // Tworzenie trapezu bezpośrednio w L.polygon 2023SE Pystynie
		var trapezpustynie23 = L.polygon([
    [41.00, -120],  // lewy górny wierzchołek
    [41.00, -103.5],  // prawy górny wierzchołek
    [32.75, -103.5],  // prawy dolny wierzchołek
    [32.75, -120]   // lewy dolny wierzchołek
], {
    color: '#B8860B',   // KOLOR taki jak w linii
    weight: 3,          // Grubość linii
    opacity: 0.8,       // Przezroczystość linii
    fillColor: '#4682B4', // Kolor wypełnienia taki sam jak linia
    fillOpacity: 0.0    // Przezroczystość wypełnienia
}).addTo(map);
      
         // Tworzenie trapezu bezpośrednio w L.polygon 2023SE Memphis
		var trapezmemphis23 = L.polygon([
    [35.604, -90.538],  // lewy górny wierzchołek
    [35.604, -89.253],  // prawy górny wierzchołek
    [34.775, -89.253],  // prawy dolny wierzchołek
    [34.775, -90.538]   // lewy dolny wierzchołek
], {
    color: '#B8860B',   // KOLOR taki jak w linii
    weight: 3,          // Grubość linii
    opacity: 0.8,       // Przezroczystość linii
    fillColor: '#4682B4', // Kolor wypełnienia taki sam jak linia
    fillOpacity: 0.0    // Przezroczystość wypełnienia
}).addTo(map);

      // Tworzenie trapezu bezpośrednio w L.polygon 2023SE Nowy Jork
		var trapeznowyjork23 = L.polygon([
    [40.901, -74.303],  // lewy górny wierzchołek
    [40.901, -73.680],  // prawy górny wierzchołek
    [40.516, -73.680],  // prawy dolny wierzchołek
    [40.516, -74.303]   // lewy dolny wierzchołek
], {
    color: '#B8860B',   // KOLOR taki jak w linii
    weight: 3,          // Grubość linii
    opacity: 0.8,       // Przezroczystość linii
    fillColor: '#4682B4', // Kolor wypełnienia taki sam jak linia
    fillOpacity: 0.0    // Przezroczystość wypełnienia
}).addTo(map);
       // Definiowanie współrzędnych linii Denver Memphis 1
		var lineCoords = [[32.75, -103.5], [32.75, -90.538]];
		// Dodanie linii
		var linedenmem231 = L.polyline(lineCoords, {
    	color: '#B8860B', // KOLOR
    	weight: 3, // waga linii
    	opacity: 0.8 // przezroczystość
      	}).addTo(map);
      // Definiowanie współrzędnych linii Denver Memphis 2
		var lineCoords = [[32.75, -90.538], [34.775, -90.538]];
		// Dodanie linii
		var linedenmem232 = L.polyline(lineCoords, {
    	color: '#B8860B', // KOLOR
    	weight: 3, // waga linii
    	opacity: 0.8 // przezroczystość
      	}).addTo(map);
      
       // Definiowanie współrzędnych linii Nowy Jork Memphis 1
		var lineCoords = [[35.604, -89.253], [35.604, -74.303]];
		// Dodanie linii
		var linenymem231 = L.polyline(lineCoords, {
    	color: '#B8860B', // KOLOR
    	weight: 3, // waga linii
    	opacity: 0.8 // przezroczystość
      	}).addTo(map);
      // Definiowanie współrzędnych linii Nowy Jork Memphis 2
		var lineCoords = [[35.604, -74.303], [40.516, -74.303]];
		// Dodanie linii
		var linenymem232 = L.polyline(lineCoords, {
    	color: '#B8860B', // KOLOR
    	weight: 3, // waga linii
    	opacity: 0.8 // przezroczystość
      	}).addTo(map);

      
        // Funkcja do tworzenia niestandardowych markerów z tekstem
        function createTextIcon(text) {
            return L.divIcon({
                className: 'text-icon', // Klasa CSS
                html: text, // Tekst markeru
                iconSize: [100, 30] // Rozmiar ikony (można dostosować)
            });
          
          
      
             // Mapowanie nazw ikon na ich definicje
var iconStyles = {
    blueDiamondIcon: blueDiamondIcon, 
    blueDiamondIconEMOJI1: blueDiamondIconEMOJI1,  // Wodospad, Tama, Most
    blueDiamondIconEMOJI2: blueDiamondIconEMOJI2, // Uzupełenienie
    blueDiamondIcon10: blueDiamondIcon10,
    redDotIconSmall: redDotIconSmall, // Zaliczone Miasto lub sam nocleg bez zwiedzania, przejazd samochodem bez zatrzymania nie liczy się
    redDotIcon10: redDotIcon10, // Miasto małe - w którym zwiedzono coś znaczącego ale punktowo - w wykazie zwykle miasto małe ale wspomniane czasami nie-wspomniane
    redDotIcon: redDotIcon, // Miasto, które zwiedzono i o którym wspomniano
    redDotIconsurplus: redDotIconsurplus, // Miasto sporej wielkości ale bez oznakowania dwóch gwiazdek - np Zielona Góra, Częstochowa, Gdynia itd
    redDotIconMedium: redDotIconMedium, // Miasto duże szacunkowo 400-1250tys. Mieszk. 
    redSquareIcon: redSquareIcon, // Miasto Wielkie szac 1500-4500tys. Mieszk.
    redSquareIcon30: redSquareIcon30, // Megametropolia powyżej 4500tys. Mieszk.
    goldDotIcon: goldDotIcon, // BRAK
    darkgreenDotIcon: darkgreenDotIcon, // BRAK
    greenDotIcon: greenDotIcon, // Mały rezerwat lub ograniczony obszarowo Park
    greenDotIcon12: greenDotIcon12, // Duży rezerwat, Perk Narodowy, Rejon Rekreacyjny
    blackTriangleIcon: blackTriangleIcon, // Szczyt
    upsideDownBlackTriangleIcon: upsideDownBlackTriangleIcon, //Przełęcz, Depresja
    steelPogeIcon: steelPogeIcon, // BRAK
    Black_square_45degree_angle: Black_square_45degree_angle //BRAK
};

    
        }
        
   // Krok 2: Definicja markerów z latami
      
      
      
      
    var markersData = {
     '2023se': [
       // ROK 2023SE Duże miasta
    { lat: 48.13743, lng: 11.57549, name: "🇩🇪 Monachium", icon: redSquareIcon },     	// Monachium, Niemcy
    { lat: 52.2297, lng: 21.0122, name: "🇵🇱 Warszawa", icon: redSquareIcon },         // Warszawa, Polska
    { lat: 51.1079, lng: 17.0385, name: "🇵🇱 Wrocław", icon: redDotIconMedium },          // Wrocław, Polska
    { lat: 52.4064, lng: 16.9252, name: "🇵🇱 Poznań", icon: redDotIconMedium },           // Poznań, Polska
    { lat: 53.1235, lng: 18.0084, name: "🇵🇱 Bydgoszcz", icon: redDotIconMedium },        // Bydgoszcz, Polska
    { lat: 52.5341, lng: 17.5821, name: "🇵🇱 Gniezno", icon: redDotIcon },          // Gniezno, Polska
    { lat: 53.1517, lng: 16.7386, name: "🇵🇱 Piła", icon: redDotIcon },             // Piła, Polska
    { lat: 51.6547, lng: 17.8091, name: "🇵🇱 Ostrów Wlkp.", icon: redDotIcon },     // Ostrów Wlkp., Polska
    { lat: 52.2230, lng: 18.2555, name: "🇵🇱 Konin", icon: redDotIcon },            // Konin, Polska
    { lat: 50.7766, lng: 15.7557, name: "🇵🇱 Karpacz", icon: redDotIcon },          // Karpacz, Polska
    { lat: 50.8277, lng: 15.5143, name: "🇵🇱 Szklarska Poręba", icon: redDotIcon }, // Szklarska Poręba, Polska
    { lat: 52.6262, lng: 16.0786, name: "🇵🇱 Sieraków", icon: redDotIcon },         // Sieraków, Polska
    { lat: 52.7107, lng: 16.3711, name: "🇵🇱 Wronki", icon: redDotIcon },            // Wronki, Polska
    { lat: 52.40760, lng: 22.25163, name: "🇵🇱 Sokołów Podlaski", icon: redDotIcon }, // Sokołów Podlaski, Polska
       
       // Rok 2023se Mniejsze
	{ lat: 52.24530, lng: 16.84711, name: "🇵🇱 Mosina", icon: redDotIconSmall }, // Mosina, Polska
	{ lat: 52.29000, lng: 16.85895, name: "🇵🇱 Puszczykowo", icon: redDotIconSmall }, // Puszczykowo, Polska
    { lat: 52.34478, lng: 16.88178, name: "🇵🇱 Luboń", icon: redDotIconSmall }, // Luboń, Polska        
    { lat: 52.26311, lng: 16.80943, name: "🇵🇱 Wielkopolski Park Narodowy", icon: greenDotIcon12 }, // Wielkopolski Park Narodowy, Polska
	{ lat: 50.85061, lng: 15.42034, name: "🇵🇱 1126 🔺 Wysoka Kopa 👑", icon: triangleSvg },           // Wielka Kopa, Polska
	{ lat: 50.79129, lng: 15.51394, name: "🇵🇱 1362 🔺 Szrenica", icon: triangleSvg },              // Szrenica, Polska
	{ lat: 50.77864, lng: 15.55716, name: "🇵🇱 🇨🇿 1497🔺 Czarcia Ambona (Śnieżne Kotły)", icon: triangleSvg },        // Czarcia Ambona, Polska
	{ lat: 50.73569, lng: 15.73997, name: "🇵🇱 🇨🇿 1603🔺 Śnieżka 👑", icon: triangleSvg },               // Śnieżka, Polska
	{ lat: 50.75253, lng: 15.79156, name: "🇵🇱 1281 🔺 Skalny Stół", icon: triangleSvg },           // Skalny Stół, Polska
	{ lat: 52.47737, lng: 17.28742, name: "🇵🇱 Pobiedziska", icon: redDotIconSmall },                 // Pobiedziska, Polska
	{ lat: 52.41038, lng: 17.07747, name: "🇵🇱 Swarzędz", icon: redDotIconSmall },                    // Swarzędz, Polska
	{ lat: 52.33432, lng: 16.80655, name: "🇵🇱 Komorniki", icon: redDotIconSmall },                   // Komorniki, Polska
	{ lat: 52.24420, lng: 17.09089, name: "🇵🇱 Kórnik", icon: redDotIcon },                      // Kórnik, Polska
	{ lat: 52.09096, lng: 17.01692, name: "🇵🇱 Śrem", icon: redDotIcon10 },                        // Śrem, Polska
	{ lat: 51.986631, lng: 17.065330, name: "🇵🇱 Dolsk", icon: redDotIconSmall },                       // Dolsk, Polska
	{ lat: 51.87784, lng: 17.01696, name: "🇵🇱 Gostyń", icon: redDotIcon },                      // Gostyń, Polska		
	{ lat: 51.758497, lng: 17.122010, name: "🇵🇱 Pępowo", icon: redDotIconSmall },                      // Pępowo, Polska
	{ lat: 51.714898, lng: 17.226616, name: "🇵🇱 Kobylin", icon: redDotIconSmall },                     // Kobylin, Polska
	{ lat: 51.695684, lng: 17.437406, name: "🇵🇱 Krotoszyn", icon: redDotIcon },                   // Krotoszyn, Polska
	{ lat: 52.575763, lng: 17.009486, name: "🇵🇱 Murowana Goślina", icon: redDotIconSmall },            // Murowana Goślina, Polska
	{ lat: 52.645290, lng: 16.810294, name: "🇵🇱 Oborniki", icon: redDotIcon10 },                    // Oborniki, Polska
	{ lat: 52.708076, lng: 16.526785, name: "🇵🇱 Obrzycko", icon: redDotIconSmall },                    // Obrzycko, Polska
	{ lat: 52.7866, lng: 16.1801, name: "🇵🇱 Puszcza Notecka", icon: greenDotIcon12 },             // Puszcza Notecka, Polska
	{ lat: 53.19647, lng: 16.74385, name: "🇵🇱 Rezerwat Kuźnik", icon: greenDotIcon },             // Rezerwat Kuźnik, Polska
	{ lat: 52.67456, lng: 17.16176, name: "🇵🇱 Skoki", icon: redDotIconSmall },                       // Skoki, Polska
	{ lat: 52.80656, lng: 17.20026, name: "🇵🇱 Wągrowiec", icon: redDotIcon },                   // Wągrowiec, Polska
	{ lat: 52.991780, lng: 17.487595, name: "🇵🇱 Kcynia", icon: redDotIcon },                      // Kcynia, Polska
	{ lat: 53.009355, lng: 17.739916, name: "🇵🇱 Szubin", icon: redDotIconSmall },                      // Szubin, Polska
	{ lat: 53.09940, lng: 17.92048, name: "🇵🇱 Białe Błota", icon: redDotIconSmall },                 // Białe Błota, Polska
	{ lat: 52.32506, lng: 17.56523, name: "🇵🇱 Września", icon: redDotIcon },                    // Września, Polska
	{ lat: 52.21720, lng: 17.62452, name: "🇵🇱 Kołaczkowo", icon: redDotIconSmall },                  // Kołaczkowo, Polska
	{ lat: 52.166694, lng: 17.687516, name: "🇵🇱 Pyzdry", icon: redDotIcon },                      // Pyzdry, Polska
	{ lat: 52.208757, lng: 17.930439, name: "🇵🇱 Lądek", icon: redDotIconSmall },                       // Lądek, Polska
	{ lat: 52.244252, lng: 18.09277, name: "🇵🇱 Golina", icon: redDotIconSmall },                      // Golina, Polska
	{ lat: 53.42050, lng: 16.81505, name: "🇵🇱 Jastrowie", icon: redDotIconSmall },                   // Jastrowie, Polska
	{ lat: 53.535181, lng: 16.849959, name: "🇵🇱 Okonek", icon: redDotIconSmall },                      // Okonek, Polska
	{ lat: 52.509228, lng: 16.971678, name: "🇵🇱 Owińska", icon: redDotIconSmall },                     // Owińska, Polska
	{ lat: 52.46855, lng: 16.97976, name: "🇵🇱 Czerwonak", icon: redDotIconSmall },                   // Czerwonak, Polska
	{ lat: 52.279272, lng: 16.700217, name: "🇵🇱 Stęszew", icon: redDotIconSmall },                     // Stęszew, Polska
	{ lat: 52.35830, lng: 16.67673, name: "🇵🇱 Dopiewo", icon: redDotIconSmall },                     // Dopiewo, Polska
	{ lat: 52.396925, lng: 16.675428, name: "🇵🇱 Sierosław", icon: redDotIcon },                   // Sierosław, Polska
	{ lat: 52.29499, lng: 16.78275, name: "🇵🇱 Rosnówko", icon: redDotIconSmall },                    // Rosnówko, Polska
	{ lat: 49.29464, lng: 19.95218, name: "🇵🇱 Zakopane", icon: redDotIcon },                    // Zakopane, Polska
	{ lat: 49.22825, lng: 19.98801, name: "🇵🇱 2012 🔺 Beskid", icon: triangleSvg },                // Beskid, Polska
	{ lat: 49.23185, lng: 19.98153, name: "🇵🇱 1987 🔺 Kasprowy Wierch", icon: triangleSvg },       // Kasprowy Wierch, Polska
	{ lat: 49.16610, lng: 20.18459, name: "🇸🇰 2452 🔺 Sławkowski Szczyt", icon: triangleSvg },     // Sławkowski Szczyt, Słowacja
	{ lat: 49.13983, lng: 20.22096, name: "🇸🇰 Vysoke Tatry", icon: redDotIconSmall },                // Vysoké Tatry, Słowacja
	{ lat: 52.55392, lng: 17.11224, name: "🇵🇱 Puszcza Zielonka", icon: greenDotIcon12 }, // Puszcza Zielonka, Polska
	{ lat: 49.2228, lng: 20.0638, name: "🇵🇱 🇸🇰 Tatrzański Park Narodowy", icon: greenDotIcon12 }, // Tatrzański Park Narodowy, Polska, Słowacja      
	{ lat: 49.33994, lng: 20.13824, name: "🇵🇱 Jurgów", icon: redDotIconSmall },                // Jurgów, Polska
	{ lat: 50.83161, lng: 15.77667, name: "🇵🇱 Mysłakowice", icon: redDotIconSmall },                // Mysłakowice, Polska
          	// ROK 2023se USA
    		{ lat: 40.7128, lng: -74.0060, name: "🇺🇸 Nowy Jork", icon: redSquareIcon30 },       // Nowy Jork, USA
    		{ lat: 34.0522, lng: -118.2437, name: "🇺🇸 Los Angeles", icon: redSquareIcon30 },    // Los Angeles, USA
    		{ lat: 39.7392, lng: -104.9903, name: "🇺🇸 Denver", icon: redSquareIcon },         // Denver, USA
    		{ lat: 36.1699, lng: -115.1398, name: "🇺🇸 Las Vegas", icon: redSquareIcon },      // Las Vegas, USA      
    		{ lat: 35.1495, lng: -90.0490, name: "🇺🇸 Memphis", icon: redDotIconMedium },         // Memphis, USA
			// Denver
            { lat: 39.665368, lng: -105.205754, name: "🇺🇸 Red Rock Park", icon: reddiamondSvg }, // Red Rock Park
            { lat: 39.67894, lng: -105.92024, name: "🇺🇸 Tunel Eisehowera", icon: reddiamondSvg }, // Tunel Eisehowera
            { lat: 39.601720, lng: -106.088877, name: "🇺🇸 Silverthorne HW", icon: reddiamondSvg }, // Silverthorne HW
          	// Canyonlands
            { lat: 38.30341, lng: -109.86774, name: "🇺🇸 Grand View Point", icon: reddiamondSvg }, // Grand View Point
            { lat: 38.31024, lng: -109.85676, name: "🇺🇸 Grand View Point Overlook", icon: reddiamondSvg }, // Grand View Point Overlook
            { lat: 38.345948, lng: -109.860379, name: "🇺🇸 Buck Canyon Overlook", icon: reddiamondSvg }, // Buck Canyon Overlook
            { lat: 38.378176, lng: -109.888204, name: "🇺🇸 Green River Overlook", icon: reddiamondSvg }, // Green River Overlook
            { lat: 38.388066, lng: -109.863625, name: "🇺🇸 Mesa Arch", icon: reddiamondSvg }, // Mesa Arch
            { lat: 38.452799, lng: -109.817984, name: "🇺🇸 Shafer Canyon Overlook", icon: reddiamondSvg }, // Shafer Canyon Overlook
            { lat: 38.469366, lng: -109.739513, name: "🇺🇸 Dead Horse Point", icon: reddiamondSvg }, // Dead Horse Point
          	// Arches
            { lat: 38.62530, lng: -109.59989, name: "🇺🇸 Park Avenue View Point", icon: reddiamondSvg }, // Park Avenue View Point
            { lat: 38.700964, lng: -109.564692, name: "🇺🇸 Balanced Rock", icon: reddiamondSvg }, // Balanced Rock
            { lat: 38.691686, lng: -109.540392, name: "🇺🇸 Double Arch", icon: reddiamondSvg }, // Double Arch
            { lat: 38.685430, lng: -109.533134, name: "🇺🇸 Windows Arches", icon: reddiamondSvg }, // Windows Arches
            { lat: 38.743530, lng: -109.499257, name: "🇺🇸 Delicate Arch", icon: reddiamondSvg }, // Delicate Arch
            { lat: 38.764376, lng: -109.581110, name: "🇺🇸 Sand Dune Arch", icon: reddiamondSvg }, // Sand Dune Arch
            { lat: 38.791155, lng: -109.606937, name: "🇺🇸 Landscape Arch", icon: reddiamondSvg }, // Landscape Arch
            { lat: 38.799189, lng: -109.621193, name: "🇺🇸 Double O Arch", icon: reddiamondSvg }, // Double O Arch
          	// Monument Valley
            { lat: 37.27682, lng: -109.93598, name: "🇺🇸 View of Valley of Gods", icon: reddiamondSvg }, // View of Valley of Gods
          	{ lat: 37.17365, lng: -109.84895, name: "🇺🇸 Mexican Hat", icon: reddiamondSvg }, // Mexican Hat
            { lat: 37.10148, lng: -109.99085, name: "🇺🇸 Forrest Gump Point", icon: reddiamondSvg }, // Forrest Gump Point
            { lat: 36.982522, lng: -110.111622, name: "🇺🇸 Monument Valley View Point", icon: reddiamondSvg }, // Monument Valley View Point
          	// Page
            { lat: 36.903157, lng: -111.413094, name: "🇺🇸 Kanion Antylopy (lower)", icon: reddiamondSvg }, // Kanion Antylopy (lower)
            { lat: 36.88072, lng: -111.51099, name: "🇺🇸 Horseshoe Bend", icon: reddiamondSvg }, // Horseshoe Bend
            { lat: 36.924277, lng: -111.478572, name: "🇺🇸 Glen Canyon Dam Overlook", icon: reddiamondSvg }, // Glen Canyon Dam Overlook
            { lat: 36.968915, lng: -111.499010, name: "🇺🇸 Wahweap Viewpoint", icon: reddiamondSvg }, // Wahweap Viewpoint
          	// Wielki Kanion Kolorado
            { lat: 36.04230, lng: -111.82541, name: "🇺🇸 Desert View Watchtower", icon: reddiamondSvg }, // Desert View Watchtower
            { lat: 36.038394, lng: -111.837580, name: "🇺🇸 Navajo Point", icon: reddiamondSvg }, // Navajo Point
            { lat: 36.00495, lng: -111.92412, name: "🇺🇸 Moran Point", icon: reddiamondSvg }, // Moran Point
            { lat: 35.998155, lng: -111.987838, name: "🇺🇸 Grandview Point", icon: reddiamondSvg }, // Grandview Point
            { lat: 36.058345, lng: -112.083201, name: "🇺🇸 Yaki Point", icon: reddiamondSvg }, // Yaki Point
            { lat: 36.061507, lng: -112.087042, name: "🇺🇸 Ooh Aah Point", icon: reddiamondSvg }, // Ooh Aah Point
            { lat: 36.065978, lng: -112.090893, name: "🇺🇸 Cedar Ridge", icon: reddiamondSvg }, // Cedar Ridge
            { lat: 36.061511, lng: -112.108140, name: "🇺🇸 Mather Point", icon: reddiamondSvg }, // Mather Point
          	// Dolina Śmierci
            { lat: 36.230012, lng: -116.767631, name: "🇺🇸 Badwater Basin", icon: upsideDownTriangleSvg }, // Badwater Basin
            { lat: 36.36421, lng: -116.80080, name: "🇺🇸 Artist's Palette", icon: reddiamondSvg }, // Artist's Palette
            { lat: 36.39626, lng: -116.83717, name: "🇺🇸 Desolation Canyon", icon: reddiamondSvg }, // Desolation Canyon
            { lat: 36.420877, lng: -116.846455, name: "🇺🇸 Golden Canyon", icon: reddiamondSvg }, // Golden Canyon
            { lat: 36.606847, lng: -117.115502, name: "🇺🇸 Mosquito Flat Sand Dunes", icon: reddiamondSvg }, // Mosquito Flat Sand Dunes
            { lat: 36.42009, lng: -116.81233, name: "🇺🇸 Zabriskie Point", icon: reddiamondSvg }, // Zabriskie Point
            { lat: 36.22072, lng: -116.72678, name: "🇺🇸 Dante's Point", icon: reddiamondSvg }, // Dante's Point
          	// Parki Narodowe
          	{ lat: 36.97417, lng: -110.08309, name: "🇺🇸 Monument Valley", icon: greenDotIcon12 }, // Monument Valley
            { lat: 37.31106, lng: -109.84989, name: "🇺🇸 Valley of Gods", icon: greenDotIcon12 }, // Valley of Gods
			{ lat: 38.64731, lng: -109.55618, name: "🇺🇸 Park Narodowy Arches", icon: greenDotIcon12 }, // Park Narodowy Arches
            { lat: 38.33344, lng: -109.82260, name: "🇺🇸 Park Narodowy Canyonlands", icon: greenDotIcon12 }, // Park Narodowy Canyonlands
            { lat: 36.1501, lng: -116.9234, name: "🇺🇸 Dolina Śmierci", icon: greenDotIcon12 }, // Dolina Śmierci
            { lat: 36.0785, lng: -111.9665, name: "🇺🇸 Wielki Kanion Colorado", icon: greenDotIcon12 }, // Wielki Kanion Colorado
            { lat: 36.9147, lng: -111.4558, name: "🇺🇸 Page", icon: redDotIcon }, // Page
            { lat: 38.5733, lng: -109.5498, name: "🇺🇸 Moab", icon: redDotIcon }, // Moab
          	{ lat: 36.46066, lng: -116.86664, name: "🇺🇸 Furnace Creek", icon: redDotIcon10 }, // Furnace Creek
            { lat: 35.0402, lng: -89.7085, name: "🇺🇸 Collierville", icon: redDotIcon }, // Collierville
            { lat: 36.99830, lng: -111.43810, name: "🇺🇸 Glen Canyon Recreation Area", icon: greenDotIcon12 }, // Glen Canyon Recreation Area
           	// Los Angeles
          	{lat: 34.11823149555428, lng: -118.30036603860084, name: "🇺🇸 Obserwatorium Griffitha", icon: reddiamondSvg }, // Los Angeles
       
  
	],
      2023: [
            // ROK 2023
    	{ lat: 52.2297, lng: 21.0122, name: "🇵🇱 Warszawa", icon: redSquareIcon },         // Warszawa, Polska
    	{ lat: 52.4064, lng: 16.9252, name: "🇵🇱 Poznań", icon: redDotIconMedium },           // Poznań, Polska
    	{ lat: 52.5341, lng: 17.5821, name: "🇵🇱 Gniezno", icon: redDotIcon },          // Gniezno, Polska
        { lat: 52.40760, lng: 22.25163, name: "🇵🇱 Sokołów Podlaski", icon: redDotIcon }, // Sokołów Podlaski, Polska
          // Punkty na mapie ROK 2023 - Praga  
    	{ lat: 50.08746, lng: 14.42116, name: "🇨🇿 Praga", icon: redSquareIcon }, // Praga, Czechy
    	{ lat: 49.91011, lng: 14.74073, name: "🇨🇿 Hrusice", icon: redDotIconSmall }, // Hrusice, Czechy
    	{ lat: 50.40731, lng: 16.51237, name: "🇵🇱 Polanica-Zdrój", icon: redDotIcon }, // Polanica-Zdrój, Polska
    	{ lat: 50.41388, lng: 16.44666, name: "🇵🇱 Szczytna", icon: redDotIcon10 }, // Szczytna, Polska
    	{ lat: 50.40915, lng: 16.48803, name: "🇵🇱 538 🔺 Piekielna Góra", icon: triangleSvg }, // Szczyt Piekielna Góra, Polska
          // Punkty na mapie ROK 2023 - Czeska i Saksońska Szwajcaria
        { lat: 50.85245, lng: 14.39424, name: "🇨🇿 Jetřichovice", icon: redDotIconSmall }, // Jetřichovice, Czechy  
		{ lat: 50.87141, lng: 14.39981, name: "🇨🇿 484 🔺 Rudolfův kámen", icon: triangleSvg }, // Rudolfův Kámen, Czechy
		{ lat: 50.86675, lng: 14.40602, name: "🇨🇿 439 🔺 Vilemínina Stěna", icon: triangleSvg }, // Vilemínina Stěna, Czechy
		{ lat: 50.86044, lng: 14.40496, name: "🇨🇿 428 🔺 Mariina Skála", icon: triangleSvg }, // Mariina Skála, Czechy
		{ lat: 50.854807, lng: 14.405404, name: "🇨🇿 295 🔺 Falkenštejn", icon: triangleSvg }, // Zamek Falkenštejn, Czechy
        { lat: 50.87568, lng: 14.23697, name: "🇨🇿 Hřensko", icon: redDotIconSmall }, // Hřensko, Czechy  
		{ lat: 50.883712, lng: 14.281264, name: "🇨🇿 373 🔶 Brama Pravčicka", icon: reddiamondSvg }, // Brama Pravčicka, Czechy
		{ lat: 50.884108, lng: 14.280177, name: "🇨🇿 295 🔺 Vyhlídkové místo", icon: triangleSvg }, // Brama Pravčicka, Czechy
        { lat: 50.86096, lng: 14.26974, name: "🇨🇿 349 🔺 Jenowski Wierch 🗼", icon: triangleSvg }, // Janov, Czechy
        { lat: 50.8588, lng: 14.3368, name: "🇨🇿 Czeska Szwajcaria", icon: greenDotIcon12 }, // Czeska Szwajcaria, Czechy
        { lat: 50.95345, lng: 14.14267, name: "🇩🇪 Saksońska Szwajcaria", icon: greenDotIcon12 }, // Saksońska Szwajcaria, Niemcy
		{ lat: 50.961709, lng: 14.073103, name: "🇩🇪 305 🔶 Most Bastei", icon: reddiamondSvg }, // Most Bastei, Niemcy
        { lat: 50.958900, lng: 14.082606, name: "🇩🇪 Kurort Rathen - Skalne Labirynty", icon: redDotIconSmall }, // Kurort Rathen, Niemcy
		{ lat: 50.84956, lng: 14.22148, name: "🇨🇿 Labská Stráň - Belvedér", icon: redDotIcon10 }, // Belvedér, Czechy
		{ lat: 50.787713, lng: 14.028897, name: "🇨🇿 Tiské stěny", icon: reddiamondSvg }, // Tiské stěny, Czechy
		{ lat: 50.91801, lng: 14.15202, name: "🇩🇪 Bad Schandau", icon: redDotIcon }, // Bad Schandau, Niemcy
		{ lat: 50.78109, lng: 14.21253, name: "🇨🇿 Děčín", icon: redDotIcon }, // Děčín, Czechy
        { lat: 50.834308, lng: 14.262024, name: "🇨🇿 Arnoltice", icon: redDotIconSmall }, // Arnoltice, Czechy
		{ lat: 51.05349, lng: 16.18947, name: "🇵🇱 Jawor", icon: redDotIcon10 }, // Jawor, Polska
          // Punkty na mapie ROK 2023 - Wyspa Rugia
		{ lat: 54.40162, lng: 13.61457, name: "🇩🇪 Binz", icon: redDotIcon }, // Binz, Niemcy
		{ lat: 54.31614, lng: 13.09294, name: "🇩🇪 Stralsund", icon: redDotIcon }, // Stralsund, Niemcy
		{ lat: 53.62358, lng: 11.41235, name: "🇩🇪 Schwerin", icon: redDotIcon }, // Schwerin, Niemcy
          // Punkty na mapie ROK 2023 - mniejsze
        { lat: 52.24530, lng: 16.84711, name: "🇵🇱 Mosina", icon: redDotIconSmall }, // Mosina, Polska
		{ lat: 52.29000, lng: 16.85895, name: "🇵🇱 Puszczykowo", icon: redDotIconSmall }, // Puszczykowo, Polska
		{ lat: 52.27252, lng: 16.86127, name: "🇵🇱 Puszczykówko", icon: redDotIconSmall }, // Puszczykówko, Polska
        { lat: 52.34478, lng: 16.88178, name: "🇵🇱 Luboń", icon: redDotIconSmall }, // Luboń, Polska
        { lat: 52.236890, lng: 16.925586, name: "🇵🇱 Dęby Rogalińskie", icon: reddiamondSvg }, // Dęby Rogalińskie, Polska
        { lat: 52.23371, lng: 16.93236, name: "🇵🇱 Rogalin - Zespół Pałacowy", icon: redDotIcon10 }, // Rogalin Polska
        { lat: 52.46676, lng: 16.97986, name: "🇵🇱 Czerwonak", icon: redDotIcon10 }, // Czerwonak, Polska
       	{ lat: 52.51153, lng: 16.97295, name: "🇵🇱 Owińska", icon: redDotIcon10 }, // Owińska, Polska
        { lat: 52.39692, lng: 16.675379, name: "🇵🇱 Sierosław", icon: redDotIcon }, // Sierosław, Polska
        { lat: 52.489764, lng: 16.896415, name: "🇵🇱 Poznań - Meteoryt Morasko", icon: reddiamondSvg }, // Poznań, Polska
        { lat: 52.41029, lng: 17.07756, name: "🇵🇱 Swarzędz", icon: redDotIcon10 }, // Swarzędz, Polska
          
        ],
      
      2024: [
          // Punkty na mapie ROK 2024 - Włochy 2024
          	{ lat: 45.4642, lng: 9.1900, name: "🇮🇹 Mediolan", icon: redSquareIcon30 }, // Mediolan, Włochy
            { lat: 44.4056, lng: 8.9463, name: "🇮🇹 Genua", icon: redDotIconMedium }, // Genua, Włochy
            { lat: 47.54606, lng: 9.68350, name: "🇩🇪 Lindau 🗼", icon: redDotIcon }, // Lindau (Bodensee), Niemcy
            { lat: 45.8095, lng: 9.0828, name: "🇮🇹 Como", icon: redDotIcon }, // Como, Włochy
            { lat: 45.8542, lng: 9.3929, name: "🇮🇹 Lecco", icon: redDotIcon }, // Lecco, Włochy
            { lat: 45.9331, lng: 8.5595, name: "🇮🇹 Verbania", icon: redDotIcon }, // Verbania, Włochy
            { lat: 45.7373, lng: 7.3214, name: "🇮🇹 Aosta", icon: redDotIcon }, // Aosta, Włochy
            { lat: 45.5665, lng: 8.0518, name: "🇮🇹 Biella", icon: redDotIcon }, // Biella, Włochy
            { lat: 46.431025, lng: 6.911538, name: "🇨🇭 Montreux", icon: redDotIcon }, // Montreux, Szwajcaria
            { lat: 46.010175, lng: 9.283351, name: "🇮🇹 Varenna", icon: redDotIcon }, // Varenna, Włochy
            { lat: 45.90805, lng: 8.61882, name: "🇮🇹 Laveno", icon: redDotIcon }, // Laveno, Włochy
            { lat: 45.54579, lng: 8.11460, name: "🇮🇹 Candelo", icon: redDotIconSmall }, // Candelo, Włochy
          	{ lat: 47.67705, lng: 8.61601, name: "🇨🇭 Przełom Renu", icon: blueDiamondIconEMOJI1 }, // Przełom Renu, Niemcy
          	{ lat: 47.67705, lng: 8.61601, name: "🇨🇭 Przełom Renu", icon: blueDiamondIconEMOJI2 }, // Przełom Renu, Niemcy
            { lat: 47.6976, lng: 8.6394, name: "🇨🇭 Szafuza", icon: redDotIcon }, // Szafuza, Szwajcaria
            { lat: 45.81453, lng: 6.96031, name: "🇮🇹 Courmayeur", icon: redDotIcon }, // Courmayeur, Włochy
            { lat: 45.44664, lng: 8.62066, name: "🇮🇹 Novara", icon: redDotIcon }, // Novara, Włochy
            { lat: 47.57933, lng: 9.16655, name: "🇨🇭 Berg", icon: redDotIconSmall }, // Berg, Szwajcaria
            { lat: 45.878686, lng: 6.887913, name: "🇫🇷 3842 🔺 Aiguille du Midi", icon: triangleSvg }, // Szczyt Aiguille du Midi
            { lat: 45.845834, lng: 6.931858, name: "🇮🇹 3466 🔺 Punta Hellbronner", icon: triangleSvg }, // Szczyt Punta Hellbronner
            { lat: 45.91160, lng: 8.64349, name: "🇮🇹 1062 🔺 Sasso del Ferro", icon: triangleSvg }, // Szczyt Sasso del Ferro
            { lat: 46.984155002029645, lng: 8.611913361877635, name: "🇨🇭 Parking Brunnen", icon: reddiamondSvg }, // Brunnen, Szwajcaria
            { lat: 46.55833, lng: 8.56380, name: "🇨🇭 Przełęcz Św. Gottarda", icon: upsideDownTriangleSvg  }, // Przełęcz Św. Gottarda
            { lat: 45.86868, lng: 7.16497, name: "🇨🇭🇮🇹 Przełęcz Św. Bernarda", icon: upsideDownTriangleSvg  }, // Przełęcz Św. Bernarda
            { lat: 45.9748, lng: 9.2302, name: "🇮🇹 Jezioro Como", icon: blueDiamondIcon }, // Jezioro Como
            { lat: 45.8699, lng: 8.5807, name: "🇮🇹 Jezioro Maggiore", icon: blueDiamondIcon }, // Jezioro Maggiore
            { lat: 46.4478, lng: 6.7082, name: "🇨🇭🇫🇷 Jezioro Genewskie", icon: blueDiamondIcon }, // Jezioro Genewskie
            { lat: 46.9409, lng: 8.5982, name: "🇨🇭 Jezioro Czterech Kantonów", icon: blueDiamondIcon }, // Jezioro Czterech Kantonów
            { lat: 47.5471, lng: 9.5145, name: "🇩🇪🇨🇭🇦🇹 Jezioro Bodeńskie", icon: blueDiamondIcon }, // Jezioro Bodeńskie
            { lat: 47.84030, lng: 8.53363, name: "🇩🇪 Blumberg", icon: redDotIconSmall }, // Blumberg, Niemcy
          	// Punkty na mapie ROK 2024 - Pozostałe 
          	{ lat: 52.4064, lng: 16.9252, name: "🇵🇱 Poznań", icon: redDotIconMedium }, // Poznań, Polska
        	{ lat: 52.26311, lng: 16.80943, name: "🇵🇱 Wielkopolski Park Narodowy", icon: greenDotIcon12 }, // Wielkopolski Park Narodowy, Polska
        	{ lat: 52.55423, lng: 17.11121, name: "🇵🇱 Puszcza Zielonka", icon: greenDotIcon12 }, // Puszcza Zielonka, Polska
        	{ lat: 52.480065, lng: 17.011267, name: "🇵🇱 Dziewicza Góra", icon: triangleSvg }, // Puszcza Zielonka, Polska
        	{ lat: 52.57578, lng: 17.00958, name: "🇵🇱 Murowana Goślina", icon: redDotIcon10 }, // Murowana Goślina, Polska  
        	{ lat: 52.43351, lng: 16.64512, name: "🇵🇱 Lusówko", icon: redDotIcon10 }, // Lusówko, Polska
    		{ lat: 52.22977, lng: 21.01178, name: "🇵🇱 Warszawa", icon: redSquareIcon }, // Warszawa, Polska
   			{ lat: 52.373981, lng: 21.968236, name: "🇵🇱 Liw", icon: redDotIcon10 }, // Zamek Liw, Polska
    		{ lat: 52.40760, lng: 22.25163, name: "🇵🇱 Sokołów Podlaski", icon: redDotIcon }, // Sokołów Podlaski, Polska
    		{ lat: 53.131021, lng: 23.155264, name: "🇵🇱 Białystok", icon: redDotIconMedium }, // Białystok, Polska
    		{ lat: 51.24645, lng: 22.56844, name: "🇵🇱 Lublin", icon: redDotIconMedium }, // Lublin, Polska
    		{ lat: 54.18125, lng: 15.56937, name: "🇵🇱 Kołobrzeg", icon: redDotIcon }, // Kołobrzeg, Polska
    		{ lat: 54.14418, lng: 15.29031, name: "🇵🇱 Mrzeżyno", icon: redDotIcon }, // Mrzeżyno, Polska
    		{ lat: 48.13743, lng: 11.57549, name: "🇩🇪 Monachium", icon: redSquareIcon }, // Monachium, Niemcy
    		{ lat: 52.46676, lng: 16.97986, name: "🇵🇱 Czerwonak", icon: redDotIcon10 }, // Czerwonak, Polska
        	{ lat: 52.51153, lng: 16.97295, name: "🇵🇱 Owińska", icon: redDotIcon10 }, // Owińska, Polska
    		{ lat: 51.48141, lng: 16.91375, name: "🇵🇱 Żmigród - Zespół parko-pałacowy", icon: redDotIcon10 }, // Pałąc Żmigród, Polska
    		{ lat: 51.79853, lng: 15.23570, name: "🇵🇱 Nowogród Bobrzański", icon: redDotIcon10 }, // Nowogród Bobrzański, Polska
    		{ lat: 51.26296, lng: 15.56497, name: "🇵🇱 Bolesławiec", icon: redDotIcon }, // Bolesławiec, Polska
    		{ lat: 51.33643, lng: 15.43390, name: "🇵🇱 Kliczków - Pałac", icon: redDotIcon10 }, // Kliczków, Polska
    		{ lat: 51.61503, lng: 15.32037, name: "🇵🇱 Żagań", icon: redDotIcon }, // Żagań, Polska
    		{ lat: 51.938286, lng: 15.505089, name: "🇵🇱 Zielona Góra", icon: redDotIconsurplus }, // Zielona Góra, Polska
   	 		{ lat: 51.77024, lng: 19.44606, name: "🇵🇱 Łódź", icon: redDotIconMedium }, // Łódź, Polska
    		{ lat: 52.16760, lng: 22.27392, name: "🇵🇱 Siedlce", icon: redDotIcon }, // Siedlce, Polska
    		{ lat: 52.31739, lng: 16.12881, name: "🇵🇱 Nowy Tomyśl", icon: redDotIcon }, // Nowy Tomyśl, Polska
    		{ lat: 52.39692, lng: 16.675379, name: "🇵🇱 Sierosław", icon: redDotIcon }, // Sierosław, Polska
    		{ lat: 52.36613, lng: 16.24749, name: "🇵🇱 Wąsowo", icon: redDotIcon10 }, // Wąsowo, Polska
          // Rok 2024 Mniejsze
			{ lat: 52.24530, lng: 16.84711, name: "🇵🇱 Mosina", icon: redDotIconSmall }, // Mosina, Polska
			{ lat: 52.29000, lng: 16.85895, name: "🇵🇱 Puszczykowo", icon: redDotIconSmall }, // Puszczykowo, Polska
			{ lat: 52.27252, lng: 16.86127, name: "🇵🇱 Puszczykówko", icon: redDotIconSmall }, // Puszczykówko, Polska
        	{ lat: 52.34478, lng: 16.88178, name: "🇵🇱 Luboń", icon: redDotIconSmall }, // Luboń, Polska
        	{ lat: 52.279272, lng: 16.700217, name: "🇵🇱 Stęszew", icon: redDotIconSmall }, // Stęszew, Polska
        	{ lat: 52.35830, lng: 16.67673, name: "🇵🇱 Dopiewo", icon: redDotIconSmall }, // Dopiewo, Polska
            
        ]
    };

    // Krok 3: Obiekt przechowujący markery dla każdego roku
    var displayedMarkers = {
        '2023se': [],
      	2023: [],
        2024: []
    };

    // Krok 5: Funkcja tworząca markery na mapie
   function createMarker(lat, lng, name, icon) {
    return L.marker([lat, lng], { icon: icon }).addTo(map).bindPopup(name);
    
    }

    // Krok 5: Funkcja aktualizacji mapy
    function updateMap() {
        var year2023Checked = document.getElementById('year2023').checked;
        var year2024Checked = document.getElementById('year2024').checked;
    	var year2023seChecked = document.getElementById('year2023se').checked;
      
      // Zarządzanie markerami dla 2023se
          if (year2023seChecked) {
        if (displayedMarkers['2023se'].length === 0) {
            markersData['2023se'].forEach(function(markerData) {
                var marker = createMarker(markerData.lat, markerData.lng, markerData.name, markerData.icon);
                displayedMarkers['2023se'].push(marker);
            });
        }
    } else {
        displayedMarkers['2023se'].forEach(function(marker) {
            map.removeLayer(marker);
        });
        displayedMarkers['2023se'] = [];
    }
      
        // Zarządzanie markerami dla 2023
        if (year2023Checked) {
            if (displayedMarkers[2023].length === 0) { // Dodaj markery tylko, jeśli ich nie ma
                markersData[2023].forEach(function(markerData) {
                    var marker = createMarker(markerData.lat, markerData.lng, markerData.name, markerData.icon);
                    displayedMarkers[2023].push(marker);
                });
            }
        } else {
            // Usuń markery dla 2023
            displayedMarkers[2023].forEach(function(marker) {
                map.removeLayer(marker);
            });
            displayedMarkers[2023] = [];
        }

        // Zarządzanie markerami dla 2024
        if (year2024Checked) {
            if (displayedMarkers[2024].length === 0) { // Dodaj markery tylko, jeśli ich nie ma
                markersData[2024].forEach(function(markerData) {
                    var marker = createMarker(markerData.lat, markerData.lng, markerData.name, markerData.icon);
                    displayedMarkers[2024].push(marker);
                });
            }
        } else {
            // Usuń markery dla 2024
            displayedMarkers[2024].forEach(function(marker) {
                map.removeLayer(marker);
            });
            displayedMarkers[2024] = [];
        }
    }

    // Krok 6: Event listener dla checkboxów
    document.getElementById('year2023').addEventListener('change', updateMap);
    document.getElementById('year2024').addEventListener('change', updateMap);
    document.getElementById('year2023se').addEventListener('change', updateMap);

    // Inicjalne załadowanie markerów
    updateMap();

        // Punkty na mapie ROK 2024 - Włochy 2024
        var markers = [
          
          
            
          
          
          // Punkty na mapie ROK 2024 - Pozostałe 
          
    	
          
           
          
           
	
          
          	
        ];

      
        // Dodawanie markerów do mapy
       markers.forEach(function(marker) {
            L.marker([marker.lat, marker.lng], {icon: marker.icon})
                .bindTooltip(marker.name, {className: 'leaflet-tooltip'}) // Dodanie tooltipa
                .addTo(map);
        });

 
		// Funkcja do zmiany opacity
        function updateOpacity() {
            var checkbox = document.getElementById('toggleOpacity');
            var newOpacity = checkbox.checked ? 0.0 : 0.8; // Nowa przezroczystość w zależności od stanu checkboxa
            
   // Linie
    linesoklodz24a.setStyle({opacity: newOpacity});
    linesoklodz24b.setStyle({opacity: newOpacity});
    linesokwaw24b.setStyle({opacity: newOpacity});
    linesokb24a.setStyle({opacity: newOpacity});
    linesokb242.setStyle({opacity: newOpacity});
    linesoklub24a.setStyle({opacity: newOpacity});
    linesoklub24b.setStyle({opacity: newOpacity});
    linePRAGAKLODZKO.setStyle({opacity: newOpacity});
    linePRAGAKLODZKO2.setStyle({opacity: newOpacity});
    linePRAGAKLODZKO3.setStyle({opacity: newOpacity});
    linerug231.setStyle({opacity: newOpacity});
    linerug232.setStyle({opacity: newOpacity});
    linewrockark231.setStyle({opacity: newOpacity});
    linewrockar232.setStyle({opacity: newOpacity});
    linedenmem231.setStyle({opacity: newOpacity});
    linedenmem232.setStyle({opacity: newOpacity});
    linenymem231.setStyle({opacity: newOpacity});
    linenymem232.setStyle({opacity: newOpacity});

    // Prostokąty
    rectangleIT2024.setStyle({opacity: newOpacity});
    rectangleCZSA2023.setStyle({opacity: newOpacity});
    rectangleNB24.setStyle({opacity: newOpacity});
    rectangleSOK2324.setStyle({opacity: newOpacity});
    rectanglePrag23.setStyle({opacity: newOpacity});
    rectangleKK23.setStyle({opacity: newOpacity});
    rectangleRUG23.setStyle({opacity: newOpacity});
    rectanglemrz24.setStyle({opacity: newOpacity});
    rectanglePoznańrower24.setStyle({opacity: newOpacity});
    rectangleik23.setStyle({opacity: newOpacity});
	rectangletatry23.setStyle({opacity: newOpacity});
          
    // Trapezy
    trapezpustynie23.setStyle({opacity: newOpacity});
    trapezmemphis23.setStyle({opacity: newOpacity});
    trapeznowyjork23.setStyle({opacity: newOpacity});
}

// Event listener dla checkboxa
document.getElementById('toggleOpacity').addEventListener('change', updateOpacity);
    

    // Wczytaj plik GPX
      
     // Wczytaj plik GPX
        new L.GPX('https://www.dropbox.com/scl/fi/xmy40a8xcoraqtyy8ab2b/pila.gpx?rlkey=0sejpnp55q0wfyw9739v2y3oq&st=ws5afshy&dl=1', {
            async: true,
           
            polyline_options: {
                color: '#B8860B',
                weight: 5,
                opacity: 1.0
            }
        }).addTo(map);
      
      
        // Dodawanie tekstu jako popup
        var textPopup = L.divIcon({
            className: 'custom-text',
            html: 'Mediolan'
        });

    
    </script>
</body>
</html>

<!--
     { lat: 52.40813, lng: 16.93371, name: "Poznań - Stary Rynek", icon: redDotIcon10 }, // Poznań, Polska
          { lat: 52.406189, lng: 16.935017, name: "Poznań - Kolegiata", icon: redDotIcon10 }, // Poznań, Polska
          { lat: 52.42129, lng: 16.93504, name: "Poznań - Cytadela", icon: redDotIcon10 }, // Poznań, Polska
          { lat: 52.40820, lng: 16.91864, name: "Poznań - Zamek Cesarski", icon: redDotIcon10 }, // Poznań, Polska
          { lat: 52.409553, lng: 16.917143, name: "Poznań - Park Adama Mickiewicza", icon: redDotIcon10 }, // Poznań, Polska
          { lat: 52.407501, lng: 16.911786, name: "Poznań - Środek Poznania", icon: redDotIcon10 }, // Poznań, Polska
          { lat: 52.411503, lng: 16.948560, name: "Poznań - Archikatedra Św. Piotra i Pawła", icon: redDotIcon10 }, // Poznań, Polska
          { lat: 52.40175, lng: 16.92734, name: "Poznań - Stary Browar", icon: redDotIcon10 }, // Poznań, Polska
          
          { lat: 52.40310, lng: 16.96654, name: "Poznań - jez. Maltańskie", icon: blueDiamondIcon10 }, // Poznań, Polska
          { lat: 52.42692, lng: 16.87749, name: "Poznań - jez. Rusałka", icon: blueDiamondIcon10 }, // Poznań, Polska
          { lat: 52.46066, lng: 16.82239, name: "Poznań - jez. Strzeszyńskie", icon: blueDiamondIcon10 }, // Poznań, Polska
          { lat: 52.46197, lng: 16.78668, name: "Poznań - jez. Kierskie", icon: blueDiamondIcon10 }, // Poznań, Polska
          { lat: 52.26717, lng: 16.79322, name: "jez. Góreckie", icon: blueDiamondIcon10 }, // jez. Góreckie, Polska
          { lat: 52.41815, lng: 17.07048, name: "Swarzędz - jez. Swarzędzkie", icon: blueDiamondIcon10 }, // Poznań, Polska
          { lat: 52.43030, lng: 16.67712, name: "Lusówko - jez. Lusowskie", icon: blueDiamondIcon10 }, // Lusówko, Polska
		  { lat: 52.24981, lng: 21.01219, name: "Warszawa - Stare Miasto", icon: redDotIcon10 }, // Warszawa, Polska
          { lat: 52.24725, lng: 21.01363, name: "Warszawa - Plac Zamkowy", icon: redDotIcon10 }, // Warszawa, Polska
          { lat: 52.23166, lng: 21.00637, name: "Warszawa - Pałac Kultury", icon: redDotIcon10 }, // Warszawa, Polska
          { lat: 52.240947, lng: 21.011310, name: "Warszawa - Pałac Plac Piłsudskiego", icon: redDotIcon10 }, // Warszawa, Polska
          { lat: 52.246216, lng: 21.014271, name: "Warszawa - kościół św Anny", icon: redDotIcon10 }, // Warszawa, Polska
          { lat: 53.130774, lng: 23.165472, name: "Białystok - Pałac Branickich", icon: redDotIcon10 }, // Białystok, Polska
          { lat: 53.130342, lng: 23.163450, name: "Białystok - Pawilon pod Orłem", icon: redDotIcon10 }, // Białystok, Polska
          { lat: 53.132367, lng: 23.158922, name: "Białystok - Rynek", icon: redDotIcon10 }, // Białystok, Polska
          { lat: 53.132599, lng: 23.163074, name: "Białystok - Bazylika Archikatedralna Wniebowzięcia NMP", icon: redDotIcon10 }, // Białystok, Polska
          { lat: 53.134575, lng: 23.145018, name: "Białystok - Kościół Św. Rocha", icon: redDotIcon10 }, // Białystok, Polska
          { lat: 51.247814, lng: 22.567989, name: "Lublin - Stare Miasto", icon: redDotIcon10 }, // Lublin, Polska
          { lat: 51.250467, lng: 22.572269, name: "Lublin - Zamek", icon: redDotIcon10 }, // Lublin, Polska
          { lat: 51.248459, lng: 22.559770, name: "Lublin - Plac Litewski", icon: redDotIcon10 }, // Lublin, Polska
          { lat: 51.938560, lng: 15.505362, name: "Zielona Góra - Stary Rynek", icon: redDotIcon10 }, // Zielona Góra, Polska
          { lat: 51.938335, lng: 15.513189, name: "Palmiarnia Zielona Góra", icon: redDotIcon10 }, // Palmiarnia Zielona Góra, Polska
          { lat: 52.536802, lng: 17.592716, name: "Gniezno - Archikatedra Wniebowzięcia NMP ", icon: redDotIcon10 }, // Gniezno, Polska
          { lat: 52.535908, lng: 17.595903, name: "Gniezno - Rynek", icon: redDotIcon10 }, // Gniezno, Polska
          { lat: 50.086494, lng: 14.411311, name: "Praga - Most Karola", icon: redDotIcon10 }, // Praga, Czechy
          { lat: 50.081021, lng: 14.451066, name: "Praga - Wieża TV", icon: redDotIcon10 }, // Praga, Czechy
          { lat: 50.09063, lng: 14.40035, name: "Praga - Zamek na Hradczanach", icon: redDotIcon10 }, // Praga, Czechy
          { lat: 50.079816, lng: 14.429662, name: "Praga - Plac Wacława", icon: redDotIcon10 }, // Praga, Czechy
          { lat: 50.087552, lng: 14.421811, name: "Praga - Ratusz i rynek główny", icon: redDotIcon10 }, // Praga, Czechy
          { lat: 50.064488, lng: 14.417979, name: "Praga - Wyszechrad", icon: redDotIcon10 }, // Praga, Czechy
          { lat: 51.61150, lng: 15.32425, name: "Pałac Żagań", icon: redDotIcon10 }, // PAłac Żagań, Polska
        	{ lat: 50.958900, lng: 14.082606, name: "Kurort Rathen - Skalne Labirynty", icon: redDotIcon10 }, // Kurort Rathen, Niemcy
        	{ lat: 50.916681, lng: 14.161248, name: "Bad Schandau - wieża", icon: redDotIcon10 }, // Bad Schandau wieża, Niemcy
            { lat: 53.62441, lng: 11.41858, name: "Zamek w Schwerinie", icon: redDotIcon10 }, // Schwerin, Niemcy
            { lat: 50.961711, lng: 14.071995, name: "Balkon Saksonii", icon: redDotIcon10 }, // Balkon Saksonii, Niemcy

         	{ lat: 44.737, lng: 10.429, name: "🇮🇹 Włochy", icon: ITAflagSVG }, // 🇮🇹 Włochy
          	{ lat: 51.344, lng: 19.688, name: "🇵🇱 Polska", icon: POLflagSVG }, // 🇵🇱 Polska
          	{ lat: 50.219, lng: 9.679, name: "🇩🇪 Niemcy", icon: GERflagSVG }, // 🇩🇪 Niemcy
          	{ lat: 46.928, lng: 6.702, name: "🇨🇭 Szwajcaria", icon: SUIflagSVG }, // 🇨🇭 Szwajcaria
          	{ lat: 45.35, lng: 4.44, name: "🇫🇷 Francja", icon: FRAflagSVG }, // 🇫🇷 Francja
          	{ lat: 47.145, lng: 10.898, name: "🇦🇹 Austria", icon: AUTflagSVG }, // 🇦🇹 Austria
          	{ lat: 49.774, lng: 15.326, name: "🇨🇿 Czechy", icon: CZEflagSVG }, // 🇨🇿 Czechy
-->
