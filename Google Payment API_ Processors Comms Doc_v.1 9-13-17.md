

![logo](https://github.com/payeezy/payeezy_pay_with_google/blob/master/images/googlelogo.png)


Google Payment API Processor comms document
===========================================

**UPDATED SEPTEMBER 13, 2017 (v.1)**

*This document is intended for our processor and gateway partners building support for the Google Payment API. Use this document to inform your merchant- and developer-facing communications and materials, in conjunction with the API’s technical documentation.*

*Please share all draft materials with your Google partner manager for approvals prior to publishing live or sending directly to any merchants.*

**Product overview**
----------------
Google users across the globe have hundreds of millions of credit and debit cards saved to their Google Accounts, to make purchases on Google properties like Play, YouTube, Chrome and more.

With the new Google Payment API, you can enable your merchants to reach these same customers by letting those users quickly and easily pay within apps and on websites, using any card they have stored in their Google Account, in addition to cards set up on their device in Android Pay.

The Google Payment API is available for apps and Chrome on Android, and also enables integrations with Google Assistant (using the [Transactions API](https://developers.google.com/actions/transactions/)).
 


**Product screenshots**
-------------------
![screencaptures](https://github.com/payeezy/payeezy_pay_with_google/blob/master/images/screencaptures.png)



*Additional mockups and examples available upon request.*

For button assets, please refer to Pay with Google Button Toolkit_2.0 PDF document. This will be publicly available on Google’s [developer site](https://developers.google.com/payments/guides/brand-guidelines) once the API is public.
 

**FAQs**
----

**GENERAL**
**Why is Google introducing the Google Payment API?**

 - More users: The Google Payment API enables third party merchants to
   offer a streamlined checkout experience to the hundreds of millions
   of Google users that have a card saved in their Google Account. These
   cards originate from any purchase the user has made in a Google
   product or service as well as from setting up the Android Pay app on
   their device.

 - Consistency: By leaning into the Google brand, we are creating a more consistent experience and letting users know they can pay with their Google Account across Google properties and third party partner apps and websites. 
 - Choice: Users can choose to pay with a card in their Google Account, cards set up in Android Pay, or add a new card natively without ever leaving the merchant app or site.

**Which platforms can the Google Payment API be enabled on?**
The Google Payment API will be initially available for Android native apps and for Chrome on Android. Support for additional platforms including Chrome for desktop is expected late 2017 or early 2018.

**Will the Google Payment API support iOS in the future?**
We don’t have anything to announce at this time.

**What markets is the Google Payment API available in?**
The API is available globally and merchants can use the Google Payment API to determine availability and device support. Users can pay with Google globally, wherever they can save and purchase with credit/debit cards on Google properties (for instance, on the Play Store).

Processors may specify additional market restrictions based on their processing capabilities by market.

**Are there any fees to use the Google Payment API?**
Google does not impose any fees to use the Google Payment API. Merchants continue to pay processing fees to their payment processor.

**How does this differ from the the Payment Request API?**
[PaymentRequest](https://developers.google.com/web/fundamentals/discovery-and-monetization/payment-request/) is a cross-browser web standard for payments, and merchants can integrate with the Google Payment API using this standard. PaymentRequest can be used by merchants to access payment credentials available via the Google Payment API, as well as other payment methods over time. Note that the merchant only needs to implement PaymentRequest for web, not native apps.

**How will Google Payment API transactions using cards be treated by the card networks? Any impacts to cost or liability for chargebacks?**
These are treated as regular e-commerce transactions, and there is no impact to costs or the existing dispute/chargeback process for merchants with these transactions.

**What implications to liability shift happen with Google Payment API transactions?** 
Implementing the Google Payment API in and of itself does not impact liability for disputed transactions. This is determined by card brand rules and the underlying payment credentials.


**ANDROID PAY**
**What is Android Pay and how does it differ from Google Payment API?**
Android Pay is a digital wallet platform developed by Google to power in-app and tap-to-pay purchases on mobile devices, enabling users to make payments with Android phones, tablets or watches using payment credentials that are provisioned by the user.

The Google Payment API enables merchants to access all available payment credentials for a Google user, including cards saved to the user’s Google Account as well as credentials in Android Pay on the user’s devices, for online transactions.

**Is Android Pay going away?**
No. Users can still set up their cards in the Android Pay app to pay in stores, and these tokenized credentials will also continue to be available for online purchases.

**How long will Android Pay integrations work for merchants?**
We will continue to support existing Android Pay integrations and work with merchants to migrate them to the Google Payment API, so they can enable payments for the additional hundreds of millions of users that have saved cards to their Google Account.

**What if a merchant would like to integrate with the Android Pay API?**
The existing Android Pay online Terms of Service have been disabled and we are no longer accepting any merchants to launch with the Android Pay API. Exceptions can be made for individual merchants on a case-by-case basis. Contact your Google partner manager if you have a merchant that wishes to pursue this route.

Note that merchants will receive access to Android Pay tokens via the Google Payment API, as well as cards saved to users’ Google Accounts.


**INTEGRATION**
Refer to technical documentation for more detail.
 
**Will merchants be able to block certain types of cards, card brands or credentials ?**
Yes. Developers can choose the card brands supported, type of credentials (cards and/or network tokens), and disable prepaid cards if they don’t support these today for regular card transactions.

**Is the card type (Visa/MC) available outside of the encrypted data payload?** 
Yes, the card description including the card brand + last4, as well as card class (debit/prepaid/credit) are returned outside the encrypted payload.

**What Terms of Service do my merchants need to accept to use the Google Payment API? How should they accept these?**
Merchants can sign up and accept terms for the Google Payment API via our developer site, once it’s public. For mobile web integrations enabled via our processor/gateway partners, please consult your Google partner manager for details on how to implement.
