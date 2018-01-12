# Sample Android App using HyperTrack SDK
A simple Android application demonstrating the use of HyperTrack SDK.

HyperTrack Setup link: https://dashboard.hypertrack.com/setup

<p>
<img src="assets/login_activity.png" alt="Login Activity" width="250" height="480">


<img src="assets/login_activity_location_permission.png" alt="Location Permission" width="250" height="480">

<img src="assets/main_activity.png" alt="LogOut" width="250" height="480">
</p>

## Requirements
[Android Studio](https://developer.android.com/studio/index.html) with emulator (or test device)

## Basic Setup

#### Step 1. Clone this repository

Clone this repository
```bash
# Clone this repository
$ git clone https://github.com/hypertrack/quickstart-android.git
```

#### Step 2. Signup and get Test Publishable key.
1. Signup [here](https://dashboard.hypertrack.com/signup).
2. Get `test` publishable key from [dashboard](https://dashboard.hypertrack.com/settings) settings page.
3. Add the test publishable key to [MyApplication](https://github.com/hypertrack/quickstart-android/blob/mock-tracking/app/src/main/java/com/hypertrack/quickstart/MyApplication.java) file.
```java
HyperTrack.initialize(this.getApplicationContext(), TEST_PUBLISHABLE_KEY);
```

#### Step 3. FCM Configuration
The SDK has a bi-directional communication model with the server. This enables the SDK to run on a variable frequency model, which balances the fine trade-off between low latency tracking and battery efficiency, and improve robustness. For this purpose, the Android SDK uses FCM or GCM silent notifications.

Refer to the [FCM Integration Guide](https://docs.hypertrack.com/sdks/android/gcm-integration.html#locate-your-gcmfcm-key).

## Usage

#### Step 1. Location Permission and Location Setting.
Ask for `location permission` and `location services`. Refer [here](https://docs.hypertrack.com/sdks/android/reference/hypertrack.html#boolean-checklocationpermission) for more detail
```java
// Check for Location permission
if (!HyperTrack.checkLocationPermission(this)) {
    HyperTrack.requestPermissions(this);
    return;
}

// Check for Location settings
if (!HyperTrack.checkLocationServices(this)) {
    HyperTrack.requestLocationServices(this);
}
```

#### Step 2. Create HyperTrack User & Start Tracking
The next thing that you need to do is to create a HyperTrack user. More details about the function [here](https://docs.hypertrack.com/sdks/android/reference/user.html#getorcreate-user).

When the user is created, we need to start tracking his location and activity. Call the following method to do so ```HyperTrack.startMockTracking()```. Refer [here](https://docs.hypertrack.com/sdks/android/reference/hypertrack.html#void-startmocktracking) for more detail. 

```java
// Get User details, if specified
final String name = nameText.getText().toString();
final String phoneNumber = phoneNumberText.getText().toString();
final String lookupId = !HTTextUtils.isEmpty(lookupIdText.getText().toString()) ?
        lookupIdText.getText().toString() : phoneNumber;

UserParams userParams = new UserParams().setName(name).setPhone(phoneNumber).setLookupId(lookupId);
/**
 * Get or Create a User for given lookupId on HyperTrack Server here to
 * login your user & configure HyperTrack SDK with this generated
 * HyperTrack UserId.
 * OR
 * Implement your API call for User Login and get back a HyperTrack
 * UserId from your API Server to be configured in the HyperTrack SDK.
 */
HyperTrack.getOrCreateUser(userParams, new HyperTrackCallback() {
    @Override
    public void onSuccess(@NonNull SuccessResponse successResponse) {
       
        User user = (User) successResponse.getResponseObject();
        String userId = user.getId();
        // Handle createUser success here, if required
        // HyperTrack SDK auto-configures UserId on createUser API call,
        // so no need to call HyperTrack.setUserId() API

        // On UserLogin success
        HyperTrack.startMockTracking();
    }

    @Override
    public void onError(@NonNull ErrorResponse errorResponse) {

        Toast.makeText(LoginActivity.this, R.string.login_error_msg + " " + errorResponse
                .getErrorMessage(), Toast.LENGTH_SHORT).show();
    }
});
```

#### Step 4. Stop Tracking
When user logs out call `HyperTracking.stopMockTracking()` to stop tracking the user's location and activity. Refer [here](https://docs.hypertrack.com/sdks/android/reference/hypertrack.html#void-stopmocktracking) for more detail.

```java
HyperTrack.stopMockTracking();
```

## Documentation
For detailed documentation of the APIs, customizations and what all you can build using HyperTrack, please visit the official [docs](https://docs.hypertrack.com/).

## Contribute
Feel free to clone, use, and contribute back via [pull requests](https://help.github.com/articles/about-pull-requests/). We'd love to see your pull requests - send them in! Please use the [issues tracker](https://github.com/hypertrack/quickstart-android/issues) to raise bug reports and feature requests.

We are excited to see what live location feature you build in your app using this project. Do ping us at help@hypertrack.io once you build one, and we would love to feature your app on our blog!

## Support
Join our [Slack community](http://slack.hypertrack.com) for instant responses, or interact with our growing [community](https://community.hypertrack.com). You can also email us at help@hypertrack.com.
