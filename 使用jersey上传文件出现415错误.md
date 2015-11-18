###关于在项目中使用jersey上传文件出现415错误的解决办法

**问题描述**  
在项目开发的过程中，我想通过REST接口上传文件，在服务端使用FormDataMultiPart来接收表单参数。但是发送表单后得到的却是415错误：Unsupported Media Type

**解决办法**
在查阅网上资料后发现stackoverflow上有一个类似的问题并且提出了解决办法：  
在web.xml中添加jersey的初始化参数  
  
     <init-param>
            <param-name>jersey.config.server.provider.classnames</param-name>
            <param-value>org.glassfish.jersey.filter.LoggingFilter;org.glassfish.jersey.media.multipart.MultiPartFeature</param-value>
        </init-param>