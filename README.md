# orwebperfplaybok
### Image Optimization
Prefer mp4 to gif

jpeg
- no transparency

png
- easy transparency
- prefer 8 bit

svg masking
- put image inside svg
```
<svg preserveAspectRatio="xMinYMin">
<defs>
<mask id="canMask">
  <image width="550" height="550" xlink:href="img/png"></image>
</mask>
</defs>
<image mask="url(#canMask)" id="canTop" width="500" height="1000" xlink:href="cantop.jpg"></image>
</svg>
```

automated optimizers
format|plugin
---|---
svg|imagemin-svgo
gif|imagemin-giflossy
jpg|imagemin-jpgoptim
png|imagemin-pngquant
