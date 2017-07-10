# 判断是否为 WebP、APNG 格式

**WebP**

```javascript
(function() {
    var img = new Image();
    img.onload = function() {
        if (img.width > 0 && img.height > 0) {
            document.documentElement.className = "webp";
            window.webpSupport = true;
        }
    }
    img.onerror = function() {}
    img.src = "data:image/webp;base64,UklGRjoAAABXRUJQVlA4IC4AAACyAgCdASoCAAIALmk0mk0iIiIiIgBoSygABc6WWgAA/veff/0PP8bA//LwYAAA";
}());
```

**APNG**

```javascript
(function() {
    "use strict";
    var apngTest = new Image(),
    ctx = document.createElement("canvas").getContext("2d");
    apngTest.onload = function () {
        ctx.drawImage(apngTest, 0, 0);
        self.apng_supported = ctx.getImageData(0, 0, 1, 1).data[3] === 0;
    };
    apngTest.src = "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAACGFjVEwAAAABAAAAAcMq2TYAAAANSURBVAiZY2BgYPgPAAEEAQB9ssjfAAAAGmZjVEwAAAAAAAAAAQAAAAEAAAAAAAAAAAD6A+gBAbNU+2sAAAARZmRBVAAAAAEImWNgYGBgAAAABQAB6MzFdgAAAABJRU5ErkJggg==";
}());
```


