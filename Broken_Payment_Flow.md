Title: Broken Payment Flow Due to Missing Server-Side Payment Verification


Severity: High

An attacker can easily leverage the critical vulnerability of missing server side payment verification, by easily controlling or modifying the user controlled payment critical parameters. A lack of payment gateway, pushes the bussiness logic into trusting and using these user controlled parameters to give a confirmation response.
These parameters can be easily modified by the attacker thus resulting in financial theft, false wallet balance, inventory loss and attack at mass on any business.


Description:

Any business relies on a payemnt gateways for verification of payments, as well as verification of debit/credit cards. Thus these gateways are the integral part of the payment flow, based on which the business logic provides a confirmation for any payment.
Missing of this payment gateway, leads to no proper payment verification for the bussiness logic, hence the logic is forced to depend upon user controlled payment parameters to provide any payment confirmation.
But, since these parameters are user controlled, they are highly vulnerable to modifications by attacker. The main issue with the Juiceshop's business logic was exactly this. It depended on the user provided parameter of PaymentId to provide payment confirmation. 
Another major vulnerability was that this user controlled parameter, when modified to a different value will also provide payment confirmation. Hence, the server totally relying on this parameter provides a payment confirmation to any confirmation request sent with any different acceptable value for this PaymentId parameter.



Steps to Reproduce:
1) Open Burpsuite and start a new project. In this new project go to 'Proxy' then down to the 'Intercept' tab, toggle the 'Intercept off' to on, and click on 'Open Browser'.
2) Navigate to the Juiceshop using http://localhost:3000 (you need to launch Juiceshop first either using Docker or Node.js).
3) Create a new User Id and Log in.
4) Add some products into the cart and head towards the checkout point. Add all the necessary information for the address and speed of delivery, and choose the payment mode.
5) Ensure you observe the burpsuite dashboard after every step on the website to forward the traffic, or else you'll be stuck on the website at one page only.
6) Once you click on the confirm and pay button, you'll notice a traffic on the Burpsuite dashboard namely with a POST method and URL as '/rest/basket/6/checkout'
7) In the Request sub window at the bottom, you'll be able to inspect the packet in depth, including its body.
8) Here you'll notice the 'paymentId' parameter which has an integer value, this is the user controlled parameter that the business logic relies on to provide a payment confirmation.
9) In the request sub-window, click on the three horizontal lines on the top right corner, and click send to repeater.
10) Now the Repeater window opens, with the request sub-window, here we'll make modifications in the request parameter.
11) Change the integer value of the paymentId parameter to another integer (eg. 7 changes to 9).
12) Click send on the top left corner to send the request and observe the servers response.
13) The server responds with an order confirmation message, even with the modified fake paymentId parameter.
    



