# Introduction

# General Techniques
## Implementing a Controller Object
### Problem:
You have a large application and want to avoid placing logic in routes and views.
### Solution: 
A possible solution for this is to introduce a supervising controller object.  The controller can be a simple object that exposes methods or actions that will be invoked based on the current route.
```javascript
var controller = {
  index : function(){
    var view = new indexView();
    view.render($('#content'));
  },
  detail : function(albumId){
    var model = new AlbumModel({albumId:albumId});
    model.fetch({success:function(response, status, xhr){
      var detailView = new DetailView({model : model});
      detailView.render($('#content'));
    }})
    
  }
};
```

### Dynamic Controller Invocation
#### Problem
Your routes are basically repeating the same logic - creating a controller object, then calling the action method, you want something a little more DRYful.
#### Solution 
If you are using a controller like object, it is farily trivial to implement a standard routing system that works like some of the server side frameworks such as ASP.NET MVC.  In the following example, we implement a dynamic route based on the url where the first part of the path defines the controller and the second part defines the action.  

```javascript
crossroads.addRoute('/{controller}/{action}/{args}', function(controller, action, args){
  require([controller], function(ControllerObject){
    var myController = new ControllerObject();
    myController[action] && myController[action].call(myController, args);
  });
});
```

Note, the above implementation uses crossroads as opposed to the standard backbone router. Mixing and matching components like this is not discouraged.  Backbone's flexibility allows you to architext a solution which best fits your needs. 

Also note that we are using requirejs to pull in the controller module, we then invoke the controller's action method using the call method, this allows us to define the controller as the 'this' variable.

## Dependency Injection
I highly reccomend you consider using a module loader such as RequireJS.
## Using Mixins
## Decorating instance methods/functions
### Problem:
You want to create a view that logs its output to the console whenever it's rendered, but you want it to automatically happen whenever render is called on a base view.  You don't want the developer to have to invoke the method excplicity in render.
### Solution:
Place render function in a temporary function, then call temporary function from within base render call.  Remember that if you require the views html to be present 
```javascript
//decorate render call so we can add the validation binding
view.oldRender = view.render; //store instance render (user provided render) in a property
view.render = function () { //now implement the base render method 
view.oldRender.apply(view, arguments);
//provide your decoration logic here
console.log('content is ' + this.$el.html());
});
```
# Model Techniques
## Types of Models
### View Models
### Domain Models
## Using Backbone with Non-Restful services
### Overriding Backbone.sync
## Modelbinding
Modelbinding is a concept where your model's attributes are automatically bound to elements on the view.  Modelbinding offers several benefits:
* eliminates need for tedious event wiring logic to keep view and model in sync.
* reduces total amount of code in application.
* encapsulates event wiring and model to view sync logic

### Model to view model binding
This is typically taken care of via a template library such as handlebars or mustache.

### View to model model binding
* Typically consists of binding a change events to form elements on page.  
* There are backbone extensions which can take care of this for you. 
* fairly easy to write your own. 

# View Techniques
## Dynamic Template Loading
## Validation
* Backbone's validation occurs on the model object.  
* Validate is provided by the developer, is called automatically before save, this gives the developer a hook to implement custom reusable logic.  There are some 3rd party extensions for this. For example:  Backbone.Validation 

## Base View
A base view can be useful in situations where you find yourself copy pasting the same code in each view or calling the same methods for each view; validation or template loading for example.




