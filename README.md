MaaSAlerts iOS SDK
==================

Version 1.0.0

This is the iOS SDK for the MaaS Phunwares Alerts & Notifications module. Visit http://maas.phunware.com/ for more details and to sign up.



Requirements
------------

- MaaSCore v1.0.0 or greater.
- iOS 5.0 or greater
- Xcode 4.4 or greater



Installation
------------

MaaSAlerts has a dependency on MaaSCore.framework which is available here: https://github.com/phunware/maas-core-ios-sdk

It's recommended that you add the MaaS framesworks to the 'Vendor/Phunware' directory. Then add the MaaSCore.framework and MaaSAlerts.framework to your Xcode project.

The following frameworks are required:
````
MaaSCore.framework
````

Scroll down for implementation details.



Documentation
------------

MaaSAlerts documentation is included in the Documents folder in the repository as both HTML and .docset. You can also find the latest documentation here: http://phunware.github.io/maas-alerts-ios-sdk/.

Configuring up your app for Apple Push Notifications can be a challenging task. Here are some resources to kickstart the process:
- [Apple Documentation](https://developer.apple.com/library/ios/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Introduction.html)
- [Apple Push Notification Services Tutorial (1/2)](http://www.raywenderlich.com/32960/apple-push-notification-services-in-ios-6-tutorial-part-1)
- [Apple Push Notification Services Tutorial (2/2)](http://www.raywenderlich.com/32963/apple-push-notification-services-in-ios-6-tutorial-part-2)


Application Setup
-----------------
At the top of your application delegate (.m) file add the following:

````objective-c
#import <MaaSCore/MaaSCore.h>
#import <MaaSAlerts/MaaSAlerts.h>
````

Inside your application delegate you will need to initialize MaaSCore in the application:didFinishLaunchingWithOptions: method. For more detailed MaaSCore installation instructions please see https://github.com/phunware/maas-core-ios-sdk#installation.

````objective-c
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
    // These values can be found for your application in MaaS Portal
    [MaaSCore setApplicationID:@"APPLICATION_ID"
    			   setAccessKey:@"ACCESS_KEY"
                  signatureKey:@"SIGNATURE_KEY"
                 encryptionKey:@"ENCRYPT_KEY"]; // Currently unused. You can place anything NSString value here
    ...
}
````

Apple has three primary methods for handling remote notifications. You will need to implement these in your application delegate, forwarding the results to MaaSAlerts:

````objective-c
- (void)application:(UIApplication *)app didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)devToken
{
    [MaaSAlerts didRegisterForRemoteNotificationsWithDeviceToken:devToken];
    ...
}

- (void)application:(UIApplication *)app didFailToRegisterForRemoteNotificationsWithError:(NSError *)err
{
    [MaaSAlerts didFailToRegisterForRemoteNotificationsWithError:err];
    ...
}

- (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo
{
    [MaaSAlerts didReceiveRemoteNotification:userInfo];
    ...
}
````

For a complete example, see https://github.com/phunware/maas-alerts-ios-sdk/Sample


Subscription Groups
-------------------
MaaS Portal provides the ability to setup subscribtion groups for which more filtered alerts and notifications can be received. There are two MaaSAlerts SDK methods the facilitate this, *getSubscriptionGroupsWithSuccess:failure:* and *subscribeToGroupsWithIDs:success:failure:*.

````objective-c
// Fetch an array of of the available subscriptions
[MaaSAlerts getSubscriptionGroupsWithSuccess:^(NSArray *groups) {
        // Display the available subscription groups to the user
        // The groups array will contain dictionary objects the conform to the following structure: {@"id" : @"SUBCRIPTION_GROUP_ID", @"name" : @"SUBSCRIPTION_GROUP_NAME"}
    } failure:^(NSError *error) {
		// Handle error
    }];
    
    
// To update the subscription groups you would make the following call
NSArray *subscribedGroups = @[@"SUBSCRIPTION_GROUP_ID", ...];
[MaaSAlerts subscribeToGroupsWithIDs:subscribedGroups success:^{
        // Handle success
edGroups    } failure:^(NSError *error) {
        // Handle error
    }];
}
````

Optional
--------

Fetching extra information associated with a push notification using MaaSAlerts is easy using *getExtraInformationForPushID:success:failure:* 

````objective-c
[MaaSAlerts getExtraInformationForPushID:@"PUSH_ID" success:^(NSDictionary *extraInformation) {
        // Process the extra information
    } failure:^(NSError *error) {
        // Handle error
    }];
````
