# Angular Material Component Theming 

Angular Material doesn't currently provide a way to make custom components / directives themeable. 

This service parses tokenized CSS to generate ```<style>``` tags based on the apps them which are added to the ```<head>``` section of the application.

## Status

* Code needs tidying
* Proper tests need writing

## Dependencies 

Requires [Angular](https://github.com/angular/angular) and [Angular Material](https://github.com/angular/material)


## Installation

In your index.html file, include the component theming module.

```html
<!-- module -->
<script type="text/javascript" src="../src/md-component-theme/theming.component.js"></script>
```

Include the material.core.theming.componentTheme module as a dependency in your application.

```javascript
angular.module('myApp', ['ngMaterial', 'material.core.theming.componentTheme']);
```

## Usage

```$mdComponentTheme``` service has one function ```$mdComponentTheme.generateTheme(css)```. The generate theme function accepts CSS as a string *not* a file.

It's useful to compile your SCSS/CSS to a constant during your components/directives build.

The CSS string can contain tokens which will get replaced with the correct colors for the apps theme at runtime.

* ```THEME_NAME``` e.g. ```md-button.md-THEME_NAME-theme```
* ```{{color}}``` e.g.  ```background-color: '{{primary-color}}'```

[See the Angular Material ```*-theme.scss`` files for more examples](https://github.com/angular/material/tree/master/src/components).

### Example

```javascript
    angular
    .module('your.custom.component',['ngMaterial','material.core.theming.componentTheme'])
    .directive('customComponent', customComponentDirective)
    .constant("$CUSTOM_COMPONENT_THEME_CSS", "md-stepper.md-THEME_NAME-theme.md-primary md-step.active md-step-circle,md-stepper.md-THEME_NAME-theme.md-primary md-step.done md-step-circle{color:'{{primary-contrast}}';background-color:'{{primary-color}}'}...");

    function customComponentDirective($injector,$mdComponentTheme) {
        //Get component theme CSS
        var themeCss = $injector.get('$MD_STEPPER_THEME_CSS');
        // Parse CSS and add to HEAD with correct theme colors
        $mdComponentTheme.generateTheme(themeCss);

        /**
        Rest of your code
        */
    }

```



