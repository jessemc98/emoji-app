not tested in contract tests as this should be covered by extensive functional and security testing
  experiment with a way to still ensure consumers call service correctly (think cas "next" dance) (would be nice to test where auth is done through cas next dance and where its done on api)
    could possibly write contracts in consumer which arent ran against provider
    maybe this just shouldnt be contract tested and instead unit? 
     then again, prefferably contracts are written like unit tests 
       need to get around issue of provider needing to write tests for functonality they dont care about because of bubbling up contracts which depend on specific functionality or specific data (e.g a pass through service testing business logic)

authenticates the client via cookie or w/e
  authenticates against external login service and appends user info e.g. username to cookies
  allows internal services to all go through one central service
  
allows BE services to authenticate client