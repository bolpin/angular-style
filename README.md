# AngularJS (Angular 1.x) Style Guide

* This is based on John Papa's [Opinionated Angular (Angular 1.x) style guide](https://github.com/johnpapa/angular-styleguide).

## Table of Contents

  1. [Coffeescript Gotchas](#coffeescript-gotchas)
  1. [Single Responsibility](#single-responsibility)
  1. [Modules](#modules)
  1. [Controllers](#controllers)
  1. [Services](#services)
  1. [Factories](#factories)
  1. [Data Services](#data-services)
  1. [Directives](#directives)
  1. [Resolving Promises for a Controller](#resolving-promises-for-a-controller)
  1. [Manual Annotating for Dependency Injection](#manual-annotating-for-dependency-injection)
  1. [Minification and Annotation](#minification-and-annotation)
  1. [Exception Handling](#exception-handling)
  1. [Naming](#naming)
  1. [Application Structure LIFT Principle](#application-structure-lift-principle)
  1. [Application Structure](#application-structure)
  1. [Modularity](#modularity)
  1. [Startup Logic](#startup-logic)
  1. [Angular $ Wrapper Services](#angular--wrapper-services)
  1. [Testing](#testing)
  1. [Constants](#constants)
  1. [File Templates and Snippets](#file-templates-and-snippets)
  1. [Routing](#routing)
  1. [Filters](#filters)
  1. [Angular Docs](#angular-docs)
  1. [License](#license)

**[Back to top](#table-of-contents)**

## Coffeescript Gotchas

##### Coffeescript does not shadow variables defined in outer scopes

- Be careful that you're not reusing the name of an external variable, when you're writing a function.
- Top-level variables should be avoided, but if absolutely needed should be implemented as properties on window.  (see 'Lexical Scoping and Variable Safety' at the [[coffeescript home page](http://coffeescript.org)].)
- [[Beware!](http://lucumr.pocoo.org/2011/12/22/implicit-scoping-in-coffeescript/)]

##### Be careful when using looping constructs, modules, indentation, etc.

- [[Warning!](http://blog.artillery.com/2014/06/working-with-coffeescript-common-gotchas.html)]
- As much as possible, prefer to use Angular's ng-repeat, .each(), or $filter() helpers instead of CoffeeScript methods.

**[Back to top](#table-of-contents)**


## Single Responsibility

### Rule of 1
###### [Style [Y001](#style-y001)]

  - Define 1 component per file.

  The following example defines the `app` module and its dependencies, defines a controller, and defines a factory all in the same file.

  ```coffeescript
  # avoid
  angular
    .module('app', [ 'ngRoute' ])
    .controller('SomeController', SomeController)
    .factory 'someFactory', someFactory

  SomeController = ->
    #...

  someFactory = ->
    #...
  ```

  The same components are now separated into their own files.

  ```coffeescript
  # recommended
  # app.module.js.coffee
  angular
    .module 'app', [ 'ngRoute' ]
  ```

  ```coffeescript
  # recommended 
  # someController.js.coffee
  angular
    .module('app')
    .controller 'SomeController', SomeController

  SomeController ->
    #...
  ```

  ```coffeescript
  # recommended
  # someFactory.js.coffee
  angular
    .module('app')
    .factory 'someFactory', someFactory

  someFactory ->
    #...
  ```

**[Back to top](#table-of-contents)**


## Modules

### Avoid Naming Collisions
###### [Style [Y020](#style-y020)]

  - Use unique naming conventions with separators for sub-modules.

  *Why?*  Unique names help avoid module name collisions. Separators help define modules and their submodule hierarchy. For example `app` may be your root module while `app.dashboard` and `app.users` may be modules that are used as dependencies of `app`.

### Definitions (aka Setters)
###### [Style [Y021](#style-y021)]

  - Declare modules without a variable using the setter syntax.

  *Why?*  With 1 component per file, there is rarely a need to introduce a variable for the module.

  ```coffeescript
  # avoid
  app = angular.module('app', [
    'ngAnimate'
    'ngRoute'
    'app.shared'
    'app.dashboard'
  ])
  ```

  Instead use the simple setter syntax.

  ```coffeescript
  # recommended
  angular.module 'app', [
    'ngAnimate'
    'ngRoute'
    'app.shared'
    'app.dashboard'
  ]
  ```

### Getters
###### [Style [Y022](#style-y022)]

  - When using a module, avoid using a variable and instead use chaining with the getter syntax.

  *Why?*  This produces more readable code and avoids variable collisions or leaks.

  ```coffeescript
  # avoid
  app = angular.module('app')
  app.controller 'SomeController', SomeController

  SomeController = ->
  ```

  ```coffeescript
  # recommended 
  angular
    .module('app')
    .controller 'SomeController', SomeController

  SomeController = ->
  ```

### Setting vs Getting
###### [Style [Y023](#style-y023)]

  - Only set once and get for all other instances.

  *Why?*  A module should only be created once, then retrieved from that point and after.

    - Use `angular.module('app', [])` to set a module.
    - Use `angular.module('app')` to get a module.

### Named vs Anonymous Functions
###### [Style [Y024](#style-y024)]

  - Use named functions instead of passing an anonymous function in as a callback.

  *Why?*  This produces more readable code, is much easier to debug, and reduces the amount of nested callback code.

  ```coffeescript
  # avoid 

  angular
    .module('app')
    .controller('Dashboard', ->).factory 'logger', ->
  ```

  ```coffeescript
  # recommended
  # dashboard.js.coffee
  angular
    .module('app')
    .controller 'Dashboard', Dashboard

  Dashboard = ->
  ```

  ```coffeescript
  # logger.js.coffee
  angular
    .module('app')
    .factory 'logger', logger

  logger = ->

  ```

**[Back to top](#table-of-contents)**

## Controllers
  * Think of controllers as the business logic layer for the view.
  * Avoid making $http requests in the controller.  (Consider doing this from within a data-access service.)

### controllerAs View Syntax
###### [Style [Y030](#style-y030)]

  - Use the ['controllerAs'](http://www.johnpapa.net/do-you-like-your-angular-controllers-with-or-without-sugar/) syntax over the `classic controller with $scope` syntax.

  *Why?*  Controllers are constructed, "newed" up, and provide a single new instance, and the `controllerAs` syntax is closer to that of a JavaScript constructor than the `classic $scope syntax`.

  *Why?*  It promotes the use of binding to a "dotted" object in the View (e.g. `customer.name` instead of `name`), which is more contextual, easier to read, and avoids any reference issues that may occur without "dotting".

  *Why?*  Helps avoid using `$parent` calls in Views with nested controllers.

  ```html
  <!-- avoid -->
  <div ng-controller="Customer">
      {{ name }}
  </div>
  ```

  ```html
  <!-- recommended -->
  <div ng-controller="Customer as customer">
      {{ customer.name }}
  </div>
  ```

### controllerAs Controller Syntax
###### [Style [Y031](#style-y031)]

  - Use the `controllerAs` syntax over the `classic controller with $scope` syntax.

  - The `controllerAs` syntax uses `this` inside controllers which gets bound to `$scope`

  *Why?*  `controllerAs` is syntactic sugar over `$scope`. You can still bind to the View and still access `$scope` methods.

  *Why?*  Helps avoid the temptation of using `$scope` methods inside a controller when it may otherwise be better to avoid them or move them to a factory. Consider using `$scope` in a factory, or if in a controller just when needed. For example when publishing and subscribing events using [`$emit`](https://docs.angularjs.org/api/ng/type/$rootScope.Scope#$emit), [`$broadcast`](https://docs.angularjs.org/api/ng/type/$rootScope.Scope#$broadcast), or [`$on`](https://docs.angularjs.org/api/ng/type/$rootScope.Scope#$on) consider moving these uses to a factory and invoke from the controller.

  ```coffeescript
  # avoid 
  Customer = ($scope) ->
    $scope.name = {}

    $scope.sendMessage = ->
  ```

  ```coffeescript
  # recommended - but see next section
  Customer = ->
    @name = {}

    @sendMessage = ->
  ```

### controllerAs with vm
###### [Style [Y032](#style-y032)]

  - Use a capture variable for `this` when using the `controllerAs` syntax. Choose a consistent variable name such as `vm`, which stands for ViewModel.

  *Why?*  The `this` keyword is contextual and when used within a function inside a controller may change its context. Capturing the context of `this` avoids encountering this problem.

  ```coffeescript
  # avoid
  Customer = ->
    @name = {}

    @sendMessage = ->
  ```

  ```coffeescript
  # recommended
  Customer = ->
    vm = this
    vm.name = {}

    vm.sendMessage = ->
  ```

  Note: You can avoid any [jshint](http://www.jshint.com/) warnings by placing the comment above the line of code. However it is not needed when the function is named using UpperCasing, as this convention means it is a constructor function, which is what a controller is in Angular.

  ```coffeescript
  # jshint validthis: true 
  vm = this
  ```

  Note: When creating watches in a controller using `controller as`, you can watch the `vm.*` member using the following syntax. (Create watches with caution as they add more load to the digest cycle.)

  ```html
  <input ng-model="vm.title"/>
  ```

  ```coffeescript
  SomeController = ($scope, $log) ->
    vm = this
    vm.title = 'Some Title'
    $scope.$watch 'vm.title', (current, original) ->
      $log.info 'vm.title was %s', original
      $log.info 'vm.title is now %s', current
  ```

### Bindable Members Up Top
###### [Style [Y033](#style-y033)]

  - Place bindable members at the top of the controller, alphabetized, and not spread through the controller code.

    *Why?*  Placing bindable members at the top makes it easy to read and helps you instantly identify which members of the controller can be bound and used in the View.

    *Why?*  Setting anonymous functions in-line can be easy, but when those functions are more than 1 line of code they can reduce the readability. Defining the functions below the bindable members (the functions will be hoisted) moves the implementation details down, keeps the bindable members up top, and makes it easier to read.

  ```coffeescript
  # avoid 
  Sessions = ->
    vm = this

    vm.gotoSession = ->
      #...

    vm.refresh = ->
      #...

    vm.search = ->
      #...

    vm.sessions = []
    vm.title = 'Sessions'
  ```

  ```coffeescript
  # recommended 
  Sessions = ->
    vm = this
    vm.gotoSession = gotoSession
    vm.refresh = refresh
    vm.search = search
    vm.sessions = []
    vm.title = 'Sessions'

    gotoSession = ->
      #...

    refresh = ->
      #...

    search = ->
      #...
  ```

  Note: If the function is a 1 liner consider keeping it right up top, as long as readability is not affected.

  ```coffeescript
  # avoid
  Sessions = (data) ->
    vm = this
    vm.gotoSession = gotoSession

    vm.refresh = ->
      # lines
      # of
      # code
      # affects
      # readability

    vm.search = search
    vm.sessions = []
    vm.title = 'Sessions'
  ```

  ```coffeescript
  # recommended 
  Sessions = (dataservice) ->
    vm = this
    vm.gotoSession = gotoSession
    vm.refresh = dataservice.refresh
    # 1 liner is OK
    vm.search = search
    vm.sessions = []
    vm.title = 'Sessions'
  ```

### Function Declarations to Hide Implementation Details
###### [Style [Y034](#style-y034)]

  - Use function declarations to hide implementation details. Keep your bindable members up top. When you need to bind a function in a controller, point it to a function declaration that appears later in the file. This is tied directly to the section Bindable Members Up Top. For more details see [this post](http://www.johnpapa.net/angular-function-declarations-function-expressions-and-readable-code).

    *Why?*  Placing bindable members at the top makes it easy to read and helps you instantly identify which members of the controller can be bound and used in the View. (Same as above.)

    *Why?*  Placing the implementation details of a function later in the file moves that complexity out of view so you can see the important stuff up top.

    *Why?*  Function declaration are hoisted so there are no concerns over using a function before it is defined (as there would be with function expressions).

    *Why?*  You never have to worry with function declarations that moving `var a` before `var b` will break your code because `a` depends on `b`.

    *Why?*  Order is critical with function expressions

  ```coffeescript
  # avoid - using function expressions
  Avengers = (dataservice, logger) ->
    vm = this
    vm.avengers = []
    vm.title = 'Avengers'

    activate = ->
      getAvengers().then ->
        logger.info 'Activated Avengers View'

    getAvengers = ->
      dataservice.getAvengers().then (data) ->
        vm.avengers = data
        vm.avengers

    vm.getAvengers = getAvengers
    activate()
  ```

  Notice that the important stuff is scattered in the preceding example. In the example below, notice that the important stuff is up top. For example, the members bound to the controller such as `vm.avengers` and `vm.title`. The implementation details are down below. This is just easier to read.

  ```coffeescript
  
  # recommend - using function declarations and bindable members up top
  Avengers = (dataservice, logger) ->
    vm = this
    vm.avengers = []
    vm.getAvengers = getAvengers
    vm.title = 'Avengers'
    activate()

    #/////

    activate = ->
      getAvengers().then ->
        logger.info 'Activated Avengers View'

    getAvengers = ->
      dataservice.getAvengers().then (data) ->
        vm.avengers = data
        vm.avengers
  ```

### Defer Controller Logic to Services
###### [Style [Y035](#style-y035)]

  - Defer logic in a controller by delegating to services and factories.

    *Why?*  Logic may be reused by multiple controllers when placed within a service and exposed via a function.

    *Why?*  Logic in a service can more easily be isolated in a unit test, while the calling logic in the controller can be easily mocked.

    *Why?*  Removes dependencies and hides implementation details from the controller.

    *Why?*  Keeps the controller slim, trim, and focused.

  ```coffeescript

  # avoid 
  Order = ($http, $q, config, userInfo) ->
    vm = this
    vm.checkCredit = checkCredit
    vm.isCreditOk
    vm.total = 0

    checkCredit = ->
      settings = {}
      # Get the credit service base URL from config
      # Set credit service required headers
      # Prepare URL query string or data object with request data
      # Add user-identifying info so service gets the right credit limit for this user.
      # Use JSONP for this browser if it doesn't support CORS
      $http.get(settings).then((data) ->
        # Unpack JSON data in the response object
        # to find maxRemainingAmount
        vm.isCreditOk = vm.total <= maxRemainingAmount
      ).catch (error) ->
        # Interpret error
        # Cope w/ timeout? retry? try alternate service?
        # Re-reject with appropriate error for a user to see
  ```

  ```coffeescript
  # recommended 

  Order = (creditService) ->
    vm = this
    vm.checkCredit = checkCredit
    vm.isCreditOk
    vm.total = 0

    checkCredit = ->
      creditService.isOrderTotalOk(vm.total).then((isOk) ->
        vm.isCreditOk = isOk
      ).catch showServiceError
  ```

### Keep Controllers Focused
###### [Style [Y037](#style-y037)]

  - Define a controller for a view, and try not to reuse the controller for other views. Instead, move reusable logic to factories and keep the controller simple and focused on its view.

    *Why?*  Reusing controllers with several views is brittle and good end-to-end (e2e) test coverage is required to ensure stability across large applications.

### Assigning Controllers
###### [Style [Y038](#style-y038)]

  - When a controller must be paired with a view and either component may be re-used by other controllers or views, define controllers along with their routes.

    Note: If a View is loaded via another means besides a route, then use the `ng-controller="Avengers as vm"` syntax.

    *Why?*  Pairing the controller in the route allows different routes to invoke different pairs of controllers and views. When controllers are assigned in the view using ['ng-controller'](https://docs.angularjs.org/api/ng/directive/ngController), that view is always associated with the same controller.

 ```coffeescript
  # avoid - when using with a route and dynamic pairing is desired 
  # route-config.js.coffee
  angular
    .module('app')
    .config config

  config = ($routeProvider) ->
    $routeProvider.when '/avengers', templateUrl: 'avengers.html'
  ```

  ```html
  <!-- avoid -->
  <!-- avengers.html -->
  <div ng-controller="Avengers as vm">
  </div>
  ```

  ```coffeescript
  # recommended 
  # route-config.js.coffee
  angular
    .module('app')
    .config config

  config = ($routeProvider) ->
    $routeProvider.when '/avengers',
      templateUrl: 'avengers.html'
      controller: 'Avengers'
      controllerAs: 'vm'
  ```

  ```html
  <!-- avengers.html -->
  <div>
  </div>
  ```

**[Back to top](#table-of-contents)**

## Services

### Singletons
###### [Style [Y040](#style-y040)]

  - Services are instantiated with the `new` keyword, use `this` for public methods and variables. Since these are so similar to factories, use a factory instead for consistency.

    Note: [All Angular services are singletons](https://docs.angularjs.org/guide/services). This means that there is only one instance of a given service per injector.

  ```coffeescript
  # service
  angular
    .module('app')
    .service 'logger', logger

  logger = ->
    @logError = (msg) ->
      #...
  ```

  ```coffeescript
  # factory
  angular
    .module('app')
    .factory 'logger', logger

  logger = ->
    logError: (msg) ->
      #...
  ```

**[Back to top](#table-of-contents)**

## Factories

### Single Responsibility
###### [Style [Y050](#style-y050)]

  - Factories should have a [single responsibility](http://en.wikipedia.org/wiki/Single_responsibility_principle), that is encapsulated by its context. Once a factory begins to exceed that singular purpose, a new factory should be created.

### Singletons
###### [Style [Y051](#style-y051)]

  - Factories are singletons and return an object that contains the members of the service.

    Note: [All Angular services are singletons](https://docs.angularjs.org/guide/services).

### Accessible Members Up Top
###### [Style [Y052](#style-y052)]

  - Expose the callable members of the service (its interface) at the top, using a technique derived from the [Revealing Module Pattern](http://addyosmani.com/resources/essentialjsdesignpatterns/book/#revealingmodulepatternjavascript).

    *Why?*  Placing the callable members at the top makes it easy to read and helps you instantly identify which members of the service can be called and must be unit tested (and/or mocked).

    *Why?*  This is especially helpful when the file gets longer as it helps avoid the need to scroll to see what is exposed.

    *Why?*  Setting functions as you go can be easy, but when those functions are more than 1 line of code they can reduce the readability and cause more scrolling. Defining the callable interface via the returned service moves the implementation details down, keeps the callable interface up top, and makes it easier to read.

  ```coffeescript
  # avoid
  dataService = ->
    someValue = ''

    save = ->

    validate = ->

    {
      save: save
      someValue: someValue
      validate: validate
    }
  ```

  ```coffeescript
  # recommended 
  dataService = ->
    someValue = ''
    service = 
      save: save
      someValue: someValue
      validate: validate

    #//////////

    save = ->

    validate = ->

    service
  ```

  This way bindings are mirrored across the host object, primitive values cannot update alone using the revealing module pattern.

    ![Factories Using "Above the Fold"](https://raw.githubusercontent.com/johnpapa/angular-styleguide/master/assets/above-the-fold-2.png)

### Function Declarations to Hide Implementation Details
###### [Style [Y053](#style-y053)]

  - Use function declarations to hide implementation details. Keep your accessible members of the factory up top. Point those to function declarations that appears later in the file. For more details see [this post](http://www.johnpapa.net/angular-function-declarations-function-expressions-and-readable-code).

    *Why?*  Placing accessible members at the top makes it easy to read and helps you instantly identify which functions of the factory you can access externally.

    *Why?*  Placing the implementation details of a function later in the file moves that complexity out of view so you can see the important stuff up top.

    *Why?*  Function declaration are hoisted so there are no concerns over using a function before it is defined (as there would be with function expressions).

    *Why?*  You never have to worry with function declarations that moving `var a` before `var b` will break your code because `a` depends on `b`.

    *Why?*  Order is critical with function expressions

  ```coffeescript
  # avoid - using function expressions
  dataservice = ($http, $location, $q, exception, logger) ->
    isPrimed = false
    primePromise = undefined

    getAvengers = ->
      # implementation details go here

    getAvengerCount = ->
      # implementation details go here

    getAvengersCast = ->
      # implementation details go here

    prime = ->
      # implementation details go here

    ready = (nextPromises) ->
      # implementation details go here

    service =
      getAvengersCast: getAvengersCast
      getAvengerCount: getAvengerCount
      getAvengers: getAvengers
      ready: ready

    service


  ```

  ```coffeescript
  # recommended
  # Using function declarations and accessible members up top
  dataservice = ($http, $location, $q, exception, logger) ->
    isPrimed = false
    primePromise = undefined

    service =
      getAvengersCast: getAvengersCast
      getAvengerCount: getAvengerCount
      getAvengers: getAvengers
      ready: ready

    #//////////

    getAvengers = ->
      # implementation details go here

    getAvengerCount = ->
      # implementation details go here

    getAvengersCast = ->
      # implementation details go here

    prime = ->
      # implementation details go here

    ready = (nextPromises) ->
      # implementation details go here

    service
  ```

**[Back to top](#table-of-contents)**

## Data Services

### Separate Data Calls
###### [Style [Y060](#style-y060)]

  - Refactor logic for making data operations and interacting with data to a factory. Make data services responsible for XHR calls, local storage, stashing in memory, or any other data operations.

    *Why?*  The controller's responsibility is for the presentation and gathering of information for the view. It should not care how it gets the data, just that it knows who to ask for it. Separating the data services moves the logic on how to get it to the data service, and lets the controller be simpler and more focused on the view.

    *Why?*  This makes it easier to test (mock or real) the data calls when testing a controller that uses a data service.

    *Why?*  Data service implementation may have very specific code to handle the data repository. This may include headers, how to talk to the data, or other services such as `$http`. Separating the logic into a data service encapsulates this logic in a single place hiding the implementation from the outside consumers (perhaps a controller), also making it easier to change the implementation.

  ```coffeescript
  # recommended 
  # dataservice factory

  angular
    .module('app.core')
    .factory 'dataservice', dataservice

  dataservice.$inject = [
    '$http'
    'logger'
  ]

  dataservice = ($http, logger) ->

    getAvengers = ->

      getAvengersComplete = (response) ->
        response.data.results

      getAvengersFailed = (error) ->
        logger.error 'XHR Failed for getAvengers.' + error.data

      $http.get('/api/maa').then(getAvengersComplete).catch getAvengersFailed

    { getAvengers: getAvengers }

  ```

    Note: The data service is called from consumers, such as a controller, hiding the implementation from the consumers, as shown below.

  ```coffeescript
  # recommended
  # controller calling the dataservice factory
  angular
    .module('app.avengers')
    .controller 'Avengers', Avengers

  Avengers.$inject = [
    'dataservice'
    'logger'
  ]

  Avengers = (dataservice, logger) ->
    vm = this

    activate = ->
      getAvengers().then ->
        logger.info 'Activated Avengers View'

    getAvengers = ->
      dataservice.getAvengers().then (data) ->
        vm.avengers = data
        vm.avengers

    vm.avengers = []
    activate()
  ```

### Return a Promise from Data Calls
###### [Style [Y061](#style-y061)]

  - When calling a data service that returns a promise such as `$http`, return a promise in your calling function too.

    *Why?*  You can chain the promises together and take further action after the data call completes and resolves or rejects the promise.

  ```coffeescript
  # recommended
  activate()

  activate = ->

    ###
    # Step 1
    # Ask the getAvengers function for the
    # avenger data and wait for the promise
    ###

    getAvengers().then ->

      ###
      # Step 4
      # Perform an action on resolve of final promise
      ###

      logger.info 'Activated Avengers View'

  getAvengers = ->

    ###
    # Step 2
    # Ask the data service for the data and wait
    # for the promise
    ###

    dataservice.getAvengers().then (data) ->

      ###
      # Step 3
      # set the data and resolve the promise
      ###

      vm.avengers = data
      vm.avengers


  ```

**[Back to top](#table-of-contents)**

## Directives
### Limit 1 Per File
###### [Style [Y070](#style-y070)]

  - Create one directive per file. Name the file for the directive.

    *Why?*  It is easy to mash all the directives in one file, but difficult to then break those out so some are shared across apps, some across modules, some just for one module.

    *Why?*  One directive per file is easy to maintain.

    > Note: "**Best Practice**  Directives should clean up after themselves. You can use `element.on('$destroy', ...)` or `scope.$on('$destroy', ...)` to run a clean-up function when the directive is removed" ... from the Angular documentation.

  ```coffeescript
  # avoid 
  # directives.js.coffee
  angular
    .module('app.widgets')

    # order directive that is specific to the order module
    .directive('orderCalendarRange', orderCalendarRange)

    # sales directive that can be used anywhere across the sales app
    .directive('salesCustomerInfo', salesCustomerInfo)

    # spinner directive that can be used anywhere across apps
    .directive('sharedSpinner', sharedSpinner)

  orderCalendarRange = ->

  salesCustomerInfo = ->

  sharedSpinner = ->
  ```

  ```coffeescript
  # recommended
  # calendarRange.directive.js.coffee
  # @desc order directive that is specific to the order module at a company named Acme
  # @example <div acme-order-calendar-range></div>

  angular
    .module('sales.order')
    .directive 'acmeOrderCalendarRange', orderCalendarRange

  orderCalendarRange = ->
  ```

  ```coffeescript
  # recommended
  # customerInfo.directive.js.coffee
  # @desc sales directive that can be used anywhere across the sales app at a company named Acme
  # @example <div acme-sales-customer-info></div>

  angular
    .module('sales.widgets')
    .directive 'acmeSalesCustomerInfo', salesCustomerInfo
  
  salesCustomerInfo = ->
  ```

  ```coffeescript
  # recommended
  # spinner.directive.js.coffee
  # @desc spinner directive that can be used anywhere across apps at a company named Acme
  # @example <div acme-shared-spinner></div>

  angular
    .module('shared.widgets')
    .directive 'acmeSharedSpinner', sharedSpinner

  sharedSpinner = ->
  ```

Note: There are many naming options for directives, especially since they can be used in narrow or wide scopes. Choose one that makes the directive and its file name distinct and clear. Some examples are below, but see the [Naming](#naming) section for more recommendations.

### Manipulate DOM in a Directive
###### [Style [Y072](#style-y072)]

  - When manipulating the DOM directly, use a directive. If alternative ways can be used such as using CSS to set styles or the [animation services](https://docs.angularjs.org/api/ngAnimate), Angular templating, ['ngShow'](https://docs.angularjs.org/api/ng/directive/ngShow) or ['ngHide'](https://docs.angularjs.org/api/ng/directive/ngHide), then use those instead. For example, if the directive simply hides and shows, use ngHide/ngShow.

    *Why?*  DOM manipulation can be difficult to test, debug, and there are often better ways (e.g. CSS, animations, templates)

### Provide a Unique Directive Prefix
###### [Style [Y073](#style-y073)]

  - Provide a short, unique and descriptive directive prefix such as `acmeSalesCustomerInfo` which would be declared in HTML as `acme-sales-customer-info`.

    *Why?*  The unique short prefix identifies the directive's context and origin. For example a prefix of `cc-` may indicate that the directive is part of a CodeCamper app while `acme-` may indicate a directive for the Acme company.

    Note: Avoid `ng-` as these are reserved for Angular directives. Research widely used directives to avoid naming conflicts, such as `ion-` for the [Ionic Framework](http://ionicframework.com/).

### Restrict to Elements and Attributes
###### [Style [Y074](#style-y074)]

  - When creating a directive that makes sense as a stand-alone element, allow restrict `E` (custom element) and optionally restrict `A` (custom attribute). Generally, if it could be its own control, `E` is appropriate. General guideline is allow `EA` but lean towards implementing as an element when it's stand-alone and as an attribute when it enhances its existing DOM element.

    *Why?*  It makes sense.

    *Why?*  While we can allow the directive to be used as a class, if the directive is truly acting as an element it makes more sense as an element or at least as an attribute.

    Note: EA is the default for Angular 1.3 +

  ```html
  <!-- avoid -->
  <div class="my-calendar-range"></div>
  ```

  ```coffeescript
  # avoid
  angular
    .module('app.widgets').directive 'myCalendarRange', myCalendarRange

  myCalendarRange = ->
    directive = 
      link: link
      templateUrl: '/template/is/located/here.html'
      restrict: 'C'

    link = (scope, element, attrs) ->

    directive
  ```

  ```html
  <!-- recommended -->
  <my-calendar-range></my-calendar-range>
  <div my-calendar-range></div>
  ```

  ```coffeescript
  # recommended 
  angular
    .module('app.widgets')
    .directive 'myCalendarRange', myCalendarRange

  myCalendarRange = ->
    directive = 
      link: link
      templateUrl: '/template/is/located/here.html'
      restrict: 'EA'

    link = (scope, element, attrs) ->

    directive
  ```

### Directives and ControllerAs
###### [Style [Y075](#style-y075)]

  - Use `controller as` syntax with a directive to be consistent with using `controller as` with view and controller pairings.

    *Why?*  It makes sense and it's not difficult.

    Note: The directive below demonstrates some of the ways you can use scope inside of link and directive controllers, using controllerAs. I in-lined the template just to keep it all in one place.

    Note: Regarding dependency injection, see [Manually Identify Dependencies](#manual-annotating-for-dependency-injection).

    Note: Note that the directive's controller is outside the directive's closure. This style eliminates issues where the injection gets created as unreachable code after a `return`.

  ```html
  <div my-example max="77"></div>
  ```

  ```coffeescript
  angular
    .module('app')
    .directive 'myExample', myExample

  myExample = ->
    directive = 
      restrict: 'EA'
      templateUrl: 'app/feature/example.directive.html'
      scope: max: '='
      link: linkFunc
      controller: ExampleController
      controllerAs: 'vm'
      bindToController: true

    linkFunc = (scope, el, attr, ctrl) ->
      console.log 'LINK: scope.min = %s *** should be undefined', scope.min
      console.log 'LINK: scope.max = %s *** should be undefined', scope.max
      console.log 'LINK: scope.vm.min = %s', scope.vm.min
      console.log 'LINK: scope.vm.max = %s', scope.vm.max

    directive

  ExampleController.$inject = [ '$scope' ]

  ExampleController = ($scope) ->
    # Injecting $scope just for comparison
    vm = this
    vm.min = 3
    console.log 'CTRL: $scope.vm.min = %s', $scope.vm.min
    console.log 'CTRL: $scope.vm.max = %s', $scope.vm.max
    console.log 'CTRL: vm.min = %s', vm.min
    console.log 'CTRL: vm.max = %s', vm.max
  ```

  ```html
  <!-- example.directive.html -->
  <div>hello world</div>
  <div>max={{vm.max}}<input ng-model="vm.max"/></div>
  <div>min={{vm.min}}<input ng-model="vm.min"/></div>
  ```

    Note: You can also name the controller when you inject it into the link function and access directive attributes as properties of the controller.

  ```coffeescript
  # Alternative to above example
  linkFunc = (scope, el, attr, vm) ->
    console.log 'LINK: scope.min = %s *** should be undefined', scope.min
    console.log 'LINK: scope.max = %s *** should be undefined', scope.max
    console.log 'LINK: vm.min = %s', vm.min
    console.log 'LINK: vm.max = %s', vm.max
  ```

###### [Style [Y076](#style-y076)]

  - Use `bindToController = true` when using `controller as` syntax with a directive when you want to bind the outer scope to the directive's controller's scope.

    *Why?*  It makes it easy to bind outer scope to the directive's controller scope.

    Note: `bindToController` was introduced in Angular 1.3.0.

  ```html
  <div my-example max="77"></div>
  ```

  ```coffeescript
  angular
    .module('app')
    .directive 'myExample', myExample

  myExample = ->

    directive = 
      restrict: 'EA'
      templateUrl: 'app/feature/example.directive.html'
      scope: max: '='
      controller: ExampleController
      controllerAs: 'vm'
      bindToController: true

    directive

  ExampleController = ->
    vm = this
    vm.min = 3
    console.log 'CTRL: vm.min = %s', vm.min
    console.log 'CTRL: vm.max = %s', vm.max
  ```

  ```html
  <!-- example.directive.html -->
  <div>hello world</div>
  <div>max={{vm.max}}<input ng-model="vm.max"/></div>
  <div>min={{vm.min}}<input ng-model="vm.min"/></div>
  ```

**[Back to top](#table-of-contents)**

## Resolving Promises for a Controller
### Controller Activation Promises
###### [Style [Y080](#style-y080)]

  - Resolve start-up logic for a controller in an `activate` function.

    *Why?*  Placing start-up logic in a consistent place in the controller makes it easier to locate, more consistent to test, and helps avoid spreading out the activation logic across the controller.

    *Why?*  The controller `activate` makes it convenient to re-use the logic for a refresh for the controller/View, keeps the logic together, gets the user to the View faster, makes animations easy on the `ng-view` or `ui-view`, and feels snappier to the user.

    Note: If you need to conditionally cancel the route before you start use the controller, use a [route resolve](#style-y081) instead.
    Note: If you need to conditionally cancel the route before you start use the controller, use a [route resolve](#style-y081) instead.

  ```coffeescript
  # avoid
  Avengers = (dataservice) ->
    vm = this
    vm.avengers = []
    vm.title = 'Avengers'

    dataservice.getAvengers().then (data) ->
      vm.avengers = data
      vm.avengers
  ```

  ```coffeescript
  # recommended
  Avengers = (dataservice) ->
    vm = this
    vm.avengers = []
    vm.title = 'Avengers'

    activate()

    #//////////

    activate = ->
      dataservice.getAvengers().then (data) ->
        vm.avengers = data
        vm.avengers
  ```

### Route Resolve Promises

  - When a controller depends on a promise to be resolved before the controller is activated, resolve those dependencies in the `$routeProvider` before the controller logic is executed. If you need to conditionally cancel a route before the controller is activated, use a route resolver.

  - Use a route resolve when you want to decide to cancel the route before ever transitioning to the View.

    *Why?*  A controller may require data before it loads. That data may come from a promise via a custom factory or [$http](https://docs.angularjs.org/api/ng/service/$http). Using a [route resolve](https://docs.angularjs.org/api/ngRoute/provider/$routeProvider) allows the promise to resolve before the controller logic executes, so it might take action based on that data from the promise.

    *Why?*  The code executes after the route and in the controller’s activate function. The View starts to load right away. Data binding kicks in when the activate promise resolves. A “busy” animation can be shown during the view transition (via `ng-view` or `ui-view`)

    Note: The code executes before the route via a promise. Rejecting the promise cancels the route. Resolve makes the new view wait for the route to resolve. A “busy” animation can be shown before the resolve and through the view transition. If you want to get to the View faster and do not require a checkpoint to decide if you can get to the View, consider the [controller `activate` technique](#style-y080) instead.
    Note: The code executes before the route via a promise. Rejecting the promise cancels the route. Resolve makes the new view wait for the route to resolve. A “busy” animation can be shown before the resolve and through the view transition. If you want to get to the View faster and do not require a checkpoint to decide if you can get to the View, consider the [controller `activate` technique](#style-y080) instead.

  ```coffeescript
  # avoid 
  angular
    .module('app')
    .controller 'Avengers', Avengers

  Avengers = (movieService) ->
    vm = this

    # unresolved
    vm.movies

    # resolved asynchronously
    movieService.getMovies().then (response) ->
      vm.movies = response.movies
  ```

  ```coffeescript
  # better
  # route-config.js.coffee
  angular
    .module('app')
    .config config

  config = ($routeProvider) ->
    $routeProvider.when '/avengers',
      templateUrl: 'avengers.html'
      controller: 'Avengers'
      controllerAs: 'vm'
      resolve: moviesPrepService: (movieService) ->
        movieService.getMovies()


  # avengers.js.coffee
  angular
    .module('app')
    .controller 'Avengers', Avengers

  Avengers.$inject = [ 'moviesPrepService' ]

  Avengers = (moviesPrepService) ->
    vm = this
    vm.movies = moviesPrepService.movies
  ```

    Note: The example below shows the route resolve points to a named function, which is easier to debug and easier to handle dependency injection.

  ```coffeescript
  # even better 
  # route-config.js.coffee
  angular
    .module('app')
    .config config

  config = ($routeProvider) ->
    $routeProvider.when '/avengers',
      templateUrl: 'avengers.html'
      controller: 'Avengers'
      controllerAs: 'vm'
      resolve: moviesPrepService: moviesPrepService

  moviesPrepService = (movieService) ->
    movieService.getMovies()


  # avengers.js.coffee
  angular
    .module('app')
    .controller 'Avengers', Avengers

  Avengers.$inject = [ 'moviesPrepService' ]

  Avengers = (moviesPrepService) ->
    vm = this
    vm.movies = moviesPrepService.movies
  ```
    Note: The code example's dependency on `movieService` is not minification safe on its own. For details on how to make this code minification safe, see the sections on [dependency injection](#manual-annotating-for-dependency-injection) and on [minification and annotation](#minification-and-annotation).

**[Back to top](#table-of-contents)**

## Manual Annotating for Dependency Injection

### UnSafe from Minification

  - Avoid using the shortcut syntax of declaring dependencies without using a minification-safe approach.

    *Why?*  The parameters to the component (e.g. controller, factory, etc) will be converted to mangled variables. For example, `common` and `dataservice` may become `a` or `b` and not be found by Angular.

    ```coffeescript
    # avoid - not minification-safe
    TODO
    ```

    This code may produce mangled variables when minified and thus cause runtime errors.

### Manually Identify Dependencies
###### [Style [Y091](#style-y091)]

  - Use `$inject` to manually identify your dependencies for Angular components.

    *Why?*  This technique mirrors the technique used by ['ng-annotate'](https://github.com/olov/ng-annotate), which I recommend for automating the creation of minification safe dependencies. If `ng-annotate` detects injection has already been made, it will not duplicate it.

    *Why?*  This safeguards your dependencies from being vulnerable to minification issues when parameters may be mangled. For example, `common` and `dataservice` may become `a` or `b` and not be found by Angular.

    *Why?*  Avoid creating in-line dependencies as long lists can be difficult to read in the array. Also it can be confusing that the array is a series of strings while the last item is the component's function.

    ```coffeescript
    # avoid
    Angular
      .module('app')
      .controller 'Dashboard', [
        '$location'
        '$routeParams'
        'common'
        'dataservice'
        ($location, $routeParams, common, dataservice) ->
      ]
    ```

    ```coffeescript
    # avoid 
    Dashboard = ($location, $routeParams, common, dataservice) ->

    angular
      .module('app')
      .controller 'Dashboard', [
        '$location'
        '$routeParams'
        'common'
        'dataservice'
        Dashboard
      ]
    ```

    ```coffeescript
    # recommended
    Dashboard = ($location, $routeParams, common, dataservice) ->

    angular
      .module('app')
      .controller 'Dashboard', Dashboard

    Dashboard.$inject = [
      '$location'
      '$routeParams'
      'common'
      'dataservice'
    ]
    ```

    Note: When your function is below a return statement the `$inject` may be unreachable (this may happen in a directive). You can solve this by moving the Controller outside of the directive.

    ```coffeescript
    # avoid
    # inside a directive definition
    outer = ->
    ddo = 
      controller: DashboardPanelController
      controllerAs: 'vm'

    DashboardPanelController = (logger) ->

    return ddo
    DashboardPanelController.$inject = [ 'logger' ]
    ```

    ```coffeescript
    # recommended
    # outside a directive definition
    outer = ->
      ddo = 
        controller: DashboardPanelController
        controllerAs: 'vm'
      ddo

    DashboardPanelController.$inject = [ 'logger' ]

    DashboardPanelController = (logger) ->
    ```

### Manually Identify Route Resolver Dependencies
###### [Style [Y092](#style-y092)]

  - Use `$inject` to manually identify your route resolver dependencies for Angular components.

    *Why?*  This technique breaks out the anonymous function for the route resolver, making it easier to read.

    *Why?*  An `$inject` statement can easily precede the resolver to handle making any dependencies minification safe.

    ```coffeescript
    # recommended
    config = ($routeProvider) ->
      $routeProvider.when '/avengers',
        templateUrl: 'avengers.html'
        controller: 'AvengersController'
        controllerAs: 'vm'
        resolve: moviesPrepService: moviesPrepService

    moviesPrepService.$inject = [ 'movieService' ]

    moviesPrepService = (movieService) ->
      movieService.getMovies()
    ```

**[Back to top](#table-of-contents)**

## Minification and Annotation

### ng-annotate
###### [Style [Y100](#style-y100)]

  - Use [ng-annotate](//github.com/olov/ng-annotate) for [Gulp](http://gulpjs.com) or [Grunt](http://gruntjs.com) and comment functions that need automated dependency injection using `/** @ngInject */`

    *Why?*  This safeguards your code from any dependencies that may not be using minification-safe practices.

    *Why?*  ['ng-min'](https://github.com/btford/ngmin) is deprecated

    >I prefer Gulp as I feel it is easier to write, to read, and to debug.

    The following code is not using minification safe dependencies.

    ```coffeescript
    angular
      .module('app')
      .controller('Avengers', Avengers)

    # @ngInject
    Avengers = (storageService, avengerService) ->
      vm = this
      vm.heroSearch = ''
      vm.storeHero = storeHero

      storeHero = ->
        hero = avengerService.find(vm.heroSearch)
        storageService.save hero.name, hero
    ```

    When the above code is run through ng-annotate it will produce the following output with the `$inject` annotation and become minification-safe.

    ```coffeescript
    angular
      .module('app')
      .controller('Avengers', Avengers)

    # @ngInject
    Avengers = (storageService, avengerService) ->
      vm = this
      vm.heroSearch = ''
      vm.storeHero = storeHero

      storeHero = ->
        hero = avengerService.find(vm.heroSearch)
        storageService.save hero.name, hero

    Avengers.$inject = [
      'storageService'
      'avengerService'
    ]
    ```

    Note: If `ng-annotate` detects injection has already been made (e.g. `@ngInject` was detected), it will not duplicate the `$inject` code.

    Note: When using a route resolver you can prefix the resolver's function with `/* @ngInject */` and it will produce properly annotated code, keeping any injected dependencies minification safe.

    ```coffeescript
    # Using @ngInject annotations
    config = ($routeProvider) ->
      $routeProvider.when '/avengers',
        templateUrl: 'avengers.html'
        controller: 'Avengers'
        controllerAs: 'vm'
        resolve: moviesPrepService: (movieService) -> #ngInject
          movieService.getMovies()
    ```

    > Note: Starting from Angular 1.3 you can use the ['ngApp'](https://docs.angularjs.org/api/ng/directive/ngApp) directive's `ngStrictDi` parameter to detect any potentially missing minification safe dependencies. When present the injector will be created in "strict-di" mode causing the application to fail to invoke functions which do not use explicit function annotation (these may not be minification safe). Debugging info will be logged to the console to help track down the offending code. I prefer to only use `ng-strict-di` for debugging purposes only.
    `<body ng-app="APP" ng-strict-di>`

### Use Gulp or Grunt for ng-annotate
###### [Style [Y101](#style-y101)]

**[Back to top](#table-of-contents)**

## Exception Handling

### decorators
###### [Style [Y110](#style-y110)]

  - Use a [decorator](https://docs.angularjs.org/api/auto/service/$provide#decorator), at config time using the [`$provide`](https://docs.angularjs.org/api/auto/service/$provide) service, on the [`$exceptionHandler`](https://docs.angularjs.org/api/ng/service/$exceptionHandler) service to perform custom actions when exceptions occur.

    *Why?*  Provides a consistent way to handle uncaught Angular exceptions for development-time or run-time.

    Note: Another option is to override the service instead of using a decorator. This is a fine option, but if you want to keep the default behavior and extend it a decorator is recommended.

    ```coffeescript
    # recommended
    angular
      .module('blocks.exception')
      .config exceptionConfig

    exceptionConfig.$inject = [ '$provide' ]

    exceptionConfig = ($provide) ->
      $provide.decorator '$exceptionHandler', extendExceptionHandler

    extendExceptionHandler = ($delegate, toastr) ->
      (exception, cause) ->
        $delegate exception, cause
        errorData =
          exception: exception
          cause: cause
        # Could add the error to a service's collection,
        # add errors to $rootScope, log errors to remote web server,
        # or log locally. Or throw hard. It is entirely up to you.
        toastr.error exception.msg, errorData

    extendExceptionHandler.$inject = [
      '$delegate'
      'toastr'
    ]
    ```

### Exception Catchers
###### [Style [Y111](#style-y111)]

  - Create a factory that exposes an interface to catch and gracefully handle exceptions.

    *Why?*  Provides a consistent way to catch exceptions that may be thrown in your code (e.g. during XHR calls or promise failures).

    Note: The exception catcher is good for catching and reacting to specific exceptions from calls that you know may throw one. For example, when making an XHR call to retrieve data from a remote web service and you want to catch any exceptions from that service and react uniquely.

    ```coffeescript
    # recommended
    angular
      .module('blocks.exception')
      .factory 'exception', exception

    exception.$inject = [ 'logger' ]

    exception = (logger) ->
      service = catcher: catcher

      catcher = (message) ->
        (reason) ->
          logger.error message, reason

      service
    ```

### Route Errors
###### [Style [Y112](#style-y112)]

  - Handle and log all routing errors using ['$routeChangeError'](https://docs.angularjs.org/api/ngRoute/service/$route#$routeChangeError).

    *Why?*  Provides a consistent way to handle all routing errors.

    *Why?*  Potentially provides a better user experience if a routing error occurs and you route them to a friendly screen with more details or recovery options.

    ```coffeescript
    # recommended

    handlingRouteChangeError = false

    handleRoutingErrors = ->
      # Route cancellation:
      # On routing error, go to the dashboard.
      # Provide an exit clause if it tries to do it twice.
      $rootScope.$on '$routeChangeError', (event, current, previous, rejection) ->
        if handlingRouteChangeError
          return
        handlingRouteChangeError = true
        destination = current and (current.title or current.name or current.loadedTemplateUrl) or 'unknown target'
        msg = 'Error routing to ' + destination + '. ' + (rejection.msg or '')
        # Optionally log using a custom service or $log.
        # (Don't forget to inject custom service)
        logger.warning msg, [ current ]
        # On routing error, go to another route/state.
        $location.path '/'
    ```

**[Back to top](#table-of-contents)**

## Naming

### Naming Guidelines
###### [Style [Y120](#style-y120)]

  - Use consistent names for all components following a pattern that describes the component's feature then (optionally) its type. My recommended pattern is `feature.type.js`. There are 2 names for most assets:
    * the file name (`avengers.controller.js`)
    * the registered component name with Angular (`AvengersController`)

    *Why?*  Naming conventions help provide a consistent way to find content at a glance. Consistency within the project is vital. Consistency with a team is important. Consistency across a company provides tremendous efficiency.

    *Why?*  The naming conventions should simply help you find your code faster and make it easier to understand.

### Feature File Names
###### [Style [Y121](#style-y121)]

  - Use consistent names for all components following a pattern that describes the component's feature then (optionally) its type. My recommended pattern is `feature.type.js`.

  *Why?*  Provides a consistent way to quickly identify components.

  *Why?*  Provides pattern matching for any automated tasks.

  ```coffeescript
  # common options
  

  # Controllers
  avengers.js.coffee
  avengers.controller.js.coffee
  avengersController.js.coffee

  # Services/Factories
  logger.js.coffee
  logger.service.js.coffee
  loggerService.js.coffee
  ```

  ```coffeescript
  # recommended
  

  # controllers
  avengers.controller.js.coffee
  avengers.controller.spec.js.coffee

  # services/factories
  logger.service.js.coffee
  logger.service.spec.js.coffee

  # constants
  constants.js.coffee

  # module definition
  avengers.module.js.coffee

  # routes
  avengers.routes.js.coffee
  avengers.routes.spec.js.coffee

  # configuration
  avengers.config.js.coffee

  # directives
  avenger-profile.directive.js.coffee
  avenger-profile.directive.spec.js.coffee
  ```

  Note: Another common convention is naming controller files without the word `controller` in the file name such as `avengers.js` instead of `avengers.controller.js`. All other conventions still hold using a suffix of the type. Controllers are the most common type of component so this just saves typing and is still easily identifiable. I recommend you choose 1 convention and be consistent for your team. My preference is `avengers.controller.js`.

  ```coffeescript
  # recommended

  # Controllers
  avengers.js.coffee
  avengers.spec.js.coffee
  ```

### Test File Names
###### [Style [Y122](#style-y122)]

  - Name test specifications similar to the component they test with a suffix of `spec`.

    *Why?*  Provides a consistent way to quickly identify components.

    *Why?*  Provides pattern matching for [karma](http://karma-runner.github.io/) or other test runners.

    ```coffeescript
    # recommended

    avengers.controller.spec.js.coffee
    logger.service.spec.js.coffee
    avengers.routes.spec.js.coffee
    avenger-profile.directive.spec.js.coffee
    ```

### Controller Names
###### [Style [Y123](#style-y123)]

  - Use consistent names for all controllers named after their feature. Use UpperCamelCase for controllers, as they are constructors.

    *Why?*  Provides a consistent way to quickly identify and reference controllers.

    *Why?*  UpperCamelCase is conventional for identifying object that can be instantiated using a constructor.

    ```coffeescript
    # recommended


    # avengers.controller.js
    angular
      .module
      .controller('HeroAvengersController', HeroAvengersController)

    HeroAvengersController = ->
    ```

### Controller Name Suffix
###### [Style [Y124](#style-y124)]

  - Append the controller name with the suffix `Controller`.

    *Why?*  The `Controller` suffix is more commonly used and is more explicitly descriptive.

    ```coffeescript
    # recommended
    # avengers.controller.js.coffee

    angular
      .module
      .controller 'AvengersController', AvengersController

    AvengersController = ->
    ```

### Factory Names
###### [Style [Y125](#style-y125)]

  - Use consistent names for all factories named after their feature. Use camel-casing for services and factories. Avoid prefixing factories and services with `$`.

    *Why?*  Provides a consistent way to quickly identify and reference factories.

    *Why?*  Avoids name collisions with built-in factories and services that use the `$` prefix.

    ```coffeescript
    # recommended
    # logger.service.js.coffee
    angular
      .module
      .factory('logger', logger)

    logger = ->
    ```

### Directive Component Names
###### [Style [Y126](#style-y126)]

  - Use consistent names for all directives using camel-case. Use a short prefix to describe the area that the directives belong (some example are company prefix or project prefix).

    *Why?*  Provides a consistent way to quickly identify and reference components.

    ```coffeescript
    # recommended
    # avenger-profile.directive.js.coffee
    angular
      .module
      .directive('xxAvengerProfile', xxAvengerProfile)

    # usage is <xx-avenger-profile> </xx-avenger-profile>

    xxAvengerProfile = ->
    ```

### Modules
###### [Style [Y127](#style-y127)]

  - When there are multiple modules, the main module file is named `app.module.js` while other dependent modules are named after what they represent. For example, an admin module is named `admin.module.js`. The respective registered module names would be `app` and `admin`.

    *Why?*  Provides consistency for multiple module apps, and for expanding to large applications.

    *Why?*  Provides easy way to use task automation to load all module definitions first, then all other angular files (for bundling).

### Configuration
###### [Style [Y128](#style-y128)]

  - Separate configuration for a module into its own file named after the module. A configuration file for the main `app` module is named `app.config.js` (or simply `config.js`). A configuration for a module named `admin.module.js` is named `admin.config.js`.

    *Why?*  Separates configuration from module definition, components, and active code.

    *Why?*  Provides an identifiable place to set configuration for a module.

### Routes
###### [Style [Y129](#style-y129)]

  - Separate route configuration into its own file. Examples might be `app.route.js` for the main module and `admin.route.js` for the `admin` module. Even in smaller apps I prefer this separation from the rest of the configuration.

**[Back to top](#table-of-contents)**

## Application Structure LIFT Principle
### LIFT
###### [Style [Y140](#style-y140)]

  - Structure your app such that you can `L`ocate your code quickly, `I`dentify the code at a glance, keep the `F`lattest structure you can, and `T`ry to stay DRY. The structure should follow these four basic guidelines.

    *Why LIFT?*  Provides a consistent structure that scales well, is modular, and makes it easier to increase developer efficiency by finding code quickly. Another way to check your app structure is to ask yourself: How quickly can you open and work in all of the related files for a feature?

    When I find my structure is not feeling comfortable, I go back and revisit these LIFT guidelines

    1. `L`ocating our code is easy
    2. `I`dentify code at a glance
    3. `F`lat structure as long as we can
    4. `T`ry to stay DRY (Don’t Repeat Yourself) or T-DRY

### Locate
###### [Style [Y141](#style-y141)]

  - Make locating your code intuitive, simple and fast.

    *Why?*  I find this to be very important for a project. If the team cannot find the files they need to work on quickly, they will not be able to work as efficiently as possible, and the structure needs to change. You may not know the file name or where its related files are, so putting them in the most intuitive locations and near each other saves a lot of time. A descriptive folder structure can help with this.

    ```
    /bower_components
    /client
      /app
        /avengers
        /blocks
          /exception
          /logger
        /core
        /dashboard
        /data
        /layout
        /widgets
      /content
      index.html
    .bower.json
    ```

### Identify
###### [Style [Y142](#style-y142)]

  - When you look at a file you should instantly know what it contains and represents.

    *Why?*  You spend less time hunting for code, and become more efficient. If this means you want longer file names, then so be it. Be descriptive with file names and keeping the contents of the file to exactly 1 component. Avoid files with multiple controllers, multiple services, or a mixture. There are deviations of the 1 per file rule when I have a set of very small features that are all related to each other, they are still easily identifiable.

### Flat
###### [Style [Y143](#style-y143)]

  - Keep a flat folder structure as long as possible. When you get to 7+ files, begin considering separation.

    *Why?*  In a folder structure there is no hard and fast number rule, but when a folder has 7-10 files, that may be time to create subfolders. Base it on your comfort level. Use a flatter structure until there is an obvious value (to help the rest of LIFT) in creating a new folder.

### T-DRY (Try to Stick to DRY)
###### [Style [Y144](#style-y144)]

  - Be DRY, but don't go nuts and sacrifice readability.

    *Why?*  Being DRY is important, but not crucial if it sacrifices the others in LIFT, which is why I call it T-DRY.

**[Back to top](#table-of-contents)**

## Application Structure

### Overall Guidelines
###### [Style [Y150](#style-y150)]

  - Have a near term view of implementation and a long term vision. In other words, start small but keep in mind on where the app is heading down the road. All of the app's code goes in a root folder named `app`. All content is 1 feature per file. Each controller, service, module, view is in its own file. All 3rd party vendor scripts are stored in another root folder and not in the `app` folder. I didn't write them and I don't want them cluttering my app (`bower_components`, `scripts`, `lib`).

    Note: Find more details and reasoning behind the structure at [this original post on application structure](http://www.johnpapa.net/angular-app-structuring-guidelines/).

### Layout
###### [Style [Y151](#style-y151)]

  - Place components that define the overall layout of the application in a folder named `layout`. These may include a shell view and controller may act as the container for the app, navigation, menus, content areas, and other regions.

    *Why?*  Organizes all layout in a single place re-used throughout the application.

### Folders-by-Feature Structure
###### [Style [Y152](#style-y152)]

  - Create folders named for the feature they represent. When a folder grows to contain more than 7 files, start to consider creating a folder for them. Your threshold may be different, so adjust as needed.

    *Why?*  A developer can locate the code, identify what each file represents at a glance, the structure is flat as can be, and there is no repetitive nor redundant names.

    *Why?*  The LIFT guidelines are all covered.

    *Why?*  Helps reduce the app from becoming cluttered through organizing the content and keeping them aligned with the LIFT guidelines.

    *Why?*  When there are a lot of files (10+) locating them is easier with a consistent folder structures and more difficult in flat structures.

    ```coffeescript
    # recommended
    # folders-by-feature
    
    app/
        app.module.js.coffee
        app.config.js.coffee
        components/
            calendar.directive.js.coffee
            calendar.directive.html
            user-profile.directive.js.coffee
            user-profile.directive.html
        layout/
            shell.html
            shell.controller.js.coffee
            topnav.html
            topnav.controller.js.coffee
        people/
            attendees.html
            attendees.controller.js.coffee
            people.routes.js.coffee
            speakers.html
            speakers.controller.js.coffee
            speaker-detail.html
            speaker-detail.controller.js.coffee
        services/
            data.service.js.coffee
            localstorage.service.js.coffee
            logger.service.js.coffee
            spinner.service.js.coffee
        sessions/
            sessions.html
            sessions.controller.js.coffee
            sessions.routes.js.coffee
            session-detail.html
            session-detail.controller.js.coffee
    ```

      Note: Do not structure your app using folders-by-type. This requires moving to multiple folders when working on a feature and gets unwieldy quickly as the app grows to 5, 10 or 25+ views and controllers (and other features), which makes it more difficult than folder-by-feature to locate files.

    ```coffeescript
    # avoid 
    # folders-by-type

    app/
        app.module.js.coffee
        app.config.js.coffee
        app.routes.js.coffee
        directives.js.coffee
        controllers/
            attendees.js.coffee
            session-detail.js.coffee
            sessions.js.coffee
            shell.js.coffee
            speakers.js.coffee
            speaker-detail.js.coffee
            topnav.js.coffee
        directives/
            calendar.directive.js.coffee
            calendar.directive.html
            user-profile.directive.js.coffee
            user-profile.directive.html
        services/
            dataservice.js.coffee
            localstorage.js.coffee
            logger.js.coffee
            spinner.js.coffee
        views/
            attendees.html
            session-detail.html
            sessions.html
            shell.html
            speakers.html
            speaker-detail.html
            topnav.html
    ```

**[Back to top](#table-of-contents)**

## Modularity

### Many Small, Self Contained Modules
###### [Style [Y160](#style-y160)]

  - Create small modules that encapsulate one responsibility.

    *Why?*  Modular applications make it easy to plug and go as they allow the development teams to build vertical slices of the applications and roll out incrementally. This means we can plug in new features as we develop them.

### Create an App Module
###### [Style [Y161](#style-y161)]

  - Create an application root module whose role is pull together all of the modules and features of your application. Name this for your application.

    *Why?*  Angular encourages modularity and separation patterns. Creating an application root module whose role is to tie your other modules together provides a very straightforward way to add or remove modules from your application.

### Keep the App Module Thin
###### [Style [Y162](#style-y162)]

  - Only put logic for pulling together the app in the application module. Leave features in their own modules.

    *Why?*  Adding additional roles to the application root to get remote data, display views, or other logic not related to pulling the app together muddies the app module and make both sets of features harder to reuse or turn off.

    *Why?*  The app module becomes a manifest that describes which modules help define the application.

### Feature Areas are Modules
###### [Style [Y163](#style-y163)]

  - Create modules that represent feature areas, such as layout, reusable and shared services, dashboards, and app specific features (e.g. customers, admin, sales).

    *Why?*  Self contained modules can be added to the application with little or no friction.

    *Why?*  Sprints or iterations can focus on feature areas and turn them on at the end of the sprint or iteration.

    *Why?*  Separating feature areas into modules makes it easier to test the modules in isolation and reuse code.

### Reusable Blocks are Modules
###### [Style [Y164](#style-y164)]

  - Create modules that represent reusable application blocks for common services such as exception handling, logging, diagnostics, security, and local data stashing.

    *Why?*  These types of features are needed in many applications, so by keeping them separated in their own modules they can be application generic and be reused across applications.

### Module Dependencies
###### [Style [Y165](#style-y165)]

  - The application root module depends on the app specific feature modules and any shared or reusable modules.

    ![Modularity and Dependencies](https://raw.githubusercontent.com/johnpapa/angular-styleguide/master/assets/modularity-1.png)

    *Why?*  The main app module contains a quickly identifiable manifest of the application's features.

    *Why?*  Each feature area contains a manifest of what it depends on, so it can be pulled in as a dependency in other applications and still work.

    *Why?*  Intra-App features such as shared data services become easy to locate and share from within `app.core` (choose your favorite name for this module).

    Note: This is a strategy for consistency. There are many good options here. Choose one that is consistent, follows Angular's dependency rules, and is easy to maintain and scale.

    > My structures vary slightly between projects but they all follow these guidelines for structure and modularity. The implementation may vary depending on the features and the team. In other words, don't get hung up on an exact like-for-like structure but do justify your structure using consistency, maintainability, and efficiency in mind.

    > In a small app, you can also consider putting all the shared dependencies in the app module where the feature modules have no direct dependencies. This makes it easier to maintain the smaller application, but makes it harder to reuse modules outside of this application.

**[Back to top](#table-of-contents)**

## Startup Logic

### Configuration
###### [Style [Y170](#style-y170)]

  - Inject code into [module configuration](https://docs.angularjs.org/guide/module#module-loading-dependencies) that must be configured before running the angular app. Ideal candidates include providers and constants.

    *Why?*  This makes it easier to have a less places for configuration.

  ```coffeescript
  angular
    .module('app')
    .config(configure)

  configure.$inject = [
    'routerHelperProvider'
    'exceptionHandlerProvider'
    'toastr'
  ]

  configure = (routerHelperProvider, exceptionHandlerProvider, toastr) ->
    exceptionHandlerProvider.configure config.appErrorPrefix
    configureStateHelper()

    toastr.options.timeOut = 4000
    toastr.options.positionClass = 'toast-bottom-right'

    configureStateHelper = ->
      routerHelperProvider.configure docTitle: 'NG-Modular: '

  ```

### Run Blocks
###### [Style [Y171](#style-y171)]

  - Any code that needs to run when an application starts should be declared in a factory, exposed via a function, and injected into the [run block](https://docs.angularjs.org/guide/module#module-loading-dependencies).

  *Why?*  Code directly in a run block can be difficult to test. Placing in a factory makes it easier to abstract and mock.

  ```coffeescript
  angular
      .module('app')
      .run(runBlock)

  runBlock.$inject = [
    'authenticator'
    'translator'
  ]

  runBlock = (authenticator, translator) ->
    authenticator.initialize()
    translator.initialize()
  ```

**[Back to top](#table-of-contents)**

## Angular $ Wrapper Services

### $document and $window
###### [Style [Y180](#style-y180)]

  - Use [$document](https://docs.angularjs.org/api/ng/service/$document) and [$window](https://docs.angularjs.org/api/ng/service/$window) instead of `document` and `window`.

  *Why?*  These services are wrapped by Angular and more easily testable than using document and window in tests. This helps you avoid having to mock document and window yourself.

### $timeout and $interval
###### [Style [Y181](#style-y181)]

  - Use [$timeout](https://docs.angularjs.org/api/ng/service/$timeout) and [$interval](https://docs.angularjs.org/api/ng/service/$interval) instead of `setTimeout` and `setInterval` .

  *Why?*  These services are wrapped by Angular and more easily testable and handle Angular's digest cycle thus keeping data binding in sync.

**[Back to top](#table-of-contents)**

## Testing
Unit testing helps maintain clean code, as such I included some of my recommendations for unit testing foundations with links for more information.

### Write Tests with Stories
###### [Style [Y190](#style-y190)]

  - Write a set of tests for every story. Start with an empty test and fill them in as you write the code for the story.

    *Why?*  Writing the test descriptions helps clearly define what your story will do, will not do, and how you can measure success.

    ```coffeescript
    it 'should have Avengers controller', ->
      # TODO

    it 'should find 1 Avenger when filtered by name', ->
      # TODO

    it 'should have 10 Avengers', ->
      # TODO (mock data?)

    it 'should return Avengers via XHR', ->
      # TODO ($httpBackend?)
    ```

### Testing Library
###### [Style [Y191](#style-y191)]

  - Use [Jasmine](http://jasmine.github.io/) 

    *Why?*  Both Jasmine is widely used in the Angular community. It is stable, well maintained, and provides robust testing features.

### Test Runner
###### [Style [Y192](#style-y192)]

  - Use [Teaspoon](https://github.com/modeset/teaspoon) as a test runner.

### Stubbing and Spying
###### [Style [Y193](#style-y193)]


### Headless Browser
###### [Style [Y194](#style-y194)]

  - Use [PhantomJS](http://phantomjs.org/) to run your tests on a server.

    *Why?*  PhantomJS is a headless browser that helps run your tests without needing a "visual" browser. So you do not have to install Chrome, Safari, IE, or other browsers on your server.

    Note: You should still test on all browsers in your environment, as appropriate for your target audience.

### Code Analysis
###### [Style [Y195](#style-y195)]

  - Run JSHint on your tests.

    *Why?*  Tests are code. JSHint can help identify code quality issues that may cause the test to work improperly.

### Alleviate Globals for JSHint Rules on Tests
###### [Style [Y196](#style-y196)]


### Organizing Tests
###### [Style [Y197](#style-y197)]

  - Place unit test files (specs) side-by-side with your client code. Place specs that cover server integration or test multiple components in a separate `tests` folder.

    *Why?*  Unit tests have a direct correlation to a specific component and file in source code.

    *Why?*  It is easier to keep them up to date since they are always in sight. When coding whether you do TDD or test during development or test after development, the specs are side-by-side and never out of sight nor mind, and thus more likely to be maintained which also helps maintain code coverage.

    *Why?*  When you update source code it is easier to go update the tests at the same time.

    *Why?*  Placing them side-by-side makes it easy to find them and easy to move them with the source code if you move the source.

    *Why?*  Having the spec nearby makes it easier for the source code reader to learn how the component is supposed to be used and to discover its known limitations.

    *Why?*  Separating specs so they are not in a distributed build is easy with grunt or gulp.

    ```
    /src/client/app/customers/customer-detail.controller.js.coffee
                             /customer-detail.controller.spec.js.coffee
                             /customers.controller.js.coffee
                             /customers.controller.spec.js.coffee
                             /customers.module.js.coffee
                             /customers.route.js.coffee
                             /customers.route.spec.js.coffee
    ```

**[Back to top](#table-of-contents)**

## Constants

### Vendor Globals
###### [Style [Y240](#style-y240)]

  - Create an Angular Constant for vendor libraries' global variables.

    *Why?*  Provides a way to inject vendor libraries that otherwise are globals. This improves code testability by allowing you to more easily know what the dependencies of your components are (avoids leaky abstractions). It also allows you to mock these dependencies, where it makes sense.

    ```coffeescript
    # constants.js.coffee

    do ->
      'use strict'
      angular
        .module('app.core')
        .constant('toastr', toastr)
        .constant 'moment', moment
    ```

###### [Style [Y241](#style-y241)]

  - Use constants for values that do not change and do not come from another service. When constants are used only for a module that may be reused in multiple applications, place constants in a file per module named after the module. Until this is required, keep constants in the main module in a `constants.js` file.

    *Why?*  A value that may change, even infrequently, should be retrieved from a service so you do not have to change the source code. For example, a url for a data service could be placed in a constants but a better place would be to load it from a web service.

    *Why?*  Constants can be injected into any angular component, including providers.

    *Why?*  When an application is separated into modules that may be reused in other applications, each stand-alone module should be able to operate on its own including any dependent constants.

    ```coffeescript
  # Constants used by the entire app
  angular
    .module('app.core')
    .constant 'moment', moment

  # Constants used only by the sales module
  angular
    .module('app.sales')
    .constant 'events',
      ORDER_CREATED: 'event_order_created'
      INVENTORY_DEPLETED: 'event_inventory_depleted'
    ```

**[Back to top](#table-of-contents)**

## Routing
Client-side routing is important for creating a navigation flow between views and composing views that are made of many smaller templates and directives.

###### [Style [Y270](#style-y270)]

  - Use the [AngularUI Router](http://angular-ui.github.io/ui-router/) for client-side routing.

    *Why?*  UI Router offers all the features of the Angular router plus a few additional ones including nested routes and states.

    *Why?*  The syntax is quite similar to the Angular router and is easy to migrate to UI Router.

  - Note: You can use a provider such as the `routerHelperProvider` shown below to help configure states across files, during the run phase.

    ```coffeescript
    # customers.routes.js

    angular
      .module('app.customers')
      .run appRun

    # ngInject
    appRun = (routerHelper) ->
      routerHelper.configureStates getStates()

    getStates = ->
      [ {
        state: 'customer'
        config:
          abstract: true
          template: '<ui-view class="shuffle-animation"/>'
          url: '/customer'
      } ]
    ```

    ```coffeescript
    # routerHelperProvider.js.coffee
    angular
      .module('blocks.router')
      .provider 'routerHelper', routerHelperProvider

    routerHelperProvider.$inject = [
      '$locationProvider'
      '$stateProvider'
      '$urlRouterProvider'
    ]

    # @ngInject
    routerHelperProvider = ($locationProvider, $stateProvider, $urlRouterProvider) ->
      # jshint validthis:true
      @$get = RouterHelper
      $locationProvider.html5Mode true
      RouterHelper.$inject = [ '$state' ]

      # @ngInject
      RouterHelper = ($state) ->
        hasOtherwise = false
        service = 
          configureStates: configureStates
          getStates: getStates

        configureStates = (states, otherwisePath) ->
          states.forEach (state) ->
            $stateProvider.state state.state, state.config
          if otherwisePath and !hasOtherwise
            hasOtherwise = true
            $urlRouterProvider.otherwise otherwisePath

        getStates = ->
          $state.get()

        service
    ```

###### [Style [Y271](#style-y271)]

  - Define routes for views in the module where they exist. Each module should contain the routes for the views in the module.

    *Why?*  Each module should be able to stand on its own.

    *Why?*  When removing a module or adding a module, the app will only contain routes that point to existing views.

    *Why?*  This makes it easy to enable or disable portions of an application without concern over orphaned routes.

**[Back to top](#table-of-contents)**

## Filters

###### [Style [Y420](#style-y420)]

  - Avoid using filters for scanning all properties of a complex object graph. Use filters for select properties.

    *Why?*  Filters can easily be abused and negatively effect performance if not used wisely, for example when a filter hits a large and deep object graph.

**[Back to top](#table-of-contents)**

## Angular docs
For anything else check the [Angular documentation](//docs.angularjs.org/api).

## License

_tldr; Use this guide. Attributions are appreciated._

### Copyright

Copyright (c) 2014-2015 [John Papa](http://johnpapa.net)

### (The MIT License)
Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

**[Back to top](#table-of-contents)**
