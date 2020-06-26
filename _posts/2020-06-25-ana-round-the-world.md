---
title: A More Optimal Round-the-World Trip
layout: post
tags: Travel Japan ANA
excerpt_separator: <!--more-->
---

Earlier [I wrote about a potential round-the-world trip]({% post_url 2020-06-22-new-trips %}) to see springtime events
in Asia and Europe, and I noted that using 165,000 Cathay Pacific miles to book my original itinerary was a good deal.

Well move over Cathay Pacific, because there's an even better deal.<!--more--> I won't bury the lede: All Nippon Airways
(ANA) has fantastic Star Alliance round the world award prices, with a few caveats.

<script src="https://d3js.org/d3.v4.js"></script>
<script src="https://d3js.org/d3-scale-chromatic.v1.min.js"></script>
<script src="https://d3js.org/d3-geo-projection.v2.min.js"></script>

## The Price

ANA's award chart for a round the world award is as follows:

![ANA Round-the-World Award Chart]({{site.baseurl}}/assets/ana-rtw-20200623.png)

These awards price based on total distance flown, not including ground sectors. For an itinerary similar to the one I
wrote about yesterday, you are probably looking at the 14,000 to 18,000 mile band, which prices at 105,000 miles in
business class, or the 18,000 to 20,000 mile band, which prices at 115,000 miles in business class. There are
[a few more routing rules](https://www.ana.co.jp/en/us/amc/reference/tukau/award/tk/zone.html):

1. You must fly Star Alliance airlines, not any other ANA partner airlines that you could normally redeem miles for.
2. You must cross both the Atlantic and Pacific oceans.
3. You must fly east-west or west-east, without backtracking.
4. Up to 8 stopovers are allowed; of those a maximum of 4 can be in Europe and a maximum of 3 can be in Japan.
5. A maximum of 12 flight segments and 4 ground segments are allowed on one ticket. Transfers between different airports
   in the same city (e.g. taking the train from NRT to HND) count as a ground segment.
6. The trip must be at least 10 days.

ANA also requires at least two degrees of kinship when booking an award for someone else; unfortunately they do not
have the same wonderfully complex chart that Japan Airlines has:

![JAL Kinship Chart]({{site.baseurl}}/assets/jal-two-degrees-kinship.png)
_Japan Airlines provides a chart illustrating two degrees of kinship._

If you are planning on using transferrable points to book this award, note that you might be able to bypass this
restriction if you trust a friend with a credit card -- American Express allows you to transfer points to the frequent
flyer accounts of authorized users on your cards, for example.

Lastly, note that some Star Alliance airlines limit award availability to partner airlines. Most notably, you can only
book Singapore Airlines business class through Singapore's own frequent flyer program or Alaska Airlines' program, but
not any other Star Alliance program.

## My Route

Like oneworld route that I outlined yesterday, I find a diversity of airlines interesting. Unfortunately, there is not
a direct flight from Boston to Asia on any Star Alliance airline, however there are many more direct flight options
to Z&uuml;rich on Swiss, so it is about a wash.

<div class="svg-container" style="max-width: 1000px; margin: 1em auto;">
    <svg id="route-map" data-hemisphere="pacific" data-label-points="true" style="width: 100%; max-width: 1000px;"></svg>
</div>

- **Boston (BOS) to Chicago O'Hare (ORD)** &mdash; United
- **Chicago O'Hare (ORD) to Tokyo Haneda (HND)** &mdash; United or ANA
- **Tokyo Haneda (HND) to Seoul Incheon (ICN)** &mdash; ANA or Asiana
- **Seoul Inceon (ICN) to Taipei Taoyuan (TPE)** &mdash; Asiana or EVA Air
- **Taipei Taoyuan (TPE) to Bangkok (BKK)** &mdash; EVA Air or Thai Airways
- **Bangkok (BKK) to Vienna (VIE)** &mdash; EVA Air
- **Vienna (VIE) to Z&uuml;rich (ZRH)** &mdash; Austrian Airlines or Swiss Air Lines
- **Z&uuml;rich (ZRH) to Boston (BOS)** &mdash; Swiss Air Lines

I resisted the urge to stick Ethiopian Airlines' flight from Tokyo to Seoul in there, no matter how cool I think it is,
given I'd be traveling with friends. However, I did put in a fifth freedom flight from Bangkok to Vienna on EVA Air.
There are a ton of Star Alliance options for jetting around Asia.

This routing is 19,756 miles, per [gcmap.com](http://www.gcmap.com), so it would actually cost 115,000 miles in business
class. It would be possible to get it under 18,000 miles by cutting out a few stops, but the 10,000 miles extra seemed
worth it.

## Bottom Line

If you can find award availability on all of the segments you want, you can piece together a great round-the-world trip
at a great price, especially given that you can transfer American Express points to ANA. There is also a ton of value in
the number of free stopovers that you get. I probably will not use this option to book my next trip, but it's a good one
to have for future reference.

<script>
const maps = {
    "route-map": [
        ["BOS", -71.006388, 42.362944],
        ["ORD", -87.906596, 41.974522],
        ["HND", 139.781111, 35.553333],
        ["ICN", 126.450517, 37.469075],
        ["TPE", 121.232822, 25.077731],
        ["BKK", 100.747283, 13.681108],
        ["VIE", 16.569722, 48.110278],
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