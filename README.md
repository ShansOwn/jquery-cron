# jQuery plugin: cron

jquery-cron is a [jQuery] plugin for 
presenting a simplified interface for users to specify **spring** cron entries.

## Dependencies

 * [jQuery]
 * [Bootstrap css]
 
## Notes:
 At this stage, we only support a subset of possible cron options.
 For example, each cron entry can only be digits or "*", no commas
 to denote multiple entries. We also limit the allowed combinations:
 * Every minute : 0 * * * * *
 * Every hour   : 0 0 * * * *
 * Every day    : 0 0 0 * * *
 * Every week   : 0 0 0 * * 0
 * Every month  : 0 0 0 0 * ?
 * Every year   : 0 0 0 0 0 ?

## Usage with Angular 1.*

To use this plugin, one needs to declare directive:
```JS
module.directive('jqueryCron', ['$timeout', function($timeout) {
  function attachCronSelector (element, initial) {
    $(element).cron({
      initial: initial,
      onChange: function() {
        $timeout(function(){
          var child = element[0].querySelector('input');
          var childNgModel = angular.element(child).controller('ngModel');
          var cron = $(element).cron("value");
          childNgModel.$setViewValue(cron);
        });
      }
    });
  }
  return {
    restrict: 'A',
    scope: {
      init: '=?'
    },
    link: function(scope, element, attrs) {
      var child = element[0].querySelector('input');
      var childNgModel = angular.element(child).controller('ngModel');
      scope.init = false;
      
      scope.$watch(function() {
        return childNgModel.$modelValue;
      }, function(modelValue) {
        if (!scope.init) {
          attachCronSelector(element, modelValue ? modelValue : "0 0 0 * * *");
        }
        if (modelValue) {
          var cron = $(element).cron("value");
          if (modelValue != cron) {
            childNgModel.$setViewValue(cron);
          }
        }
        scope.init = true;
      });
    }
  }
}])
```
And use it in template:
```html
<div class="input-group full-width" jquery-cron>
    <input ng-model="model.cron" ng-show="false" type="text">
</div>
```

## Others

Copyright (c) 2010-2013, Shawn Chin.

This project is licensed under the [MIT license].

 [jQuery]: http://jquery.com "jQuery"
 [Bootstrap css]: http://getbootstrap.com/css/ "Bootstrap css"
 [MIT License]: http://www.opensource.org/licenses/mit-license.php "MIT License" 
