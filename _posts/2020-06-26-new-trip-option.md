---
title: The New Japan Trip
layout: post
tags: Travel Japan Japan-Airlines Delta
excerpt_separator: <!--more-->
---

I have officially cancelled my Japan trip in August. It makes me sad, but I also get to start thinking about the next
trip in April, which [I wrote about the other day]({% post_url 2020-06-22-new-trips %}).

<!--more-->

<script src="https://d3js.org/d3.v4.js"></script>
<script src="https://d3js.org/d3-scale-chromatic.v1.min.js"></script>
<script src="https://d3js.org/d3-geo-projection.v2.min.js"></script>

## Routing Options

Broadly, I want to get three people, in business class, from the US to Asia to Europe and back to the US. I'm not too
concerned about having to buy a positioning flight in some regions, but I'd like the intercontinental flights to be
booked with miles. Additionally, given that I now have 120,000 Virgin Atlantic miles, I would like to use those if
possible.

Virgin charges fuel surcharges on almost all transatlantic flights, including those operated by partners such as Delta,
so if possible I would also like to use the Virgin miles for the transpacific segment. Conveniently, there do seem to be
a number of options for awards on Delta.

### North America to Asia

Although you can also redeem Virgin Atlantic miles on ANA and Air China, ANA does not allow one-way awards and I would
prefer to avoid China. Therefore, we're looking at non-stop Delta flights from the mainland US to Asia, which they fly
from their hubs in Atlanta (ATL), Detroit (DTW), Minneapolis (MSP), Seattle (SEA), and Los Angeles (LAX), as well as
a flight to Tokyo from Portland (PDX).

Award availability looks pretty good in April on most of these, so we have some good options. Most of the options are on
the A350 too, which would be really cool.

Based on the Asia to Europe routing, we will likely have to decide whether to go to Japan or Korea first, but it seems
that both are options.

### Asia to Europe

This one is a bit trickier. For non-stop routings from Japan or South Korea to Z&uuml;rich, we are looking at Swiss or
Korean Air. There are a lot of potential one-stop routings, but most programs charge more for an Asia to Europe flight
in business than a North America to Asia flight (about 75,000 miles, though there are a few programs that charge less).
This is where I likely expect to use American Airlines miles.

There are three major ways to get from Asia to Europe using AA miles:

- You can fly from a oneworld hub in Asia; your options in east Asia are Tokyo (Japan Airlines) or Hong Kong (Cathay
  Pacific). Cathay Pacific does have a nonstop flight from Hong Kong to Z&uuml;rich, however I don't currently know how
  the political situation in Hong Kong will evolve within the next year.
- You can fly to a oneworld hub in Europe; your options are London (British Airways), Helsinki (Finnair), or Madrid
  (Iberia). Finnair has a quite large Asia network and is geographically favorable compared to the other two, but may
  not release award seats in long-haul business to partner airlines.
- You can fly through the Middle East; you can redeem American miles on Etihad and Qatar Airways.

Currently I have not decided what I want to do, but I am leaning towards flying through Helsinki. It seems that though
Finnair does not release award seats in long-haul business readily, Japan Airlines does.

### Europe to North America

There are several good deals here (as well as a lot of bad deals):

- Aeroplan charges 55,000 miles for a non-stop flight on Swiss.
- American charges 57,500 miles for a flight via London or Philadelphia, though they add fuel surcharges.

Right now I think I am inclined to go with Aeroplan.

## My Tentative Routing

With all of these routing restrictions in mind, here is the routing I am thinking of:

<div class="svg-container" style="max-width: 1000px; margin: 1em auto;">
    <svg id="route-map" data-hemisphere="pacific" data-label-points="true" style="width: 100%; max-width: 1000px;"></svg>
</div>

For 60,000 Virgin Atlantic miles + $5.60 per person:

- **Atlanta (ATL) to Seoul (ICN)** &mdash; Delta Air Lines, A350 _(Business)_

For 7,500 Delta SkyMiles + $23.46 per person:

- **Seoul - Gimpo (GMP) to Tokyo - Haneda (HND)** &mdash; Korean Air, 777-300 _(Economy)_

For 75,000 American Airlines miles + $45.70 per person:

- **Tokyo - Haneda (HND) to Helsinki (HEL)** &mdash; Japan Airlines, 787-9 or Finnair, A350 _(Business)_
- **Helsinki (HEL) to Z&uuml;rich (ZRH)** &mdash; Finnair, A321 _(Business)_

For 55,000 Aeroplan miles + $54.38 per person:

- **Z&uuml;rich (ZRH) to Boston (BOS)** &mdash; Swiss, A340 _(Business)_

Amazingly, all of these routes currently have three award seats for the dates I am interested in. Luckily, the fuel
surcharges and taxes total about $130 per person, which I think is pretty great for flying around the world in business
class. It's not the 115,000 mile ANA deal, but it's still pretty good in my book.

## Bottom Line

Assuming the Coronavirus has subsided enough to travel by next April, this should be a great trip. I have been to Japan
enough that it is still quite cool but not amazingly special; this trip has the "trip of a lifetime" feeling to me,
because I get to go with my friends and circumnavigate the world.

<script>
const maps = {
    "route-map": [
        ["ATL", -84.427863, 33.636700],
        ["ICN", 126.450517, 37.469075],
        ["HND", 139.781111, 35.553333],
        ["HEL", 24.963333, 60.317222],
        ["ZRH", 8.549167, 47.464722],
        ["BOS", -71.006388, 42.362944],
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