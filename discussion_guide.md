<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>

# COMPARE AND DISCUSS
```ruby
describe '#add_chapter' do
  let(:author) = Author.new
  let(:publisher) = Publisher.new
  let(:book) = Book.new(author, publisher)
  before { book.add_chapter("Early Years") }

  it "has one chapter" do
    expect(book.chapters.size).to eq(1)
  end

  it "records the name of the chapter" do
    expect(book.chapters.first.name).to eq("Early Years")
  end
end
```
vs.
```ruby
describe '#add_chapter' do
  let(:author) = Author.new
  let(:publisher) = Publisher.new
  let(:subject) = Book.new(author, publisher)

  before { subject.add_chapter("Early Years") }

  it "has one chapter" do
    expect(subject.chapters.size).to eq(1)
  end

  it "records the name of the chapter" do
    expect(subject.chapters.first.name).to eq("Early Years")
  end
end
```


<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>

# COMPARE AND DISCUSS
```ruby
class PlayerTest < Minitest::Test

  def test_display_name
    subject = Player.new(                       # Given
      name: "pants",
      email: "pantsmail")

    result = subject.diplay_name                # When

    assert_equal "pants <pantsmail>", result    # Then
  end

end
```
vs.
```ruby
class PlayerTest < Minitest::Test

  def test_display_name
    player = Player.new(                                            # Setup
      name: "John Smith",
      phone: "(555) 123-4567",
      email: "smith@mail.com")

    assert_equal player.display_name, "John Smith <smith@mail.com>" # Assert
  end

end
```
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
# COMPARE AND DISCUSS
###### [Style [Y031](http://github.com/bolpin/angular-style/README.md#style-y031)]
###### [Style [Y032](http://github.com/bolpin/angular-style/README.md#style-y032)]

```coffeescript
Customer = ($scope) ->
  $scope.name = {}

  $scope.sendMessage = ->
```
vs.

```coffeescript
Customer = ->
  @name = {}

  @sendMessage = ->
```
vs.

```coffeescript
Customer = ->
  vm = this
  vm.name = {}

  vm.sendMessage = ->
```
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
# COMPARE AND DISCUSS
###### [Style [Y034](http://github.com/bolpin/angular-style/README.md#style-y034)]


  ```coffeescript
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

vs.

  ```coffeescript

  # function declarations, bindable members up top
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

<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
# COMPARE AND DISCUSS
###### [Style [Y033](http://github.com/bolpin/angular-style/README.md#style-y033)]


```coffeescript
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

vs.

```coffeescript
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
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>

What about one-liners?

```coffeescript
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

vs.

```coffeescript
Sessions = (dataservice) ->
  vm = this
  vm.gotoSession = gotoSession
  vm.refresh = dataservice.refresh
  # 1 liner
  vm.search = search
  vm.sessions = []
  vm.title = 'Sessions'
```

<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
# COMPARE AND DISCUSS
###### [Style [Y020](http://github.com/bolpin/angular-style/README.md#style-y001)]

```coffeescript
angular
  .module('app', [ 'ngRoute' ])
  .controller('SomeController', SomeController)
  .factory 'someFactory', someFactory

SomeController = ->
  #...

someFactory = ->
  #...
```
vs.

```coffeescript
# app.module.js.coffee
angular
  .module 'app', [ 'ngRoute' ]
```

```coffeescript
# someController.js.coffee
angular
  .module('app')
  .controller 'SomeController', SomeController

SomeController ->
  #...
```

```coffeescript
# someFactory.js.coffee
angular
  .module('app')
  .factory 'someFactory', someFactory

someFactory ->
  #...
```
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>

# DISCUSS
###### [Style [Y023](http://github.com/bolpin/angular-style/README.md#style-y023)]

Good?

  - Use `angular.module('app', [])` to set a module.
  - Use `angular.module('app')` to get a module.

<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
# COMPARE AND DISCUSS
###### [Style [Y030](http://github.com/bolpin/angular-style/README.md#style-y030)]


```html
<div ng-controller="Customer">
    {{ name }}
</div>
```

vs.

```html
<div ng-controller="Customer as customer">
    {{ customer.name }}
</div>
```

<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
# COMPARE AND DISCUSS
###### [Style [Y035](http://github.com/bolpin/angular-style/README.md#style-y035)]

  ```coffeescript

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

  vs.

  ```coffeescript

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

<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
# COMPARE AND DISCUSS
###### [Style [Y037](http://github.com/bolpin/angular-style/README.md#style-y037)]


### Re-use controllers amongst multiple views?
vs.
### Move reusable logic to factories and keep the controller simple and focused on its view?


<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
# COMPARE AND DISCUSS
###### [Style [Y038](http://github.com/bolpin/angular-style/README.md#style-y038)]

 ```coffeescript
  # route-config.js.coffee
  angular
    .module('app')
    .config config

  config = ($routeProvider) ->
    $routeProvider.when '/avengers', templateUrl: 'avengers.html'
  ```

  ```html
  <!-- avengers.html -->
  <div ng-controller="Avengers as vm">
  </div>
  ```

  vs.

  ```coffeescript
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

<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
###### [Style [Y040](http://github.com/bolpin/angular-style/README.md#style-y040)]
  How many instances of given service can there be per injector? (Are services singletons?)
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>

# COMPARE AND DISCUSS
###### [Style [Y040](http://github.com/bolpin/angular-style/README.md#style-y040)]


  ```coffeescript
  # service
  angular
    .module('app')
    .service 'logger', logger

  logger = ->
    @logError = (msg) ->
      #...
  ```

vs.

  ```coffeescript
  # factory
  angular
    .module('app')
    .factory 'logger', logger

  logger = ->
    logError: (msg) ->
      #...
  ```


<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
# DISCUSS
###### [Style [Y050](http://github.com/bolpin/angular-style/README.md#style-y050)]

  - Factories should have a [single responsibility](http://en.wikipedia.org/wiki/Single_responsibility_principle), that is encapsulated by its context.
  - Once a factory begins to exceed that singular purpose, a new factory should be created.

<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
# DISCUSS
###### [Style [Y051](http://github.com/bolpin/angular-style/README.md#style-y051)]

  - Factories are singletons and return an object that contains _________________.
  - How does a factory differ from a service?
  - How does a factory differ from a provider?


<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
# COMPARE AND DISCUSS
###### [Style [Y052](http://github.com/bolpin/angular-style/README.md#style-y052)]

```coffeescript
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
vs.

```coffeescript
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


<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
# COMPARE AND DISCUSS
###### [Style [Y053](http://github.com/bolpin/angular-style/README.md#style-y053)]

  ```coffeescript
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
vs.

  ```coffeescript
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


<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
# DISCUSS
###### [Style [Y060](http://github.com/bolpin/angular-style/README.md#style-y060)]

  ```coffeescript
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

are implementation details hidden?

  ```coffeescript
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

<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
# DISCUSS
###### [Style [Y061](http://github.com/bolpin/angular-style/README.md#style-y061)]

returning a promise:

  ```coffeescript
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


<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
# DISCUSS
###### [Style [Y070](http://github.com/bolpin/angular-style/README.md#style-y070)]

  Create one directive per file? Why?

  ```coffeescript
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

  vs.

  ```coffeescript
  # calendarRange.directive.js.coffee
  # @desc order directive that is specific to the order module at a company named Acme
  # @example <div acme-order-calendar-range></div>

  angular
    .module('sales.order')
    .directive 'acmeOrderCalendarRange', orderCalendarRange

  orderCalendarRange = ->
  ```

good or bad?

  ```coffeescript
  # customerInfo.directive.js.coffee
  # @desc sales directive that can be used anywhere across the sales app at a company named Acme
  # @example <div acme-sales-customer-info></div>

  angular
    .module('sales.widgets')
    .directive 'acmeSalesCustomerInfo', salesCustomerInfo

  salesCustomerInfo = ->
  ```

good or bad?

  ```coffeescript
  # spinner.directive.js.coffee
  # @desc spinner directive that can be used anywhere across apps at a company named Acme
  # @example <div acme-shared-spinner></div>

  angular
    .module('shared.widgets')
    .directive 'acmeSharedSpinner', sharedSpinner

  sharedSpinner = ->
  ```

<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
# COMPARE AND DISCUSS
###### [Style [Y072](http://github.com/bolpin/angular-style/README.md#style-y072)]

  - When manipulating the DOM directly, use a ______________.

    *Why?*  DOM manipulation can be difficult to test, debug, and there are often better ways (e.g. _______, _______, _______)

<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
# COMPARE AND DISCUSS
###### [Style [Y074](http://github.com/bolpin/angular-style/README.md#style-y074)]

  - When creating a directive that makes sense as a stand-alone element, allow restrict `_`
  - Implement as an _______  when it's stand-alone and as an ______  when it enhances its existing DOM element.


  ```html
  <div class="my-calendar-range"></div>
  ```

  ```coffeescript
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

vs.

  ```html
  <my-calendar-range></my-calendar-range>
  <div my-calendar-range></div>
  ```

  ```coffeescript
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

<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
# DISCUSS
###### [Style [Y075](http://github.com/bolpin/angular-style/README.md#style-y075)]

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

How about this?

  ```coffeescript
  # Alternative to above example
  linkFunc = (scope, el, attr, vm) ->
    console.log 'LINK: scope.min = %s *** should be undefined', scope.min
    console.log 'LINK: scope.max = %s *** should be undefined', scope.max
    console.log 'LINK: vm.min = %s', vm.min
    console.log 'LINK: vm.max = %s', vm.max
  ```

<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
# DISCUSS
###### [Style [Y076](http://github.com/bolpin/angular-style/README.md#style-y076)]

  bindToController

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

<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
# COMPARE AND DISCUSS
###### [Style [Y080](http://github.com/bolpin/angular-style/README.md#style-y080)]

  - Where to put start-up logic for a controller?

  ```coffeescript
  Avengers = (dataservice) ->
    vm = this
    vm.avengers = []
    vm.title = 'Avengers'

    dataservice.getAvengers().then (data) ->
      vm.avengers = data
      vm.avengers
  ```
vs.

  ```coffeescript
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

  - When a controller depends on a promise to be resolved before the controller is activated, resolve those dependencies in the `______________` before the controller logic is executed. If you need to conditionally cancel a route before the controller is activated, use a _____________.

  - Use a ________________ when you want to decide to cancel the route before ever transitioning to the View.


  ```coffeescript
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
vs.

  ```coffeescript
  # better?
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

  vs.

  ```coffeescript
  # even better?
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

<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
# COMPARE AND DISCUSS
###### [Style [Y091](http://github.com/bolpin/angular-style/README.md#style-y091)]

identify dependencies

  - Use ________ to manually identify your dependencies for Angular components.


```coffeescript
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

vs.

```coffeescript
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

  When your function is below a return statement the `$inject` may be unreachable (this may happen in a directive). How can we solve this? 

```coffeescript
# inside a directive definition
outer = ->
ddo =
  controller: DashboardPanelController
  controllerAs: 'vm'

DashboardPanelController = (logger) ->

return ddo
DashboardPanelController.$inject = [ 'logger' ]
```

vs.

```coffeescript
# outside a directive definition
outer = ->
  ddo =
    controller: DashboardPanelController
    controllerAs: 'vm'
  ddo

DashboardPanelController.$inject = [ 'logger' ]

DashboardPanelController = (logger) ->
```






























<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>




























### Route Resolver Dependencies
# COMPARE AND DISCUSS
###### [Style [Y092](http://github.com/bolpin/angular-style/README.md#style-y092)]

good?
```coffeescript
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



<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
# DISCUSS
###### [Style [Y100](http://github.com/bolpin/angular-style/README.md#style-y100)]
### ng-annotate

Is the following code using minification-safe dependencies?

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



<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
# DISCUSS
###### [Style [Y110](http://github.com/bolpin/angular-style/README.md#style-y110)]
### decorators

good?

```coffeescript
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

<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
# DISCUSS
###### [Style [Y111](http://github.com/bolpin/angular-style/README.md#style-y111)]

### Exception Catchers

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


<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
# DISCUSS
###### [Style [Y112](http://github.com/bolpin/angular-style/README.md#style-y112)]
### Route Errors


good?
```coffeescript
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


<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
# COMPARE AND DISCUSS
###### [Style [Y120](http://github.com/bolpin/angular-style/README.md#style-y120)]
### Naming

  - Use consistent names for all components following a pattern that describes the component's feature then (optionally) its type. My recommended pattern is `feature.type.js`. There are 2 names for most assets:
    * the file name (`avengers.controller.js`)
    * the registered component name with Angular (`AvengersController`)

<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
# DISCUSS
###### [Style [Y121](http://github.com/bolpin/angular-style/README.md#style-y121)]
### Feature File Names

  - Use consistent names for all components following a pattern that describes the component's feature then (optionally) its type. My recommended pattern is `feature.type.js`.

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
or

```coffeescript

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
or

```coffeescript

# Controllers
avengers.js.coffee
avengers.spec.js.coffee
```

<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
# DISCUSS
###### [Style [Y122](http://github.com/bolpin/angular-style/README.md#style-y122)]
### Test File Names



```coffeescript

avengers.controller.spec.js.coffee
logger.service.spec.js.coffee
avengers.routes.spec.js.coffee
avenger-profile.directive.spec.js.coffee
```

<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
# DISCUSS
###### [Style [Y123](http://github.com/bolpin/angular-style/README.md#style-y123)]
### Controller Names


```coffeescript

# avengers.controller.js
angular
  .module
  .controller('HeroAvengersController', HeroAvengersController)

HeroAvengersController = ->
```

<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
# DISCUSS
###### [Style [Y124](http://github.com/bolpin/angular-style/README.md#style-y124)]
### Controller Name Suffix

```coffeescript
# avengers.controller.js.coffee

angular
  .module
  .controller 'AvengersController', AvengersController

AvengersController = ->
```

<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
# DISCUSS
###### [Style [Y125](http://github.com/bolpin/angular-style/README.md#style-y125)]
### Factory Names

```coffeescript
# logger.service.js.coffee
angular
  .module
  .factory('logger', logger)

logger = ->
```

<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
# DISCUSS
###### [Style [Y126](http://github.com/bolpin/angular-style/README.md#style-y126)]
### Directive Component Names


```coffeescript
# avenger-profile.directive.js.coffee
angular
  .module
  .directive('xxAvengerProfile', xxAvengerProfile)

# usage is <xx-avenger-profile> </xx-avenger-profile>

xxAvengerProfile = ->
```



<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
# DISCUSS
###### [Style [Y128](http://github.com/bolpin/angular-style/README.md#style-y128)]
### Configuration

  - Separate configuration for a module into its own file named after the module. A configuration file for the main `app` module is named `app.config.js` (or simply `config.js`). A configuration for a module named `admin.module.js` is named `admin.config.js`.


<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
# DISCUSS
###### [Style [Y129](http://github.com/bolpin/angular-style/README.md#style-y129)]
### Routes

  - Separate route configuration into its own file. Examples might be `app.route.js` for the main module and `admin.route.js` for the `admin` module. Even in smaller apps I prefer this separation from the rest of the configuration.


<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
# DISCUSS
###### [Style [Y140](http://github.com/bolpin/angular-style/README.md#style-y140)]
## Application Structure LIFT Principle

  - Structure your app such that you can `L`______ your code quickly, `I`_________ the code at a glance, keep the `F`______est structure you can, and `T`______ to stay DRY. The structure should follow these four basic guidelines.




<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
# DISCUSS
###### [Style [Y141](http://github.com/bolpin/angular-style/README.md#style-y141)]
### Locate

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

<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
## Application Structure

### Overall Guidelines
# DISCUSS
###### [Style [Y150](http://github.com/bolpin/angular-style/README.md#style-y150)]

  - Have a near term view of implementation and a long term vision. In other words, start small but keep in mind on where the app is heading down the road. All of the app's code goes in a root folder named `app`. All content is 1 feature per file. Each controller, service, module, view is in its own file. All 3rd party vendor scripts are stored in another root folder and not in the `app` folder. I didn't write them and I don't want them cluttering my app (`bower_components`, `scripts`, `lib`).

    Note: Find more details and reasoning behind the structure at [this original post on application structure](http://www.johnpapa.net/angular-app-structuring-guidelines/).

<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
### Layout
# DISCUSS
###### [Style [Y151](http://github.com/bolpin/angular-style/README.md#style-y151)]

  - Place components that define the overall layout of the application in a folder named `layout`. These may include a shell view and controller may act as the container for the app, navigation, menus, content areas, and other regions.

    *Why?*  Organizes all layout in a single place re-used throughout the application.

<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
### Folders-by-Feature Structure
# DISCUSS
###### [Style [Y152](http://github.com/bolpin/angular-style/README.md#style-y152)]


```coffeescript
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

## vs.

```coffeescript
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


<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
### Many Small, Self Contained Modules
# COMPARE AND DISCUSS
###### [Style [Y160](http://github.com/bolpin/angular-style/README.md#style-y160)]

  - Create small modules that encapsulate one responsibility.

    *Why?*  Modular applications make it easy to plug and go as they allow the development teams to build vertical slices of the applications and roll out incrementally. This means we can plug in new features as we develop them.

<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
### Create an App Module
# COMPARE AND DISCUSS
###### [Style [Y161](http://github.com/bolpin/angular-style/README.md#style-y161)]

  - Create an application root module whose role is pull together all of the modules and features of your application. Name this for your application.

    *Why?*  Angular encourages modularity and separation patterns. Creating an application root module whose role is to tie your other modules together provides a very straightforward way to add or remove modules from your application.

<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
### Keep the App Module Thin
# COMPARE AND DISCUSS
###### [Style [Y162](http://github.com/bolpin/angular-style/README.md#style-y162)]

  - Only put logic for pulling together the app in the application module. Leave features in their own modules.

    *Why?*  Adding additional roles to the application root to get remote data, display views, or other logic not related to pulling the app together muddies the app module and make both sets of features harder to reuse or turn off.

    *Why?*  The app module becomes a manifest that describes which modules help define the application.

<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
### Feature Areas are Modules
# COMPARE AND DISCUSS
###### [Style [Y163](http://github.com/bolpin/angular-style/README.md#style-y163)]

  - Create modules that represent feature areas, such as layout, reusable and shared services, dashboards, and app specific features (e.g. customers, admin, sales).

    *Why?*  Self contained modules can be added to the application with little or no friction.

    *Why?*  Sprints or iterations can focus on feature areas and turn them on at the end of the sprint or iteration.

    *Why?*  Separating feature areas into modules makes it easier to test the modules in isolation and reuse code.

<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
### Reusable Blocks are Modules
# COMPARE AND DISCUSS
###### [Style [Y164](http://github.com/bolpin/angular-style/README.md#style-y164)]

  - Create modules that represent reusable application blocks for common services such as exception handling, logging, diagnostics, security, and local data stashing.

    *Why?*  These types of features are needed in many applications, so by keeping them separated in their own modules they can be application generic and be reused across applications.

<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
### Module Dependencies
# COMPARE AND DISCUSS
###### [Style [Y165](http://github.com/bolpin/angular-style/README.md#style-y165)]

  - The application root module depends on the app specific feature modules and any shared or reusable modules.

    ![Modularity and Dependencies](https://raw.githubusercontent.com/johnpapa/angular-styleguide/master/assets/modularity-1.png)

    *Why?*  The main app module contains a quickly identifiable manifest of the application's features.

    *Why?*  Each feature area contains a manifest of what it depends on, so it can be pulled in as a dependency in other applications and still work.

    *Why?*  Intra-App features such as shared data services become easy to locate and share from within `app.core` (choose your favorite name for this module).

    Note: This is a strategy for consistency. There are many good options here. Choose one that is consistent, follows Angular's dependency rules, and is easy to maintain and scale.

    > My structures vary slightly between projects but they all follow these guidelines for structure and modularity. The implementation may vary depending on the features and the team. In other words, don't get hung up on an exact like-for-like structure but do justify your structure using consistency, maintainability, and efficiency in mind.

    > In a small app, you can also consider putting all the shared dependencies in the app module where the feature modules have no direct dependencies. This makes it easier to maintain the smaller application, but makes it harder to reuse modules outside of this application.


<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
## Startup Logic

### Configuration
# COMPARE AND DISCUSS
###### [Style [Y170](http://github.com/bolpin/angular-style/README.md#style-y170)]

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

<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
# DISCUSS
###### [Style [Y171](http://github.com/bolpin/angular-style/README.md#style-y171)]
### Run Blocks


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


<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>

### $document and $window
# DISCUSS
###### [Style [Y180](http://github.com/bolpin/angular-style/README.md#style-y180)]
## Angular $ Wrapper Services

  - Use [$document](https://docs.angularjs.org/api/ng/service/$document) and [$window](https://docs.angularjs.org/api/ng/service/$window) instead of `document` and `window`.

  *Why?*  These services are wrapped by Angular and more easily testable than using document and window in tests. This helps you avoid having to mock document and window yourself.

<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
### $timeout and $interval
# DISCUSS
###### [Style [Y181](http://github.com/bolpin/angular-style/README.md#style-y181)]

  - Use [$timeout](https://docs.angularjs.org/api/ng/service/$timeout) and [$interval](https://docs.angularjs.org/api/ng/service/$interval) instead of `setTimeout` and `setInterval` .

  *Why?*  These services are wrapped by Angular and more easily testable and handle Angular's digest cycle thus keeping data binding in sync.


<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
## Testing
Unit testing helps maintain clean code, as such I included some of my recommendations for unit testing foundations with links for more information.

### Write Tests with Stories
# COMPARE AND DISCUSS
###### [Style [Y190](http://github.com/bolpin/angular-style/README.md#style-y190)]

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

<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
### Testing Library
# COMPARE AND DISCUSS
###### [Style [Y191](http://github.com/bolpin/angular-style/README.md#style-y191)]

  - Use [Jasmine](http://jasmine.github.io/) 

    *Why?*  Both Jasmine is widely used in the Angular community. It is stable, well maintained, and provides robust testing features.

<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
### Test Runner
# COMPARE AND DISCUSS
###### [Style [Y192](http://github.com/bolpin/angular-style/README.md#style-y192)]

  - Use [Teaspoon](https://github.com/modeset/teaspoon) as a test runner.

### Stubbing and Spying
# COMPARE AND DISCUSS
###### [Style [Y193](http://github.com/bolpin/angular-style/README.md#style-y193)]


### Headless Browser
# COMPARE AND DISCUSS
###### [Style [Y194](http://github.com/bolpin/angular-style/README.md#style-y194)]

  - Use [PhantomJS](http://phantomjs.org/) to run your tests on a server.

    *Why?*  PhantomJS is a headless browser that helps run your tests without needing a "visual" browser. So you do not have to install Chrome, Safari, IE, or other browsers on your server.

    Note: You should still test on all browsers in your environment, as appropriate for your target audience.

### Code Analysis
# COMPARE AND DISCUSS
###### [Style [Y195](http://github.com/bolpin/angular-style/README.md#style-y195)]

  - Run JSHint on your tests.

    *Why?*  Tests are code. JSHint can help identify code quality issues that may cause the test to work improperly.

### Alleviate Globals for JSHint Rules on Tests
# COMPARE AND DISCUSS
###### [Style [Y196](http://github.com/bolpin/angular-style/README.md#style-y196)]


### Organizing Tests
# COMPARE AND DISCUSS
###### [Style [Y197](http://github.com/bolpin/angular-style/README.md#style-y197)]

  - Place unit test files (specs) side-by-side with your client code.
  - Place specs that cover server integration or test multiple components in a separate `tests` folder.
```
/src/client/app/customers/customer-detail.controller.js.coffee
                          /customer-detail.controller.spec.js.coffee
                          /customers.controller.js.coffee
                          /customers.controller.spec.js.coffee
                          /customers.module.js.coffee
                          /customers.route.js.coffee
                          /customers.route.spec.js.coffee
```


## Constants

# COMPARE AND DISCUSS
###### [Style [Y240](http://github.com/bolpin/angular-style/README.md#style-y240)]
### Vendor Globals

```coffeescript
# constants.js.coffee

do ->
  'use strict'
  angular
    .module('app.core')
    .constant('toastr', toastr)
    .constant 'moment', moment
```

<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
# DISCUSS
###### [Style [Y241](http://github.com/bolpin/angular-style/README.md#style-y241)]

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
































## Routing
Client-side routing is important for creating a navigation flow between views and composing views that are made of many smaller templates and directives.

# COMPARE AND DISCUSS
###### [Style [Y270](http://github.com/bolpin/angular-style/README.md#style-y270)]

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

# COMPARE AND DISCUSS
###### [Style [Y021](http://github.com/bolpin/angular-style/README.md#style-y021)]

```coffeescript
app = angular.module('app', [
  'ngAnimate'
  'ngRoute'
  'app.shared'
  'app.dashboard'
])
```

vs.

```coffeescript
angular.module 'app', [
  'ngAnimate'
  'ngRoute'
  'app.shared'
  'app.dashboard'
]
```
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>


# COMPARE AND DISCUSS
###### [Style [Y271](http://github.com/bolpin/angular-style/README.md#style-y271)]

  - Define routes for views in the module where they exist. Each module should contain the routes for the views in the module.

    *Why?*  Each module should be able to stand on its own.

    *Why?*  When removing a module or adding a module, the app will only contain routes that point to existing views.

    *Why?*  This makes it easy to enable or disable portions of an application without concern over orphaned routes.


## Filters

<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
## Quiz

###### [Style [Y030](http://github.com/bolpin/angular-style/README.md#controllers)]
* Think of ________  as the business logic layer for the view.

* Avoid making $http requests in the ______.  (Consider doing this from _______.)


<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
# COMPARE AND DISCUSS
###### [Style [Y024](http://github.com/bolpin/angular-style/README.md#style-y024)]


```coffeescript
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
vs.

```coffeescript

angular
  .module('app')
  .controller 'Dashboard', -> 
  .factory 'logger', ->
```

