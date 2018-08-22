---
title: Getting Started
sidebarDepth: 3
---

# Getting Started

In order to use the API platform, you will need to register for an account [here]( https://pg-all.portal.azure-api.net/). You can register with an individual or company account.

An account activation link will be sent in an email to the email address you used in registering for a developer account; the activation link expires within 24 hours of it being sent, and you will need to register for another account. The account activation link enables you to activate your developer account and set a password for your account.



After you have activated your account, and logged in, click on your “Username” at the top of the login page; select “PROFILE” from the drop down list. 

You should now have access to a portal where you can view your profile information, your subscriptions and your applications. Let us explore this portal for a bit.

## Profile

In this section, you will see your personal information, account type and developer level. You can also change your `Account Password` and `Account Information` here.

### Subscriptions

A developer is issued with a `Primary Key` and `Secondary Key` for every product. Both Primary and secondary Subscription key provides access to the API. Without one of them a developer cannot access any of the APIs. Subscriptions are stored under the user profile and have no expiry.

This section provides information about your current subscription package. Here you can view the status of the package, date it started, conduct cancellation or activation actions, and also show or regenerate your `Primary Key` and `Secondary Key`

![Your subscriptions](/your-subscriptions.png) 

::: warning
Please keep these keys a secret. :wink:
:::


Regeneration of a subscription key invalidates the old Key. Therefore, a developer needs to update the application with new keys after successful regeneration.

You will also notice that in the top right corner to the subscriptions is the `Analytics reports` button. Click this button to view performance over a period of time - day, week(s), month(s). We provide analytics on Calls, Response time and Bandwidth.

### Applications

In this section is where all great ideas begin. Here is where you register all your apps. Click the `Register Application` button to create your first app. When you are done creating your first app, it might look something like this.

![Your Applications](/your-applications.png) 


## Product Subscription

Product defines how a developer can access the APIs. A developer must subscribe to at least one product to access the APIs. Upon subscription a developer shall be issued with a subscription key. This key shall be use to authenticate and track the developer activities on the API Manager. Currently there are two products published on the portal.

::: tip
At this point you should be able to see the [API products](https://pg-all.portal.azure-api.net/docs/services) available. :tada: :100:
:::

The API offers two options:

- Starter: Exposes all available APIs but limits the number of API calls to 5/min and a total of 100 per week. Subscription to this product doesn’t need approval.
- Unlimited: Exposes all available APIs with unlimited API calls towards API Manager. Subscription to this product requires Administrator Approval approval.

Let us explore the differences between the packages.

### Starter Package

This product package contains 4 APIs:

- [Sandbox MTN MoMo APIs](https://pg-all.portal.azure-api.net/docs/services/5ae176865809f91734da012e)
- [Sandbox Oauth API](https://pg-all.portal.azure-api.net/docs/services/sandbox-oauth-api)
- [Test Oauth API](https://pg-all.portal.azure-api.net/docs/services/test-oauth)
- [Test Partner Gateway API](https://pg-all.portal.azure-api.net/docs/services/test-partner-gateway-api)

To select this package, click the `subscribe` button to confirm. 

### Unlimited Package

This product package contains 4 APIs:

- Test Oauth API
- Test Partner Gateway API

Click the `subscribe` button to confirm. You should then receive an alert notifying you of the following:

`You have successfully submitted a subscription request to the Unlimited product. You will be notified when the request is approved`



## Register an app

Now that you are subscribed to a product and can use the API, it is time to create an app. You will need to fill in the following information:

- Title
- Description
- Requirements
- Category
- URL

Upon submission, you should see the application listed under `Your Applications`. The app name is shown along with the category and state. It shows the state as `Not submitted`. We need to change this. Click the submit button to proceed.

This takes you to the `Application publishing` page where a summary of your application is shown, along with any warnings that you may need to resolve. It also shows you the reason why all application submissions are vetted.

::: tip Application Submission Vetting
Before publishing on this portal, your application record has to undergo the process of approval by portal administration. Once your application reviewed, you will receive an email on the mailbox that you specified during registration.
:::

If all the information provided checks out, your application should now be ready to use the API platform.


## MTN MoMoPay Widget

There is also a MoMo Widget that is not listed along with the packages. This widget can be used to integrate a MoMoPay checkout button to accept MoMo payments on your e-commerce site. You can see more about it [here](https://pg-all.portal.azure-api.net/widget-api)




