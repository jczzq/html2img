# html2img
## 内容
html内容通过canvas转化为图片
<pre>
    var app = {
        //设置监听事件
        setListener: function () {
            this.htmlToCanvas(document.querySelector('#content'));
        },
        //获取像素密度
        getPixelRatio: function (context) {
            var backingStore = context.backingStorePixelRatio ||
                context.webkitBackingStorePixelRatio ||
                context.mozBackingStorePixelRatio ||
                context.msBackingStorePixelRatio ||
                context.oBackingStorePixelRatio ||
                context.backingStorePixelRatio || 1;
            return (window.devicePixelRatio || 1) / backingStore;
        },
        //绘制dom 元素，生成截图canvas
        htmlToCanvas: function (dom) {
            var shareContent = dom;// 需要绘制的部分的 (原生）dom 对象 ，注意容器的宽度不要使用百分比，使用固定宽度，避免缩放问题
            var width = shareContent.offsetWidth;  // 获取(原生）dom 宽度
            var height = shareContent.offsetHeight; // 获取(原生）dom 高
            var offsetTop = shareContent.offsetTop;  //元素距离顶部的偏移量

            var canvas = document.createElement('canvas');  //创建canvas 对象
            var context = canvas.getContext('2d');
            var scaleBy = this.getPixelRatio(context);  //获取像素密度的方法 (也可以采用自定义缩放比例)
            canvas.width = width * scaleBy;   //这里 由于绘制的dom 为固定宽度，居中，所以没有偏移
            canvas.height = (height + offsetTop) * scaleBy;  // 注意高度问题，由于顶部有个距离所以要加上顶部的距离，解决图像高度偏移问题
            context.scale(scaleBy, scaleBy);

            var opts = {
                allowTaint: true,//允许加载跨域的图片
                tainttest: true, //检测每张图片都已经加载完成
                scale: scaleBy, // 添加的scale 参数
                canvas: canvas, //自定义 canvas
                logging: false, //日志开关，发布的时候记得改成false
                width: width, //dom 原始宽度
                height: height //dom 原始高度
            };
            html2canvas(shareContent, opts).then(function (canvas) {
                canvas.setAttribute('id', 'thecanvas');
                document.getElementById('images').appendChild(canvas);
            });
        }
    };
</pre>

## 引用插件
[niklasvh/html2canvas](https://github.com/niklasvh/html2canvas)

