## A WIP Angular 2 cheatsheet for dart (alpha 24)

**Bootstrap angular**
```dart
import 'package:angular2/angular2.dart';
main() => bootstrap(MyApp); //MyApp is a component
```

**Bootstrap angular with default router**
```dart
import 'package:angular2/angular2.dart';
import 'package:angular2/router.dart';
main() => bootstrap(MyApp, routerInjectables);
```


### Components

```dart
@Component(selector: 'selector-name', appInjector: const [injectables])
@View(templateUrl: "home.html", directives: const [directives])
class MyComponent {}
```
#### @View

**template**: replace the current element with the contents of the
HTML string.
```dart
//<my-banner></my-banner>
@Component(selector: 'my-banner')
@View(template: '<div class="banner">...</div>')
class MyBanner {}
```

**templateUrl**: replace the current element with the contents loaded by the specified URL
```dart
//<my-banner></my-banner>
@Component(selector: 'my-banner')
@View(templateUrl: 'my-banner.html')
class MyBanner {}
```
```html
<!-- my-banner.html -->
<div class="banner">...</div>
```

**directives**: Specifies a list of directives that can be used within a template. *Its optional*

**Add Angular core directives (NgFor, NgIf, NgNonBindable, NgSwitch, NgSwitchWhen, NgSwitchDefault)**
```dart
@Component(selector: 'my-component')
@View(templateUrl: "my-component.html", directives: const [coreDirectives])
class MyComponent {}
```

**Add directives/components to be used in the template**
```dart
@Directive(selector: '[opacity]')
class Opacity {/* ... */}

@Component(selector: '[item]')
@View(templateUrl: "item.html")
class Item {/* ... */}

@Component(selector: 'my-component')
@View(templateUrl: "my-component.html", directives: const [Opacity, Item])
class MyComponent {}
```

#### @Component

**selector**: The CSS selector that triggers the instantiation of a directive.

   - `element-name`: select by element name.
   - `.class`: select by class name.
   - `[attribute]`: select by attribute name.
   - `[attribute=value]`: select by attribute name and value.
   - `:not(sub_selector)`: select only if the element does not match the `sub_selector`.
   - `selector1, selector2`: select if either `selector1` or `selector2` matches.

**Selector example**
```dart
//<my-component></my-component>
@Component(selector: 'my-component')
@View(templateUrl: "my-component.html")
class MyComponent {}

//<div my-component></div>
@Decorator(selector: '[my-component]')
@View(templateUrl: "my-component.html")
class MyComponent {}
```

**Inject dependencies into a component**
```dart
@Injectable //Needed for Angular tranformer
class MyService {}

@Component(selector: 'selector-name', appInjector: const [MyService])
class MyComponent {
    MyService service;
    MyComponent(this.service);
}
```

**Accesing host DOM element in a component/decorator**

```dart
//<div selector-name></div>
@Decorator(selector: '[selector-name]')
class MyComponent {
    dom.Element element;
    MyComponent(ElementRef ref) {
        element =  ref.domElement;
    }
}
```

**properties**: The `properties` property defines a set of `directiveProperty` to `bindingProperty`  key-value pairs. *Its optional*
   - `directiveProperty` specifies the component property where the value is written.
   - `bindingProperty` specifies the DOM property where the value is read from.

**Example of properties**
```dart
//<my-component my-name="Hodor" my-desc="hooodor?"></my-component>
@Component(
    selector: 'my-component', 
    properties: const {
        'name': 'my-name',// -> set name(name)
        'desc': 'my-desc'
})
@View(templateUrl: "my-component.html")
class MyComponent {
    String _name;
    int desc;
    set name(name) => _name;
}
```
