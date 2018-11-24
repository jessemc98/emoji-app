// over engineered emoji stock market/social network for prototyping contract lib

how to test things like 
  adding emoji to cart
  adding to cart then buying emoji
    its bought at the correct price
    receipt is shown with correct emoji 
      imagine a bug shows up where in a very specific case you show the wrong emoji so you want to
        write a test for it
          to stop the bug coming back
          to help with debugging by writing tests in services starting with ui until you find failure 
            should you start with ui or should you start where you think error most likely lies or in the middle and use divide and conquer if you dont know?
            should definitely write one in the ui if that is where the error manifested right?
        find where the wrong emoji is coming from
          prefferably by ruling out assumptions at each stage of the pipeline
          where to start?
          does the api cache the previous request and return an emoji?
          does the ui do some magic?
          between which ui services does the issue lie?
          are all your systems fine and external system is acting up?
  adding emoji to cart, price updates, user is shown price update
  adding to cart then buying but emoji is sold out before purchase goes through (would be super important for businesses like airplane ticket vendors)
  adding to cart then buying but price updates before purchase goes through


how to test pass through service  pass through service (i.e. test stateless vs stateful services) (i.e. previous messages mater vs dont matter)
how to test stateful services?
  should you build up state using previous scenarios?
  what would be different in the case where the providers/consumers of this service are themselves stateful/stateless and any combination of them?
  what would be different in the case where the providers/consumers of this service are external/internal (i.e. covered or not by contracts) and any combination of them?
  do we test some contracts with logic and others with structure/patterns, separating the concepts?
  need to allow the pass through services to not need a whole bunch of tests just because a consumer needs to test a gazillion stateful scenarios

ab test/backwards compatible api/versioned api

allow marking deprecation in contracts so consumers tests show warning about feature being deprecated in provider

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
might be good architecture or framework to be built upon architecture what we have:
all ui microservices have any number of these services but may not skip any layers e.g. layers=[1,2,3] service = [1,2,3] | [1,2] | [2,3] | [1] | [2] | [3] service != [1,3]
(1) ui layer
      ui layer, cant be depended on by other services
(2) logic layer
      exposes a mb api which other ui and logic services can communicate through
      communicates with api adapter to BE services

(3) api adapter
      exposes a js api
      communicates with BE services
      published on npm
      may include both push and/or pull protocols 

maybe can allow skipping second layer i.e. service = [1,3] and/or [1/2,3] (ui/logic layer, api adapter)
  this could complicate testing
    an example of this is the login, you can login through the ui but not though the mb (though it doesnt let you login through mb it does publish login state [1/2,3])
    while we can imitate the login state through the mb, this then complicates using contracts to do end-2-end live tests later
      though a nice way to support this may be by defining environment specific things in contracts 
        e.g. in dev send mb to imitate login, in pp/live where dependencies are deployed and you dont need to mock their dependants login through real ui
  this allows flexibility as you may want a service which 
    has all ui with logic which comms directly to api and doesnt have any mb comms
    or one which has mb comms but doesnt separate logic layer from ui
    or one which has mb comms but doesnt support everything the ui does through those comms
  this however is a double edged sword
    this flexibility might allow for bad design and is exactly what good architecture/a framework tries to protect by restricting you enough to allow for good patterns
      the question is, is it the job of the test framework to support everything and not enforce any specific "frontend microservice" framework/architecture concept? or should it be flexible? where is the line of how much it enforces?
        on one hand the way you test should encourage good architecture and arguably is going to be coupled to the concept of microservices you go with
          that said, we should probably let the users the flexibility to decide what THEY deem to be a frontend microservice as the concept is very immature and so there are no best practices or proven architectures
          need to find what is the right balance

we can then define different "primitives" and begin to build a framework, best practices and a test strategy e.g. microservice, mb microservice, ui microservice, api adapter/client, api, "user/test service", npm module/js module, etc...
