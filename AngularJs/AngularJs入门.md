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
