###自定义指令  

指令的作用：实现语义的标签化   

我们常用的HTML标签是这样的：   

     <div>
        <span>一点点内容</span>
     </div>  

而使用AngularJs的指令机制我们可以实现自定义指令：  

    <tabpanel>
        <panel>子面板1</panel>
        <panel>子面板2</panel>
    </tabpanel>  

**实例1，定义一个简单的标签**  
	
	<body>
        <hello></hello>
    </body>

对于<hello>，浏览器是不认识的，使用AngularJs自定义一个hello指令，浏览器就能够识别了   

	var appModule = angular.module('app', []);
	appModule.directive('hello',function(){
		return {
			restrict:'
		}
	}); 


Scope的特性  
scope提供$watch方法监视Model变化  
scope提供$apply方法传播Model的变化