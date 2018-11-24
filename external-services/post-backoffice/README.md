should only be accessed from BE services? 
  might be interesting for the sake of forcing certain testing scenarios 
    e.g. needing to set up state for a scenario where you cant set up state simply by building up from previous scenarios/actions in that service or its dependencies (i.e. service A depends on B, B depends on C, C on D but to cause push update to A from B you need to do action on D)
         needing to do some actions which cause a push update in a test scenario where you can only cause the push update through an action on a completely separate service to the one being tested

create posts
update posts
remove posts