# Application Architecture - Version 0.1

The application is arranged in set of different services

There are following services which handle the overall application life-cycle:

* [UserSessionService](#UserSessionService)
* [ProductsService](#ProductService)
* [CartService](#CartService)
* [OrderService](#OrderService)

## <a name="#UserSessionService"></a>UserSessionService
---
`UserSessionService` manages the user session in the application. As soon as a user visits the website a unique session is generated and stored on __LocalStorage__ of browser. A `sessionId` is generated which uses __UUID v4__ and a `sessionToken` is also generated using __JWT (JSON Web Token)__. Since `sessionToken` has a timeout of 3600 seconds (1 hour), if the `sessionToken` is expired; the `sessionId` can be used to regenerate a new `sessionToken`. Each `sessionId` uniquely identifies an individual device/browser as long as user doesn't clear browser history(which is not the case with general audience). This vital information can be utilized for analytics data in future.

At the time of checkout when the user puts their phone number; if the phone number is assosiated with an existing user then an additional `userToken` is also recieved which can be used to fetch stored address of the available user. For security reasons each `sessionToken` can request only 3 unique user tokens to prevent API misuse. The number 3 is decided to cover any typo error(s) during user input. The APIs will also be CORS protected so that API misuse can be prevented.

It has following methods:

* `GetSessionId`
* `GetSessionToken`
* `SessionHasUser`
* `GetUserToken`
* `AddNewUser`
* `GetUserDetails`
* `UpdateUserDetails`
* `ListAddresses`
* `AddAddress`
* `UpdateAddress`

<br/>

## <a name="#ProductService"></a>ProductService
---

`ProductService` manages the products listing and other operations on the website. It makes use of `sessionToken` to make requests on server. This way the APIs can be secured. It will provide the following methods:

* `ListProducts`
* `ListByCategory`
* `SearchByTerm`
* `GetById`
* `GetBySlug`

<br/>

## <a name="#CartService"></a>CartService
---

`CartService` manages the shopping cart. It makes use of `sessionToken` to make requests on server. This way the APIs can be secured. It will provide the following methods:

* `ListCart`
* `AddToCart`
* `UpdateCart`
* `RemoveFromCart`
* `EmptyCart`
* `ApplyCoupon`
* `RemoveCoupon`

<br/>

## <a name="#OrderService"></a>OrderService
---
`OrderService` manages the order placement, tracking and listing feature of website. It also make use of `userToken` to make requests on server. This way the API can only be used by a valid existing user. If a user is not available a new user is created by `UserSessionService` before the API calls can be made by `OrderService`. It will provide the following methods:

* `ListOrder`
* `PlaceOrder`
* `TrackOrder`

<br/>

Application architected by:

[Tushar Srivastava](mailto:razieldecarte@gmail.com)
