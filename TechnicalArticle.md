# Custom directive to conditionally render wrapper element

## The Problem

Using AngularJS, I want to be able to conditionally render a HTML element with contents or just the contents of that HTML element. For example, I either want to render an `<a>` tag around some text or just the text based on some condition:

**HTML markup logic**

*[Language: HTML]*

	<!-- If model.hasMoreInfo (a bool value) is true,
	render link and text -->
	<a href="/moreinfo">{{model.label}}</a>
	<!-- otherwise just render text -->
	{{model.label}}

**`$scope` model example**

*[Language: JavaScript]*

    $scope.model = {
        hasMoreInfo: true,
        label: 'My Label'
    };

I want to avoid having to wrap the text with some other "container" HTML element (like a `<span>`) when there is no need for it.


## The Solution

The solution I have come up with is a custom directive that uses the **[ng-if][1]** type of logic/pattern under the hood that I can apply as an attribute to an element (within an immediately invoked function):

**AngularJS Directive**  

*[Language: JavaScript]*

    (function () {

        var directiveId = 'renderWrapIf';

        var directive = function() {
            return {
                link: function($scope, element, attributes) {
                    $scope.$watch(attributes[directiveId],
						function ngIfWatchAction(value) {
                        if (!value) {
                            element.replaceWith(
								element.contents());
                        }
                    });
                }
            };
        };

        angular.module('app').directive(directiveId, directive);

    })();

**Markup usage**  

*[Language: HTML]*

    <a render-wrap-if="model.hasMoreInfo" href="/moreinfo">
		{{model.label}}
	</a>

*Render results when true*

*[Language: HTML]*

    <a render-wrap-if="model.hasMoreInfo"
	   href="/moreinfo"
	   class="ng-binding">
		My Label
	</a>

*Render results when false*

    My Label

This allows me to create markup in a form that is still familiar and easily identifiable as to what the intended render will be, but also be able to avoid the whole "container" extra markup just to get some data binding logic to work. I can also use this directive for other elements, not just `<a>` tags, etc.


  [1]: https://github.com/angular/angular.js/blob/master/src/ng/directive/ngIf.js#L3
