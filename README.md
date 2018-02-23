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

format | plugin
 --- | --- 
svg | imagemin-svgo
gif | imagemin-giflossy
jpg | imagemin-jpgoptim
png | imagemin-pngquant

### Font Loading
reason
- Render blocking resource
- Content invisible while loading
- flash of invisible text(foit)

try critical foft
- async loads the full font family

reduce font size
- fontsqui.  create subset

[unicode generator](https://codepen.io/elifitch/pen/Ljqway)
in css
```
<link rel="preload" as="style" href="style.css"/>
<link rel="preload" as="font" type="font/woff2" href="fonts/brandon"/>
@font-face{
  unicode-range: U+30-39,U+41-5A,U+61-7A
}
.font-stage-2{
  font-family:'brandon subset';
}
```
use fontfaceobserver:
```
//fontfaceobserver
(function(){
  if(sessionStorage.criticalFontLoaded){
    document.documentElement.className+=' fonts-stage-2';
    return;
  }
  const font = new FontFaceObserver('brandon');
  Promise.all([font.load()]).load(()=>{
    sessionStorage.criticalFontLoaded=true;
    document.documentElement.className+=' fonts-stage-2';
    return;
  })
})()
```

### Animation Performance
pixel pipeline
- layout
- paint
- composite

layout
- takes longer the more elements on the page
- take care to not thrash
- avoid if possible

layout properties
- width height
- align-items justify-content
- font-size line-height
- margins padding
- top l r b

paint
- Rasterization
- faster than layout
- takes longer the more pixels there are to paint

paint properties
- background
- color
- box-shadow
- fixed position

composite
- Arrange pages into tiles and layers
- Uploads tiles and layers to the GPU
- Very fast,efficient

composite properties
- opacity
- transform
