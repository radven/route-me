# What is interactivity? 

Interactivity is the ability to embed contextual data about locations into the map data sources themselves. For example, you could encode the country name, population size, and an image of the country's flag into the data source, such that users could touch the country and have the information presented to them by the application. 

Information about how to present the data -- for example, to bold the country name, format the population size according to the user's locale, and display the flag image -- can also be embedded into the data source. 

# How is this accomplished? 

Applications like [TileMill][1] can create maps that efficiently embed this data without manual intervention. For example, you could take a Shapefile of country borders with name data, add it as a layer to a TileMill map, choose it as the interactivity layer, and indicate that countries should show their respective name, bolded, on interactive events (mouseover on the web, perhaps a touch on iOS). The map gets rendered out to a format like [MBTiles][2], which enabled compact and discrete download -- and offline-capable use -- of the map data on a variety of platforms. 

# How is this really accomplished? 

Ok, glad you're still with us. For each map tile, a UTF-8 text "grid" is created in JSON, mapping individual pixels to individual UTF-8 characters. These characters are each coded to a given key, and a table is created linked keys and data values. Then the textual grids, which are already relatively space-efficient, are gzip-deflated in order to further reduce overhead. Additionally, a formatting template (in the [Mustache][3] template language or formerly, as a JavaScript function) is bundled along with the tiles. 

When a point is queried for interactive features, say, in response to a user action, the appropriate tile grid is retrieved, gzip-inflated, JSON-parsed, the character corresponding to the pixel is retrieved, UTF-8-decoded, and correlated to a dictionary of values, which are then run through the formatting template and returned as HTML. 

We've provided a behind-the-scenes [demo][4] of how this works on the web if you'd like to take a quick glance. 

All of this is accomplished in Route-Me by including `RMInteractiveSource.h`, checking a compatible tile source object against `- (BOOL)supportsInteractivity`, and querying it with `- (NSString *)formattedOutputOfType:forPoint:inMapView:` to get the HTML, which the application can then display. 

# Who is behind this and what are you selling? 

This library was built by [Development Seed][5], a creative data visualization and mapping team based in Washington, DC. We work on projects with other organizations while open sourcing as many of the technical tools as we are able. 

The tools are free -- those to design and host maps, those to display them, and those to bring them to mobile devices on iOS. We invite you to help us build our open source stack of [MapBox][6] tools on [GitHub][7] today! 

We also offer industrial-grade map hosting [services][8] and [support][9] for using our tools. 

[1]: http://mapbox.com/tilemill/
[2]: http://mbtiles.org/
[3]: http://mustache.github.com/
[4]: http://mapbox.com/demo/visiblemap/
[5]: http://developmentseed.org/
[6]: http://mapbox.com/
[7]: http://github.com/mapbox/
[8]: http://mapbox.com/hosting/
[9]: http://mapbox.com/support/