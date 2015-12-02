###ng-calss
ng-class是AngularJS预设的一个指令，用于动态自定义DOM元素的css class name，在实际应用中有三种方式给元素的class属性值。   

**scope绑定变量（不推荐）**  


    function ctr($scope){
       $scope.test =“classname”;
     }

    <div class=”{{test}}”></div>

这种方式完全没错，是AngularJs改变class的一种方法，但是在controller涉及了classname总是很诡异，我们希望controller是一个干净的纯javascript意义，object。  

**字符串数组的形式**  
    function Ctr($scope) { 
        $scope.isActive = true;
    }

    <div ng-class="{true: 'active', false: 'inactive'}[isActive]"></div>  

**对象key/value处理**  
    function Ctr($scope) { 

    }

    <div ng-class {'selected': isSelected, 'car': isCar}"></div>   
当isSelected=true则增加selected class。当isCar=true,则增加car class。