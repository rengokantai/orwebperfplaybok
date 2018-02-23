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
  const font = new FontFaceObserver('brandon');
  Promise.all([font.load()]).load(()=>{
    document.documentElement.className+=' fonts-stage-2';
    return;
  })
})()
```
