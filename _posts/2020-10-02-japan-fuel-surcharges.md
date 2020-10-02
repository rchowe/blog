---
title: A (Fixed) More Optimal Round the World Trip
layout: post
tags: Travel Japan ANA
excerpt_separator: <!--more-->
---

Back in June, [I wrote about a really good use of points for a round-the-world trip in business class]({% post_url 2020-06-25-ana-round-the-world %})
to see a bunch of Asian countries and then a few European countries, for 115,000 ANA miles. However, going back to it,
I realize there is one major flaw with the example itinerary I gave.

<!--more-->

<script src="https://d3js.org/d3.v4.js"></script>
<script src="https://d3js.org/d3-scale-chromatic.v1.min.js"></script>
<script src="https://d3js.org/d3-geo-projection.v2.min.js"></script>

## The Original Route

When I wrote the old post, I noted that there is not a direct flight from Boston to Asia on any Star Alliance airline,
however there are many more direct flight options to Z&uuml;rich on Swiss. Here is the route that I proposed:

<div class="svg-container" style="max-width: 1000px; margin: 1em auto;">
    <svg id="route-map" data-hemisphere="pacific" data-label-points="true" style="width: 100%; max-width: 1000px;"></svg>
</div>

- **Boston (BOS) to Chicago O'Hare (ORD)** &mdash; United
- **Chicago O'Hare (ORD) to Tokyo Haneda (HND) or Narita (NRT)** &mdash; United or ANA
- **Tokyo Haneda (HND) or Narita (NRT) to Seoul Incheon (ICN)** &mdash; ANA, Asiana, or Ethiopian
- **Seoul Inceon (ICN) to Taipei Taoyuan (TPE)** &mdash; Asiana or EVA Air
- **Taipei Taoyuan (TPE) to Bangkok (BKK)** &mdash; EVA Air or Thai Airways
- **Bangkok (BKK) to Vienna (VIE)** &mdash; EVA Air
- **Vienna (VIE) to Z&uuml;rich (ZRH)** &mdash; Austrian Airlines or Swiss Air Lines
- **Z&uuml;rich (ZRH) to Boston (BOS)** &mdash; Swiss Air Lines

## The Issue

That routing would be bookable. However, when I wrote the previous post, I did not realize that ANA passes on fuel
surcharges from Swiss, which total to about $700 for a one-way business class flight (this is unfortunately pretty
common among some European operators). As cool as business class on Swiss seems to be, I don't think it's worth a $700
fuel surcharge, so we need another way of getting there.

## Options

Among European Star Alliance airlines, ANA only seems to charge fuel surcharges on the Lufthasna Group airlines
(Lufthansa, Swiss, Austrian, Brussels Airlines). All of the other options will involve a connection, but luckily there
are a bunch of other Star Alliance airlines that ANA does not charge fuel surcharges on, so our options are:

- **United** - A ton of flights from Z&uuml;rich to the United States, Polaris is apparently pretty good if you get a
    plane with the new seats, and you get US domestic first on a connection instead of EuroBusiness.
- **Air Canada** - Similar to United, but you have to go through immigration in both Canada and the US.
- **TAP Air Portugal** -- A bunch of flights via Lisbon to Boston, but feels like more of a budget carrier than some of
    the others on this list.
- **Scandanavian Airlines (SAS)** -- Don't know that much, but going via Copenhagen might be kind of cool.
- **Turkish Airlines** -- It would be a bit of a backtrack (and you'd have to keep your eyes on the mileage since the
    RTW award is billed by mileage), but apparently the food is amazing. Unfortunately, they do seem to charge a ~$350
    fuel surcharge.

Unfortunately, the following are not options:

- **Singapore Airlines** -- They fly Singapore to Frankfurt to New York (JFK), and they will sell you Frankfurt to JFK
    as a standalone segment, but they do not open business class awards to partner programs.
- **Virgin Atlantic** -- You can book Virgin Atlantic flights with ANA miles, but sadly they do not qualify for the RTW
    awards since they are not a Star Alliance airline. They also have some hefty fuel surcharges.

## The New Route

Who knows if I will ever get to fly this due to COVID-19, but here goes:

<div class="svg-container" style="max-width: 1000px; margin: 1em auto;">
    <svg id="new-route-map" data-hemisphere="pacific" data-label-points="true" style="width: 100%; max-width: 1000px;"></svg>
</div>

- **Boston (BOS) to Chicago O'Hare (ORD)** &mdash; United
- **Chicago O'Hare (ORD) to Tokyo Haneda (HND) or Narita (NRT)** &mdash; United or ANA
- **Tokyo Haneda (HND) or Narita (NRT) to Seoul Incheon (ICN)** &mdash; ANA, Asiana, or Ethiopian
- **Seoul Inceon (ICN) to Taipei Taoyuan (TPE)** &mdash; Asiana or EVA Air
- **Taipei Taoyuan (TPE) to Bangkok (BKK)** &mdash; EVA Air or Thai Airways
- **Bangkok (BKK) to Vienna (VIE)** &mdash; EVA Air
- **Vienna (VIE) to Z&uuml;rich (ZRH)** &mdash; Austrian Airlines or Swiss Air Lines
- **Z&uuml;rich (ZRH) to Copenhagen (CPH)** &mdash; Scandanavian Airlines or Swiss Air Lines
- **Copenhagen (CPH) to Boston (BOS)** &mdash; Scandinavian Airlines

## Bottom Line

Fuel surcharges are annoying. Swiss has some outrageously high fuel surcharges, though it is not unique among European
carriers. To avoid them when booking through ANA, you have to fly another carrier.

<script>
const maps = {
    "route-map": [
        ["BOS", -71.006388, 42.362944],
        ["ORD", -87.906596, 41.974522],
        ["TYO", 139.781111, 35.553333],
        ["ICN", 126.450517, 37.469075],
        ["TPE", 121.232822, 25.077731],
        ["BKK", 100.747283, 13.681108],
        ["VIE", 16.569722, 48.110278],
        ["ZRH", 8.549167, 47.464722],
        ["BOS", -71.006388, 42.362944]
    ],
    "new-route-map": [
        ["BOS", -71.006388, 42.362944],
        ["ORD", -87.906596, 41.974522],
        ["TYO", 139.781111, 35.553333],
        ["ICN", 126.450517, 37.469075],
        ["TPE", 121.232822, 25.077731],
        ["BKK", 100.747283, 13.681108],
        ["VIE", 16.569722, 48.110278],
        ["ZRH", 8.549167, 47.464722],
        ["CPH", 12.655972, 55.617917],
        ["BOS", -71.006388, 42.362944]
    ]
};

const textPositionOrder = [
    [2, -3, "red"],
    [-3, -2, "blue"],
    [0, 7, "green"],
    [0, 5, "orange"]
];
let closeLabelRadius = 11;

for (let key in maps) {
    if (!maps.hasOwnProperty(key))
        continue;
    
    let airports = maps[key];
    
    let svg = d3.select("svg#" + key);
    
    if (svg == null)
        continue;
    
    let width  = svg.node().getBoundingClientRect().width || 1000,
        height = width * 0.7;

    svg.attr("height", height);
    
    let projection = d3.geoOrthographic()
        .rotate([105, -90])
        .translate([width / 2, height / 2]);

    let links = airports.reduce(function (acc, point) {
        if (typeof(point) === "string" && point.startsWith("-"))
            acc.push([]);
        else
            acc[acc.length-1].push(point);
        return acc;
    }, [[]]).map(function (link) {
        return {
            type: "LineString",
            coordinates: link.map(function (l) { return [l[1], l[2]]; })
        };
    });

    let path = d3.geoPath()
        .projection(projection);
    
    d3.json("https://raw.githubusercontent.com/holtzy/D3-graph-gallery/master/DATA/world.geojson", function (data) {
        var group = svg.append("g");
        group
            .selectAll("path")
            .data(data.features)
            .enter()
            .append("path")
                .attr("fill", "#cccccc")
                .attr("d", d3.geoPath().projection(projection))
                .style("stroke", "#fff")
                .style("stroke-width", 1);
        
        // Add the path
        for (let i = 0; i < links.length; i++) {
            let link = links[i];
            group.append("path")
                .attr("d", path(link))
                .style("fill", "none")
                .style("stroke", "white")
                .style("stroke-width", 6);
            // Add the path
            group.append("path")
                .attr("d", path(link))
                .style("fill", "none")
                .style("stroke", "#00aaff")
                .style("stroke-width", 3);
        }

        // Airports and Labels
        for (let i = 0; i < airports.length; i++) {
            let item = airports[i];
            if (typeof(item) === "string" && item.startsWith("-"))
                continue;
            let point = projection([item[1], item[2]]);
            group.append("circle")
                .attr("cx", point[0])
                .attr("cy", point[1])
                .attr("r", 5)
                .style("fill", "#0088ee")
                .style("stroke", "white")
                .style("stroke-width", 2);
        }

        if (svg.attr("data-label-points") == "true")
        {
            let labelPositions = [];
            let pointsLabeled = {};
            for (let i = 0; i < airports.length; i++) {
                let item = airports[i];
                if (pointsLabeled.hasOwnProperty(item[0]))
                    continue;
                pointsLabeled[item[0]] = true;

                let lx = item[1], ly = item[2];
                for (var j = 0; j < textPositionOrder.length; j++)
                {
                    lx += textPositionOrder[j][0];
                    ly += textPositionOrder[j][1];
                    var closeLabels = labelPositions
                        .map(function (p) { return Math.sqrt((p[0] - lx) * (p[0] - lx) + (p[1] - ly) * (p[1] - ly)); });
                    closeLabels.sort();
                    if (closeLabels.length == 0 || closeLabels[0] > closeLabelRadius)
                    {
                        labelPositions.push([lx, ly]);
                        
                        var text = svg.append("text")
                            .attr("x", projection([lx, ly])[0])
                            .attr("y", projection([lx, ly])[1])
                            .text(item[0])
                            .style("font-weight", "bold")
                            .style("font-size", 12)
    //                        .style("fill", textPositionOrder[j][2])
                            .style("fill", "#000000");
                        if (textPositionOrder[j][0] < 0)
                            text.style("text-anchor", "end");
                        break;
                    }
                }
            }
        }

        svg.select("g")
            .attr("transform", "scale(" + 1.35 * width / 1000 + ")");
    });
}

function sizeChange() {
    for (let key in maps) {
        let svg    = d3.select("svg#" + key),
            width  = svg.node().getBoundingClientRect().width || 1000,
            height = width * 0.6;
        
        console.log("size change");
        svg.select("g")
            .attr("transform", "scale(" + 1.35 * width / 1000 + ")");
        svg.attr("height", height);
    }
}

d3.select(window).on("resize", sizeChange);

</script>