title: Call ASP.NET WebApi from AngularJS
date: 2015-11-24 14:33:23
categories: [JavaScript, AngularJS]
tags: [WebAPI, AngularJS]
---

AngularJS's **$resouce** service makes integrating WebAPI with client applications easier.
This post introduces an example of ASP.NET WebAPI as follows.

### Implement ASP.NET WebAPI 
ProductsController.cs
<pre class="brush: c-sharp;">
public class ProductsController : ApiController
{
    // GET api/products
    public IEnumerable&lt;Product> GetProducts()
    {
        return db.Products.AsEnumerable();
    }
}
</pre>
<br/>
Web.config
<pre class="brush: xml;">
&lt;system.webServer>
    &lt;httpProtocol>
        &lt;customHeaders>
            &lt;add name="Access-Control-Allow-Origin" value="*" />
            &lt;add name="Access-Control-Allow-Headers" value="Content-Type" />
            &lt;add name="Access-Control-Allow-Methods" value="GET, POST, PUT, DELETE, OPTIONS" />
        &lt;/customHeaders>
    &lt;/httpProtocol>
&lt;system.webServer>
</pre>

#### $resouce service
JavaScript file (using AngularJS)
<pre class="brush: js;">
var productRepository = $resource('http://localhost:44312/api/products/:id', { id: '@Id' });
// Call "GET /api/products"
$scope.products = productRepository.query();
</pre>

View html
<pre class="brush: xml;">
&lt;ul>
    &lt;li ng-repeat="product in products">&#123;&#123;product.name}}&lt;/li>
&lt;/ul>
</pre>

### $save() and $remove() methods
**productRepository.query()** returns an array of deserialized objects. 
AngularJS adds CRUD methods ($save, $remove, etc...) to these objests.

For example:

<pre class="brush: js;">
product.name = "New Name";
product.$save();
</pre>

**$save()** sends "POST /api/products/1" request with setting "{"id":1,"name":"New Name"}" to the request body.
Similarly, **product.$remove()** sends "DELETE /api/products/1" request.
