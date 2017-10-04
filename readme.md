# Assessment 1

De opdracht voor assessment 1 bestaat uit het visualiseren van bepaalde data. Ik heb gekozen om te gaan werken met de data over de temperatuur. 

Ik ben allereerst gaan kijken op https://bl.ocks.org/mbostock/3883195 en heb de area chart uitgekozen om mee aan de slag te gaan. Vervolgens heb ik de volgende files aan gemaakt:

## Aangemaakte files:
- temperature.csv	
- temperature.html	
- temperature.js
- temperature.css

## Aangepaste code
Ik ben toen gaan werken met de code heb de volgende dingen aangepast:

### Van
```html
<!DOCTYPE html>
<svg width="960" height="500"></svg>
<script src="https://d3js.org/d3.v4.min.js"></script>
<script>

var svg = d3.select("svg"),
    margin = {top: 20, right: 20, bottom: 30, left: 50},
    width = +svg.attr("width") - margin.left - margin.right,
    height = +svg.attr("height") - margin.top - margin.bottom,
    g = svg.append("g").attr("transform", "translate(" + margin.left + "," + margin.top + ")");

var parseTime = d3.timeParse("%d-%b-%y");

var x = d3.scaleTime()
    .rangeRound([0, width]);

var y = d3.scaleLinear()
    .rangeRound([height, 0]);

var area = d3.area()
    .x(function(d) { return x(d.date); })
    .y1(function(d) { return y(d.close); });

d3.tsv("data.tsv", function(d) {
  d.date = parseTime(d.date);
  d.close = +d.close;
  return d;
}, function(error, data) {
  if (error) throw error;

  x.domain(d3.extent(data, function(d) { return d.date; }));
  y.domain([0, d3.max(data, function(d) { return d.close; })]);
  area.y0(y(0));

  g.append("path")
      .datum(data)
      .attr("fill", "steelblue")
      .attr("d", area);

  g.append("g")
      .attr("transform", "translate(0," + height + ")")
      .call(d3.axisBottom(x));

  g.append("g")
      .call(d3.axisLeft(y))
    .append("text")
      .attr("fill", "#000")
      .attr("transform", "rotate(-90)")
      .attr("y", 6)
      .attr("dy", "0.71em")
      .attr("text-anchor", "end")
      .text("Price ($)");
});

</script>
```
### Naar
```js
// Deze codes zijn bezit van https://bl.ocks.org/mbostock/3883195 en heb ik naar mijn eigen kunnen aangepast van deze assessment.

// Ik maak hier een variabele aan die ik svg noem en stop d3.select in om zo het svg element in de html te selecteren.
var svg = d3.select("svg");
var margin = {
     top: 20,
     right: 20,
     bottom: 30,
     left: 50
 };
/*Ik maak hier variabelen width en heigth aan en door middel van attr geef ik de variabele svg een with -marginleft - marginright*/
 var width = +svg.attr("width") - margin.left - margin.right;
 var height = +svg.attr("height") - margin.top - margin.bottom;
 var g = svg.append("g").attr("transform", "translate(" + margin.left + "," + margin.top + ")");

// Met "%Y%0m%0d" selecteer ik de manier waarom de datum is gemaakt in de csv.
var parseTime = d3.timeParse("%Y%0m%0d");


// dmv de rangeRound width zorg ik ervoor dat de x as horizontaal loopt en bij 0 begint
 var x = d3.scaleTime()
     .rangeRound([0, width]);

 var y = d3.scaleLinear()
     .rangeRound([height, 0]);

// deze functie (d) returned de date binnen de area.
 var area = d3.area()
     .x(function (d) {
         return x(d.date);
     })
 // deze functie (d) returned de temperatuur binnen de area.
     .y1(function (d) {
         return y(d.temp);
     });


// hiermee wordt het bestand temperature.csv aangeroepen
 d3.csv("temperature.csv", function (d) {
     d.date = parseTime(d.date);
     d.temp = +d.temp;
     return d;
 }, function (error, data) {
     if (error) throw error;

     x.domain(d3.extent(data, function (d) {
         return d.date;
     }));
     
     // met de -6 geef ik aan de de y-as moet beginnen bij -6 graden.
     y.domain([-6, d3.max(data, function (d) {
         return d.temp;
     })]);
     area.y0(y(0));
     
     // de append geeft bepaalde content mee aan geslecteerde elementen. in dit gebal is dat het g element

     g.append("path")
         .datum(data)
         .attr("fill", "steelblue") // hiermee krijg de "path" dus een blauwe kleur. 
         .attr("d", area);


     
     g.append("g")
         .attr("transform", "translate(0," + height + ")")
         .call(d3.axisBottom(x))
         .append("text")
         .attr("fill", "#000")
         .attr("transform", "rotate(-90)")
         .attr("x", 6)
         .attr("dy", "90em")
         .attr("text-anchor", "end")
         .text("Date");


     g.append("g")
         .call(d3.axisLeft(y))
         .append("text")
         .attr("fill", "#000")
         .attr("transform", "rotate(-90)")
         .attr("y", 6)
         .attr("dy", "0.71em")
         .attr("text-anchor", "end")
         .text("Temperature");
 });
 ```
 
 ## License
 
 MIT @ Björn Völkers
