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
###### 02:30 pixel pipeline
- layout
- paint: resterize and painting element
- composite: tuen image data to textures pass GPU to display

###### 03:40
layout (reflow)
- spatial relationships
- takes longer the more elements on the page
- take care to not thrash
- avoid if possible



layout properties
- width height
- align-items justify-content
- font-size line-height
- margins padding
- top left right bottom

paint
- Rasterization
- Generally faster than layout
- Takes longer the more pixels there are to paint

paint properties
- background
- color
- box-shadow
- fixed position

###### test.
in Chrome, Rendering-> Paint Flashing

composite
- Arrange pages into tiles and layers
- Uploads tiles and layers to the GPU
- Very fast,efficient

composite properties
- opacity
- transform

###### 13:00
avoid repaint for fixed
```
will-change: transform
transform: translateZ(0)
```
summary
- DONOT animate layout properties
- Animate using transform and opacity



### Critical Loading
###### 01:22
render blocking resources:
- link rel
- script src
- fonts

type of css
- critical: inline
- less:
```
<link rel="preload" as="style" type="text/css" href="" onliad="this.rel='stylesheet'">
```
(this wont block rendering)
current using filament loadcss as polyfill.  

tools:
- webpack-plugin-critical
- gulp critical


### Script Loading
async:
- js execution will block html parsing  

defer
- js execution will not block html parsing  

when to use:
async: many small scripts dont rely on each other(download in parallel)
defer: small rely on big js file(download in series)

### Code Minification
gzipping
- Replaces repeated characters with pointers to first instance
- Browsers can read files compressed with zip

### Lazy Loading
image loader, version 1 naive.
```
const targets = document.querySelectorAll('img[data-src]');
function lazyLoadImg(el){
  el.src=el.getAttribute('data-src');
}
targets.forEach(el=>{lazyLoadImg(el);})
```
image loader, version 2 IntersectionObserver
```
const targets = document.querySelectorAll(img[data-src]');
function createLazyLoadObserver(){
  const observer = new IntersectionObserver(function(){
    console.log()
  })
  targets.forEach(el=>{
    observer.observe(el);
  });
}

function lazyLoadImg(el){
   el.src=el.getAttribute('data-src');
}

function lazyLoadManager(entries){
  entries.forEach(entry=>{
    if(entry.intersectionRatio>0){
      lazyLoading(entry.target);
    }
  })
}
createLazyLoadObserver();

```


### Loading Indicators
skeleton screen
