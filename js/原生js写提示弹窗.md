### 插件思路：
1、可执行的闭包格式，防止变量混淆；定义弹框函数，并引入外部传入的参数；初始化执行在body内部加入弹框的div代码
```
    (function() {
    var tipAlert = function(options) {
        this.options = {
            "id": 'body',
            "content": options.content ? options.content : '错误信息',
            "leftPx": 0
        };
        this.init();
        };    
        tipAlert.prototype.init = function() {
            let tipE = document.createElement("div");
            tipE.id = "toast_data_short";
            tipE.className = "toast_data_short";
            document.body.appendChild(tipE);
        };
    window.tipAlert = tipAlert;
    })();
```
2、向弹框加入内容并显示，显示3s后隐藏。函数的prototype属性增加相应的方法，最终完整代码如下
```
    (function() {
    var tipAlert = function(options) {
        this.options = {
            "id": 'body',//弹框加入的位置
            "content": options.content ? options.content : '错误信息',//提示信息
            "leftPx": 0//弹框距离左侧的像素（保证居中）
        };
        this.init();//初始化，写入弹框html代码
        this.Tip();//调用展示弹框，3s后自动消失
        };    
        //创造弹框
        tipAlert.prototype.init = function() {
            let tipE = document.createElement("div");
            tipE.id = "toast_data_short";
            tipE.className = "toast_data_short";
            document.body.appendChild(tipE);
        };
        //显示弹框
        tipAlert.prototype.showTip = function() {
            let leftpx = this.options.leftPx;
            let tipE = document.getElementById("toast_data_short");
            tipE.style.zIndex = 12;
            tipE.style.left = leftpx + "px";
            tipE.style.opacity = 0.85;
        };
        //隐藏弹框
        tipAlert.prototype.hideTip = function() {
            let tipE = document.getElementById("toast_data_short");
            tipE.style.zIndex = -12;
            tipE.style.opacity = 0;
        };
        //调用
        tipAlert.prototype.Tip = function() {
            let tipE = document.getElementById("toast_data_short");
            tipE.innerHTML = this.options.content;
            this.options.leftPx = this.count_leftpx();
            this.showTip();
            window.setTimeout(this.hideTip, 3000);
        };
        //计算距离左侧距离
        tipAlert.prototype.count_leftpx = function() {
            let tipE = document.getElementById("toast_data_short");
            var toast_w = tipE.offsetWidth;
            var body_w = document.body.clientWidth;
            var leftpx = (body_w - toast_w) / 2;
            return Math.floor(leftpx);
        };
    window.tipAlert = tipAlert;
    })();
```
使用方式：在html页面引入tip.js文件
```
    <!-- 引入tip.js -->
        <script src="assets/js/plug/tip.js" type="text/javascript "></script>
        <script type="application/javascript">
            var errorMessage = "[[${errorMessage}]]";//错误信息
            new tipAlert({
                "content": errorMessage
            })
        </script>
```