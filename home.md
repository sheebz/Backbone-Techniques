# Modelbinding
Modelbinding is a concept where your model's attributes are automatically bound to elements on the view.  Modelbinding offers several benefits:
* eliminates need for tedious event wiring logic to keep view and model in sync.
* reduces total amount of code in application.
* encapsulates event wiring and model to view sync logic

## Model to view model binding
This is typically taken care of via a template library such as handlebars or mustache.

## View to model model binding
* Typically consists of binding a change events to form elements on page.  
* There are backbone extensions which can take care of this for you. 
* fairly easy to write your own.

# Validation
* Backbone's validation occurs on the model object.  
* Validate is provided by the developer, is called automatically before save, this gives the developer a hook to implement custom reusable logic.  There are some 3rd party extensions for this.
** Backbone.Validation - 

# Dependency Injection
