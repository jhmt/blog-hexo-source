title: AngularJS $resource in TypeScript
date: 2015-11-26 19:19:47
categories: [JavaScript, TypeScript]
tags: [AngularJS, TypeScript]
---

AnuglarJS's $resource service is available in TypeScript with adding reference for <a href="https://github.com/DefinitelyTyped/DefinitelyTyped">DefinitelyTyped</a> file.

<pre class="brush: js;">
/// <reference path="typings/angularjs/angular.d.ts" />

var productRepository = $resource('http://localhost:44312/api/products/:id', { id: '@Id' });
$scope.products = productRepository.query();
</pre>

### Enable Intellisense

Intellisense for properties are enabled according to the defined type.

<pre class="brush: js;">
/// &lt;reference path="typings/angularjs/angular.d.ts" />

interface Product {
  Id: number;
  Name: string;
  Price: number;
}

app.controller('myCtrl', function ($scope, $resource) {
    var productRepository = $resource&lt;Product>('http://localhost:44312/api/products/:id', { id: '@id' });
    
    $scope.updatePrice = function(productId, newPrice){
        var product = productRepository.get({ id: productId });
        // Intellisense enabled
        product.Price = newPrice;
        product.$save();
    };
}
</pre>

However, intellisense for $save, $remove and other $resource methods are disabled in this way.

### Enable Intellisense for both properties and $resource methods

Intellisense for both properties and $resource methods will be enabled for a type derived from <a href="http://definitelytyped.org/docs/angularjs--angular-resource/interfaces/ng.resource.iresourceclass.html">IResourceClass<T></a>.
IResourceClass<T> is generic and T must be specified as **derived class name ("Product")**.

<pre class="brush: js;">
interface Product extends IResourceClass&lt;Product>{
  Id: number;
  Name: string;
  Price: number;
}
</pre>

