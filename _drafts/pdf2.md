## Printing transparency as vector pdf with a raster image underlay

- Image on the view:
  
  - Elemements with transparency get a white background

- Image on the sheet:
  
  - Elements become totally transpareny, like wireframe

- If both, view governs

### Solution

You can try the following:  

- If you only need this for printing, use raster printing. If you know 
  the resolution of the printer set the resolution to that dpi value in 
  your pdf printer settings, you won't loose quality. If you don't know 
  the resolution of the printer use 144 dpi, or if you need photo quality 
  300dpi. Most commercial printers use this resolutions  
- If you really want to use vector printing, you have two options:  
  - Place the image on the sheet, not on the view, and position it under 
    the viewport. If there is no image on the view, Revit will replace the 
    transparency with empty fill so the image will be visible under the 
    topography. Apply some white overlay or desaturation to the image before
    importing it to Revit, so you will have similar results. 
  - Trace the image in Revit if it's possible and use native Revit elements: fills and 
    detail lines. 

*Based on my [comment on Revit forum](https://revitforum.org/showthread.php/42830-Topo-surface-transparent-override-not-printing-with-Vector-(Hidden-Line-mode)?p=228719&viewfull=1#post228719)*
