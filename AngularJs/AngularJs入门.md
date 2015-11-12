###0x00
AngularJs是一个MV*框架，最适合用于开打客户端单页面应用。他专注于拓展HTML功能，提供动态数据绑定，而且能跟其他框架合作融洽。  

###0x01 angular.module
要定义AngularJs应用，首先要定义一个AngularJS模块。模块是很多函数的集合，当应用被启动时，这些函数就被执行。  
`var app = angular.module('myApp', [])`  
用以上代码，我么创建了一个"myApp"的AngularJs模块。  
接下来在页面实例化一个模块，使用ng-app这个指令，它告诉AngularJs我们的模块拥有哪一部分Dom结构。传入我们的应用名字作为ng-app指令的值，我么就可以在html页面上实例化我们的模块
`<html ng-app="myApp">`  
我们可以在任何DOM元素上设置ng-app，然后该元素就会成为AngularJs启动运行应用的地方。  
定义好应用之后，就可以开始创建其它部分。可以使用$scope来创建。  

###0x02 scope
$scope是一个把view(一个DOM元素)连接到controller上的对象，在MVC结构里，$scope将成为model，它提供一个绑定到DOM元素及其子元素上的可执行上下文(excecution context)。  

实际上$scope就是一个JavaScript对象，controller和view都可以访问他，所以可以利用scope在两者之间传递信息。在$scope对象中，既存储数据，又存储将要运行在view上的函数。  

每一个AngularJs应用都有一个$rootScope。这个$rootScope是最顶级的scope，它对应着含有ng-app指令属性的那个DOM元素。  

如果页面上没有明确设定$scope,Angular就会把数据和函数都绑定到这里。  

###0x03 ng-controller
要明确创建一个$scope对象，就要给DOM元素加上controller对象，使用的是ng-controller指令属性：  

    <div ng-controller="MyController">
      {{ person.name }}
    </div>
ng-controller指令给所在的DOM元素创建了$scope对象，并将这个$scope对象包含进外层DOM元素$scope里。 
 
    app.controller('MyController', function($scope) {
      $scope.person = {
        name: "Ari Lerner"
      };
    });
以后我们就可以在所有包含ng-controller="MyController"的DOM元素中访问到person对象。  
<<<<<<< HEAD

**互动**  
数据绑定不只限于数据，我们还可以利用绑定条用$scope中的函数。  
可以使用ng-click指令将DOM元素的鼠标点击事件绑定到一个方法上。

**Ajax**  
在动态的内容刷新应用程序中，使用$http来填充我们view。  
AngularJs原生支持Ajax，具有与一个或者多个服务器来回发送请求的能力。AngularJs通过一个服务来支持Ajax，这个服务就是$http服务。  
所有AngularJs的核心服务都用$前缀。   
$http服务机器灵活，给了我们很多方式来调用Ajax服务。简单地创建请求：  
 
    $http({
      method: 'JSONP',
      url: 'http://api.openbeerdatabase.com/v1/beers.json?callback=JSON_CALLBACK'
    }).success(function(data, status, headers, config) {
      // data contains the response
      // status is the HTTP status
      // headers is the header getter function
      // config is the object that was used to create the HTTP request
    }).error(function(data, status, headers, config) {
    });

$http 服务是这样一个函数：它接受一个设置对象，其中指定了如何创建HTTP请求；它将返回一个承诺（*参考JavaScript异步编程的promise模式），其中提供两个方法： success方法和error方法。   

###0x04 指令和表达式  
**指令**  
之前提到过指令属性，指令属性就是绑定在DOM元素上的函数，它可以调用方法，定义行为，绑定controller及$scope对象，操作DOM等。  
当AngularJs应用启动，Angular编译器就会遍历DOM树(从有ng-app的那个元素开始)，解析HTML，寻找这些指令函数。  
当在一个DOM元素上找到一个或者多个指令属性函数，他们就会被收集起来，排序，然后按照优先级顺序被执行。   

**表达式**  
要先理解指令的运作就要先理解表达式，表达式形如：`{{ person.name }} 和 {{ clock }}和{{ 10 * 3.3 | currency }}$33.00`。  其中还有过滤器。  
表达式有点像eval(javascript)的结果，他们经过AngularJs的处理会拥有一些重要而独特的性质：   

- 所有表达式都在scope这个context里执行，因此可以使用所有本地$scope中的变量。
- 如果一个表达式的执行到时类型错误或者引用错误，这些错误将不会被抛出。
- 表达式里不允许任何控制函数流程的功能比如：if.else.
- 表达式可以接受一个或者多个串联起来的过滤器。    

表达式都运行在他们的scope里，所以一个表达式可以访问并操作scope上的一切。因此可以使用表达式遍历其scope的属性，调用scope里的函数，或者对scope中的变量进行数学运算。   

**ng-init**  
ng-init 指令属性是一个在启动时运行的函数，它让我们能在程序运行前设定初始变量的值:  

	
`<b ng-init='name = "Ari Lerner"'>Hello, {{ name }}</b>`  

**ng-click**  
ng-click指令属性给DOM元素注册一个点击事件监听器。当次元素上有点击事件发生时，AngularJs就会执行表达式的内容并且相应更新view。  

    <button ng-click="counter = counter + 1">Add one</button>
    Current counter: {{ counter }}  

**ng-show/ng-hide**  
ng-show/ng-hide指令根据表达式的真值来决定显示还是隐藏DOM元素。  

    <button ng-init="shouldShow = true" ng-click="shouldShow = !shouldShow">Flip the shouldShow variable</button>
     <div ng-show="shouldShow">
      <h3>Showing {{ shouldShow }}</h3>
     </div> <div ng-hide="shouldShow">
      <h3>Hiding {{ shouldShow }}</h3>
     </div>

**ng-repeat**  
ng-repeat指令遍历一个数据集合中的每个数据元素，加载HTML模板把数据渲染出来。 

我们还可以创建自己的指令，参见：http://www.ng-newsletter.com/posts/directives.html  
创建自定义指令属性，可以使用app对象的directive方法：  

    app.directive（'nprlink', function() {
		return{
		  restrict:'EA',
          require:['^ngModel'],
          replace:true,
		  scope:{
             ngModel:'=',
             play:'&'
	      },
          templateUrl:'/views/nprListItem.html',
          linkL: function(scope,ele,attr){
             scope.duration = scope.ngModel.audio[0].duration.$text;
          }
		}
	}）;  

###0x05 服务  
从内存和效率的角度出发，controller仅当需要的时候才会被实例化，并在不需要的时候被丢弃，这就意味着每一次我们使用route跳转或者重载视图当前的controller会被销毁。  
Service可以让我们在整个应用的生命周期中保存数据并可以让controller之间共享数据。  
Service都是单例的就是说在一个应用中，每一个Service只会被实例化一次（使用$injector服务），主要负责提供一个接口把特定函数需要的方法放在一起，例如，$http Service提供了访问底层浏览器的XMLHttpRequest对象方法，相较于直接使用XMLHttpRequest，$http API使用更简单。  

AngularJs内建了许多服务供日常使用，这些服务对于在复杂应用中建立自己的Service是很有用的。  

AngularJs让我们可以轻松创建自己的service，仅仅是注册service即可，一旦注册Angular编译器就可以找到并加载他作为依赖，供程序运行时使用，最常见的创建方法就是用angular.module API的factory模式：  

     angular.module（'myApp.service',[]）
       .factory('githubservice', function() {
        var serviceInstance = {};
        //我们的第一个服务
        return serviceInstance；
       })；

当然也可以使用内建的$provider service来创建service。  
以上的服务并没有做实际的事情，只是展示如何定义一个service，下面加一些实际意义的代码：  

    angular.module('myApp.service',[])
      .factory('githubService', ['$http',function(http){
        var doRequest = function(username, path){
          return $http({
            method:'JSONP',
            url:'https://api.github.com/users/' + username + '/' + path + '?callback=JSON_CALLBACK'
          });
        }
        return {
          events:function(username){return doRequest(username, 'events')},
        };
        }]);

我们创建了一个只有一个方法的githubservice，events可以获取到给定的github用户最新的hithub时间，为了把这个服务添加到controller中，建立一个controller并加载或者注入githubService作为运行依赖，我们把service的名字作为参数传递给controller函数（使用中括号[]）:

    app.controller('ServiceController', ['$scope','githubService', 
		function($scope, githubService){
    }]);
这种依赖注入写法对于js压缩是安全的。一旦githubService注入到ServiceController中，我们就可以像使用其他服务一样使用githubService。   

注意我们要遵守Angular services依赖注入的规范：自定义的service要写在内建的Angular services之后，自定义的service之间是没有先后顺序的。  

###0x06 Routing
在单页面应用中，视图之间的跳转显得尤为重要，需要一种方法精确控制什么时候应该呈现怎么样的页面给用户。  

我们可以在主页引入不同的模板来支持不同的页面的切换，缺点就是越来越多的内嵌代码导致难以管理。  

通过ng-include指令我么可以把很多模板整合在视图中，但是我们有更好的方法来处理这种情况，我么可以把视图打散成layout和模板视图，然后根据用户访问特定的url来呈现需要的视图。  

AngularJs通过在$routeProvider（$route服务提供者）上声明routes来实现。使用$routeProvider。我们可以更好的利用浏览历史的API并且可以让用户把当前路径存成书签以方便以后访问。  

设定路由需要做两件事：  

- 需要指出我们存放将要存放新页面内容的布局模板在哪里。
- 需要配置我们的路由信息，配置$routeProvider。

$routeProvider提供了两种方法处理路由：when和otherwise，方法when接收两个参数，第一个设置$location.path（）；第二个参数是配置对象，这个可以包含不同的键。

**controller**
    
    controller: 'MyController'
    // or
    controller: function($scope) {
  
    // ...
    }

如果在配置对象中设置了controller属性，那这个controller会在route加载的时候实例化，这个属性可以是一个字符串(必须在module中注册过的controller)也可以是controller function。  

**Template模板**  
`template: '<div><h2>Route</h2></div>'`  
如果我们在配置对象的template属性设置了值，那么模板就会渲染到DOM的ng-view中。

**templateUrl**  
`templateUrl: 'views/template_name.html'`  
如果我们在配置对象的templateUrl属性中设置了值，AngularJs将通过XHR来获取改模板内容渲染到DOM的ng-view处。  
值得注意的是：templateUrl属性跟其他AngularJS XHR请求的处理流程是一样的，也就是说，即使用户从这个页面离开，等他再回到这个页面，应用不会再去请求这个模板页面，因为$templateCache已经缓存了这个模板。  

**添加路由**  

    angular.module('myApp',[])
      .config(['$routeProvider', function($routeProvider){
        $routeProvider.when('/',{
          controller:"HomeController",
          template:'<h2>We are home</h2>'
         })
         .otherwise({redirectTo:'/'});
      }]);

$routeProvider还可以处理URL里传递的参数（比如，/people/42, 假设42是我们要找的people的id号）只需要简单在字符串前加上':',$routeProvider会尝试匹配URL中id并把id作为key在$routeParams服务中使用


     $routeProvider.when('/person/:id', {
       controller: 'PeopleController',
       template: '<div>Person show page: {{ name }}</div>'
      })

**过滤器**  
在AngularJs中，filter提供了一种格式化数据的方法，Angular也提供很多内建的过滤器，并且建立自定义过滤器很简单。  
在Html的模板绑定{{}}中，使用|来调用过滤器。  

传递参数到filter，只需要把参数放在filter之后，中间价一个冒号。  

**currency**

Currency过滤器主要是把数字格式化成货币，意思就是123格式化以后就成了$123.00

Currency可以根据需要选择适当的货币符号。默认的是根据当前操作系统的locale来转换的  

**date**  

日期过滤器主要根据我们提供的格式化形式来格式化日期，他提供了很多内建的选项，如果没有指定格式，默认显示mediumDate形式

下面是一些自带的日期格式化形式，我们可以通过把不同的格式化选项组合使用来创建自定义的日期格式化形式：  
![](http://i.imgur.com/tIF7hcB.png)  

**filter**  
filter过滤器主要用来过滤一个数组并返回包子数组数据的新数组，比如在客户端搜索是，我们可以快速的从数组中过滤我们想要的结果。filter方法接收一个string，object，或者function参数用来选择/移除数组元素  
如果第二个参数传递的是：  
 true 	 执行严格的匹配比较（跟’angular.equals(expected,actual)一样）  
false	 执行大小写敏感的substring匹配  
Function 执行function并接受一个元素，前提是这个function的返回结果是真  

**json**  
json过滤器接收JSON或者Javascript对象，然后转换为字符串，这个功能在调试过程中非常有用。  

**limitTo**  
limitTo过滤器会根据传递的参数值来生成新的数组或字符串，参数值为整数，从开头截取，参数为负值，从最后开始截取  
如果限定值超过了字符串长度，返回整个数组或字符串  

**lowercase**  
lowercase过滤器很明显，将整个字符串编程小写形式  

**number**  
Number过滤器格式化文本成数字，可以接受参数（可选）来决定格式化后数字的位数  
如果参数是非数字，将返回空字符串  

**orderBy**  
orderBy过滤器主要是根据给定的表达式对数组进行排序  
orderBy函数可以接受两个参数：第一个是必须要提供的，第二个是可选参数。  

第一个参数决定了如何对数组进行排序，如果第一个传入的参数是：
function：将被用作这个对象的‘getter‘函数  
string：字符串会被作为key来对数组元素进行排序，你也可以传进来 ‘+’ 或者‘-‘来决定是升序还是降序  
array：使用这个数组里的元素作为排序表达式的判断依据，使用第一个不严格相等表达式的结果的元素作为其他元素的判断依据  
第二个参数控制数组的顺序。  

**自定义过滤器**  
创建自定义过滤器箱单简单，我们只要把他配置到我们的module下就可以了，首先创建一个module  
    
    angular.module('myApp.filters',[])
      .filter('capitalize', function(){
        return function(input){
          if (input) {
            return input[0].toUpperCase() + input.slice(1);
         }  
    });  
Fliters其实就是一个function，接收input 字符串，我们可以函数里做一些错误检查  
=======
除了一个例外,所有的scope都遵循原型继承,这意味着他们都能访问到付scope们。对任何属性和方法，如果AngularJs在当前scope上找不到，就会到父scope上去找，如果在父scope上也没找到，就会继续上溯，一直到$rootscope。唯一的例外：有些指令可以选择性创建一个独立的scope，让这个scope不继承它的父scope们。

####0x04 数据绑定和Ajax
双向绑定是指如果model改变了，view将会看得到这个改变，如果view改变了，model同样看得到。要想了解其中的原理需要了解digest_loop的运作。  
要建立双向绑定，可以在DOM上使用ng-model指令属性。  

    <div ng-controller="MyController">
     <input type="text" ng-model="person.name" placeholder="Enter your name" />
     <h5>Hello {{ person.name }}</h5>
    </div>

>>>>>>> faea77856d9d2a08394b067b88d92c889b50af46
