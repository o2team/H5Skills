# 使用 [exif.js](http://code.ciaoca.com/javascript/exif-js/) 和 [html2canvas](http://html2canvas.hertzen.com/) 遇到的一些坑

## 将拍摄的图像绘制到 Canvas 后，图像方向不对

在微信中使用 `<input type="file" multiple accept="image/*">` 传入照片后，绘制到 Canvas 中时会发现绘制的图像方向不对（手 Q 端貌似不会存在这个问题），这时需要使用 exif.js 来解决。

> Exif.js 提供了 JavaScript 读取图像的原始数据的功能扩展，例如：拍照方向、相机设备型号、拍摄时间、ISO 感光度、GPS 地理位置等数据。

```html
<input class="uploadBtn" type="file" multiple accept="image/*">
```

```javascript
document.querySelector(".uploadBtn").addEventListener("change", previewImgFile, false);

function previewImgFile() {

    var _files = files || event.target.files;
    var _index = index || 0;
    var reader = new FileReader();

    reader.onload = function(event) {

        var image = new Image();
            image.src = event.target.result;

        var orientation;

        image.onload = function() {

            EXIF.getData(image, function() { // 获取图像的数据

                EXIF.getAllTags(this); // 获取图像的全部数据，值以对象的方式返回
                orientation = EXIF.getTag(this, "Orientation"); // 获取图像的拍摄方向

                var rotateCanvas = document.createElement("canvas"),
                    rotateCtx = rotateCanvas.getContext("2d");

                // 针对图像方向进行处理
                switch (orientation) {

                    case 1 :
                        rotateCanvas.width = image.width;
                        rotateCanvas.height = image.height;
                        rotateCtx.drawImage(image, 0, 0, image.width, image.height);
                        break;
                    case 6 : // 顺时针 90 度
                        rotateCanvas.width = image.height;
                        rotateCanvas.height = image.width;
                        rotateCtx.translate(0, 0);
                        rotateCtx.rotate(90 * Math.PI / 180);
                        rotateCtx.drawImage(image, 0, -image.height, image.width, image.height);
                        break;
                    case 8 :
                        rotateCanvas.width = image.height;
                        rotateCanvas.height = image.width;
                        rotateCtx.translate(0, 0);
                        rotateCtx.rotate(-90 * Math.PI / 180);
                        rotateCtx.drawImage(image, -image.width, 0, image.width, image.height);
                        break;
                    case 3 : // 180 度
                        rotateCanvas.width = image.width;
                        rotateCanvas.height = image.height;
                        rotateCtx.translate(0, 0);
                        rotateCtx.rotate(Math.PI);
                        rotateCtx.drawImage(image, -image.width, -image.height, image.width, image.height);
                        break;
                    default :
                        rotateCanvas.width = image.width;
                        rotateCanvas.height = image.height;
                        rotateCtx.drawImage(image, 0, 0, image.width, image.height);

                }

                var rotateBase64 = rotateCanvas.toDataURL("image/jpeg", 0.5);

            });

        }

    }

    reader.readAsDataURL(_files[_index]);

}
```

## html2canvas 使用过程中遇到的问题

**1、图片模糊**

默认生成的 Canvas 是 1 倍图，在移动端 Retina 屏幕下显示模糊，可以通过 2 倍图的方式解决：

```javascript
var stageWidth = $(".stage").width(),
    stageHeight = $(".stage").height();

var retinaCanvas = document.createElement("canvas");
retinaCanvas.width = stageWidth * 2;
retinaCanvas.height = stageHeight * 2;
retinaCanvas.style.width = stageWidth + "px";
retinaCanvas.style.height = stageHeight + "px";

var ctx = retinaCanvas.getContext("2d");
ctx.scale(2, 2);

html2canvas(document.querySelector(".stage"), {
    canvas: retinaCanvas,
    onrendered: function(canvas) {
        var base64 = canvas.toDataURL(); // 返回 base64
    }
});
```

**2、图片偏移**

html2canvas 是解析 DOM 元素实际的样式来生成图片，如果对元素位置进行调整，例如设置了 `top`、`left` 或者 `margin-top`、`margin-left` 的样式会导致生成的图片偏移。

```javascript
ctx.transform(-x, -y); // 计算一下偏移的值，通过 transform 方法来修正回去
```

**3、ICON 模糊**

即使是使用了 2 倍图，生成后的图片里的 ICON 还是模糊，可以用 SVG 的方式去代替图片 ICON。

**4、不支持设置 border-radius 的元素**

![](http://storage.360buyimg.com/mtd/home/border1492773921827.png)

![](http://storage.360buyimg.com/mtd/home/html2canvas_border1492774034788.png)

如果你需要在一个头像中设置 `border-radius: 50%` 在转为图片后，你会发现并不成功，解决方法在源码 `html2canvas.js` 中加入以下代码：

```
tlh = borderRadius[0][0],
tlv = borderRadius[0][1],
trh = borderRadius[1][0],
trv = borderRadius[1][1],
brh = borderRadius[2][0],
brv = borderRadius[2][1],
blh = borderRadius[3][0],
blv = borderRadius[3][1];

/* S 插入这段代码 */
var halfHeight = Math.floor(height / 2);

tlh = tlh > halfHeight ? halfHeight : tlh;
tlv = tlv > halfHeight ? halfHeight : tlv;
trh = trh > halfHeight ? halfHeight : trh;
trv = trv > halfHeight ? halfHeight : trv;
brh = brh > halfHeight ? halfHeight : brh;
brv = brv > halfHeight ? halfHeight : brv;
blh = blh > halfHeight ? halfHeight : blh;
blv = blv > halfHeight ? halfHeight : blv;
/* E 插入这段代码 */

var topWidth = width - trh,
    rightHeight = height - brv,
    bottomWidth = width - brh,
    leftHeight = height - blv;
```

