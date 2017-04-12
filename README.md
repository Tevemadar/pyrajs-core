# PyraJS-core
Tiny JavaScript component for displaying 2D pyramidal image sources in HTML5 canvas.

** Basic usage: **

```new Zoomer(canvas,config).fullcanvas();```

Where ```canvas``` is a HTML5 canvas element (e.g. ```document.getElementById("mycanvas")```), and ```config``` is a DeepZoom-resembling configuration object:
- ```TileSize```: length of tile edges in pixels. Tiles are treated as ```TileSize*TileSize``` squares
- ```Width```: width of image in pixels
- ```Height```: height of image in pixels
- ```MaxLevel```: number of the maximum downscale level available. Levels are numbered from 0 (original size). Tiles from ```MaxLevel``` are requested by the component
- ```Key(level,x,y)```: function for generating a tile key (string) for given downscale ```level``` (```0...MaxLevel```) and tile column(```x```), tile row(```y```). Columns and rows are numbered from 0. The component asks for existing tiles only. Implementations are expected to generate an URL or partial URL here
- ```Load(key,x,y,next)```: function for loading a tile, based on its ```key``` (generated by ```Key``` above). Tile column(```x```) and row(```y```) coordinates are provided for convenience only. Load is expected to invoke ```next``` with a drawable HTML5 element ([```CanvasImageSource```](https://html.spec.whatwg.org/multipage/scripting.html#image-sources-for-2d-rendering-contexts)), typically with dimensions ```TileSize*TileSize```
- [```FillStyle```]: optional style/color for filling the portions of ```canvas``` which are not covered by the image. Defaults to opaque white (```"#FFFFFF"```).

Note on DZI use-cases:
- ```Overlap``` is not used by PyraJS, as ```Load()``` is expected to provide the actual ```TileSize*TileSize``` data without extra nonsense. ```x``` and ```y``` can help with handling the overlay properly. See example
- ```Format``` is not used by PyraJS either, as ```Load()``` is expected to provide the actual image/canvas/etc. However ```Key()``` or ```Load()``` will typically need ```Format```

** Interface **

PyraJS exports the following methods:
- ```fullcanvas()```: aligns image to fill the actual canvas. Typically used for starting PyraJS and in this core version this is the only method for handling when the canvas is resized
- ```redraw()```: redraws the image, it can be used after switching image sources (but keeping image dimensions)
- ```mdown(event), mup(event), mmove(event), mwheel(event)```: mouse event handlers.