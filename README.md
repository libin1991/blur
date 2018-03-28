    高斯模糊(毛玻璃效果)实现方式
      1：css
        .blur {	
            -webkit-filter: blur(10px); /* Chrome, Opera */
              -moz-filter: blur(10px);
                -ms-filter: blur(10px);    
                    filter: blur(10px);

            filter: progid:DXImageTransform.Microsoft.Blur(PixelRadius=10, MakeShadow=false); /* IE6~IE9 */
        }
      2：canvas StackBlur.js 借用
      3：svg 
    文选用方式 优势：比canvas库小，库源码算法较难懂；实现比 css 样式效果更好，之前遇到过该类需求filter大后 css边会变的模糊，效果较差
    step1：利用svg,高斯模模糊属性，生成图片的滤镜，高斯模糊效果，相关api参照http://www.w3school.com.cn/svg/svg_filters_gaussian.asp
    结构
    <svg width="100%" height="100%" version="1.1"
      xmlns="http://www.w3.org/2000/svg">
      <filter id="Gaussian_Blur">
        <feGaussianBlur in="SourceGraphic" stdDeviation="3" />
      </filter>
      <image xlink:href="xxx.jpg" x="" y="" height="" width="" style="filter:url(#Gaussian_Blur)></image>
    </svg>
    文档注释
      <filter> 标签的 id 属性可为滤镜定义一个唯一的名称（同一滤镜可被文档中的多个元素使用）
      filter:url 属性用来把元素链接到滤镜。当链接滤镜 id 时，必须使用 # 字符
      滤镜效果是通过 <feGaussianBlur> 标签进行定义的。fe 后缀可用于所有的滤镜
      <feGaussianBlur> 标签的 stdDeviation 属性可定义模糊的程度
      in="SourceGraphic" 这个部分定义了由整个图像创建效果
    step2:coding
    使用：
     var BlurFn = new Blur(document.querySelector('#container'),{
        url: 'timg.jpg',
        blurAmount: 20,
        duration: 500, 
        opacity: 1 ,
        overlayClass:'_class'
      })
      实例提供setBlurAmount，generateBlurredImage两个接口动态设置