---
title: Can Booking Japan Airlines with Flying Blue Miles be a Good Value?
layout: post
tags: Air-France KLM Flying-Blue Points Japan-Airlines
excerpt_separator: <!--more-->
---

<script src="https://d3js.org/d3.v4.js"></script>
<script src="https://d3js.org/d3-scale-chromatic.v1.min.js"></script>
<script src="https://d3js.org/d3-geo-projection.v2.min.js"></script>

Doing my analysis of [American Express's transfer bonus history]({% post_url 2020-04-12-amex-transfer-bonus-history %})
got me thinking about some of the uses for miles that are relatively rare in the blogosphere, but still might have
utility to someone, potentially with a transfer bonus. Flying Blue, the loyalty program of Air France and KLM (and Kenya
Airways and a few others) has a few interesting quirks.

<!--more-->

![]({{site.baseurl}}/assets/jl773.jpg)
_A Japan Airlines 777-300_

## Why Flying Blue?

I will preface this by saying that Flying Blue now does dyanmic pricing of awards, though there is generally a known
lowest dynamic price for a route. They also add pretty high fuel surcharges to their European awards, and pass on fuel
surcharges from other carriers. Most programs have some kind of sweet spot though, and Flying Blue has much better award
availability on their own flights than partner programs like Delta's does.

Let me also say that the gold standard for US to Japan awards in the blogosphere seems to be transfering American
Express points to Virgin Atlantic during a 30% bonus and booking ANA business or first class. From the eastern US,
Virgin charges 95,000 miles _round trip_ in business class and 120,000  miles _round trip_ in first class. It is
important to make an apples-to-apples comparison, so that would be 36,539 American Express points each way for a
business class ticket to Japan, which is amazing.

Generally, economy mileage redemptions are not a great value in most programs, so I will assume that business class and
first class are the default thing to redeem miles for.

## Booking Methods for Japan Airlines

But let's say for some reason you have your heart set on flying Japan Airlines, or have some obscure routing requirement
(like originating in a small US airport with expensive domestic flights, or wanting to fly to Osaka without taking an
intra-Asia flight, [which I did]({% post_url 2020-01-04-trips-planned %})). So what are your options for booking JAL?
Looking at a one-way business class ticket from North America to Tokyo (I've included the number of miles with Amex's
usual transfer bonus after the slash, and the number of miles necessary for one domestic connection):

| Frequent Flyer Program | Miles (nonstop)          | Miles (domestic connection) |
| ---------------------- |-------------------------:|----------------------------:|
| Japan Airlines         | 50,000                   | 50,000                      |
| American Airlines      | 60,000                   | 60,000                      |
| Alaska Airlines        | 60,000                   | 60,000                      |
| Cathay Pacific         | 75,000                   | 100,000                     |
| British Airways        | 108,250 / 77,322         | 123,250 / 88,036            |
| Air France / KLM       | 115,000 / 92,000         | 115,000 / 92,000            |
| Emirates               | 125,000                  | --                          |

Flying Blue is still a quite expensive option (I included Emirates to demonstrate that it is not the most expensive),
though not by much with a 25% transfer bonus and a connection. There is a certain confluence of events where that might
prove useful:

1. You can transfer points from any of the major transferrable point programs _(American Express, Chase, Citi, Capital
   One, and Marriott Bonvoy)_ to Flying Blue.
   [Award Hacker](https://www.awardhacker.com/#f=Boston&t=NRT&o=0&c=j&s=1&p=1) is a useful tool for figuring out which
   points transfer to which frequent flyer programs, as well as the standard rates that each program has for a given
   city pair.
2. You need to, or would prefer to, fly with a connection vs a non-stop flight.
3. You have a lot of points in one of the transferrable currencies and don't many have Japan Airlines, American, or
   Alaska miles. The only transferrable points program that transfers to those programs is Marriott Bonvoy, so they can
   be more difficult to obtain.

One interesting quirk here is that Flying Blue's US partner is Delta, while all of the other programs partner with
American Airlines and/or Alaska Airlines. So booking with Flying Blue gets you access to a quite
different route map for domestic connections. Additionally, four of the cities that Japan Airlines flies to are Delta
hubs (Boston, New York, Los Angeles, and Seattle). See the map below for the full list of Japan Airlines' North American
destinations.

<div class="svg-container" style="max-width: 1000px; margin: 1em auto;">
    <svg id="japan-map" data-hemisphere="pacific" data-label-points="true" style="width: 100%; max-width: 1000px;"></svg>
    <div style="font-style: italic; margin-bottom: 1em;">Japan Airlines US Destinations</div>
</div>

Given that the cost in miles for a Flying Blue booking is only competitive for bookings with a connection within
North America, Flying Blue is going to be most valuable if you do not start in one of the cities listed, or you cannot
find award availability on the nonstop flights from the city you want to start in. If you live in a city with expensive
domestic flights, having both British Airways and Flying Blue as an option could be very valuable.

Certain people (_*cough*_ crazy people like me) also like to fly interesting flights, and you could fly from Boston to Los
Angeles on Delta's 757 with lie flat seats in business class, and then from Los Angeles to Osaka on Japan Airlines. Were
you to book through another program, you would have to fly American or Alaska from Boston to Los Angeles, which does not
have lie flat seats.

## Other Flying Blue Quirks

There are a few other quirks worth mentioning that could be useful:

First, if you live in a city served by Air France, they allow routing a North America to Asia route via Europe (the long
way). Strangely, the cost in miles for this is lower for this than the transpacific routing at 95,000 miles, but it also
comes with $400 in taxes and fuel surcharges. Depending on your valuation of Flying Blue miles and/or transferrable
points, this could be a good deal, but it likely is not.

Second, if you are interested in flying Delta to Japan and then connecting on to somewhere within Japan or Asia, you
booking with Flying Blue allows you to connect from Delta on Japan Airlines, which could be useful because Delta does
not have a Japanese airline partner, and Japan Airlines flights can be quite expensive when booked with cash. However,
if you are a tourist, you might consider a [Japan Rail Pass](https://japanrailpass.net/en/), a
[JAL Japan Explorer Pass](https://www.jal.co.jp/world/en/world/japan_explorer_pass/lp/), or an
[ANA Experience Japan Fare](https://www.ana.co.jp/en/us/promotions/share/experience_jp/).

Third, they have a similar partnership with Qantas, though awards are expensive and availability is very tough to find.
Connections from Delta to Qantas in Asia have the potential to be valuable, and Qantas' Asia routes may have more award
availability, but it is still expensive and tough.

Lastly, for travel to Europe on Air France and KLM flights, they have
[promo awards](https://www.flyingblue.us/en/flights/promo-rewards), which can often be a good deal, but cannot get you
to Japan.

![]({{site.baseurl}}/assets/shrine.png)
_A Shinto Shrine in Japan_

## The Bottom Line

If you want to fly Japan Airlines and have a certain confluence of events, it's useful to keep Flying Blue in your
toolbox. This will be most useful for folks in cities without direct service on Japan Airlines and where a domestic
connection would be expensive to book with cash (like Philadelphia, Miami, or Atlanta) that has common flights to Delta
to a city served by Japan Airlines. However, if you have miles in a program like American or Alaska, you might consider
saving your transferrable points and using those (and consider
[whether the premium for first class is worth it]({% post_url 2020-01-08-japan-airlines-first %})).

<script>
const maps = {
    "japan-map": [
        ["TYO", 139.781111, 35.553333],
        ["BOS", -71.006388, 42.362944],
        "-",
        ["TYO", 139.781111, 35.553333],
        ["LAX", -118.408048, 33.942496],
        "-",
        ["TYO", 139.781111, 35.553333],
        ["SFO", -122.375416, 37.618806],
        "-",
        ["TYO", 139.781111, 35.553333],
        ["SEA", -122.311777, 47.449889],
        "-",
        ["TYO", 139.781111, 35.553333],
        ["ORD", -87.906596, 41.974522],
        "-",
        ["TYO", 139.781111, 35.553333],
        ["DFW", -97.037694, 32.897232],
        "-",
        ["TYO", 139.781111, 35.553333],
        ["JFK", -73.778692, 40.639928],
        "-",
        ["TYO", 139.781111, 35.553333],
        ["HNL", -157.920262, 21.317827],
        "-",
        ["TYO", 139.781111, 35.553333],
        ["KOA", -156.045630, 19.738766],
        "-",
        ["TYO", 139.781111, 35.553333],
        ["YVR", -123.183888, 49.194722],
        "-",
        ["KIX", 135.244072, 34.427306],
        ["LAX", -118.408048, 33.942496],
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
        .rotate([105, -70])
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
                .style("stroke", "#cc0000")
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
                .style("fill", "#cc0000")
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