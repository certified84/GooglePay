# GooglePay Library
![Jit](https://img.shields.io/jitpack/v/github/certified84/GooglePay?style=for-the-badge&color=2F9319) 

 A wrapper over the [Google Pay API](https://developers.google.com/pay/api/android/overview) 

<!--
## Demo
<img src="https://github.com/certified84/CustomProgressIndicator/blob/master/demo/custom_progress_indicator1.gif" alt="demo"/>   <img src="https://github.com/certified84/CustomProgressIndicator/blob/master/demo/custom_progress_indicator.gif" alt="demo"/>
-->

## Setup

Add it in your root `build.gradle.kts` at the end of repositories:

```groovy
      allprojects {
          repositories {
              //...omitted for brevity
              maven(url = "https://jitpack.io")
          }
      }
```



Add the dependency

```kotlin
      dependencies {
          implementation("com.github.certified84:GooglePay:$latest_release")
          implementation ("com.google.android.gms:play-services-wallet:19.2.1")
      }
```

## :bulb: Tech Used

<img src="https://marvel-b1-cdn.bc0a.com/f00000000156946/www.jrebel.com/sites/rebel/files/image/2021-01/what%20is%20kotlin%20banner%20image.png" height="70px" width="100px"> 
    
## Usage
Sample implementation [here](app/)

### BuyWithGooglePay
- Add `BuyWithGooglePay` to your xml layout.

```xml
        <com.certified.google_pay.BuyWithGooglePay
            android:id="@+id/btn_google_pay"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginStart="16dp"
            android:layout_marginEnd="16dp"
            app:layout_constraintBottom_toBottomOf="parent"
            app:layout_constraintEnd_toEndOf="parent"
            app:layout_constraintStart_toStartOf="parent"
            app:layout_constraintTop_toTopOf="parent" />
```
- Align the button properly to suit your needs

### Initialize the indicator

```kotlin
      var buyWithGoogleButton: BuyWithGooglePay = findViewById(R.id.btn_google_pay)
      // ViewBinding
      var buyWithGoogleButton = binding.btnGooglePay
```

### Call the initialize() method

```kotlin
      /*
        * This step is required
        * Pass the current to the initialize method
        * You'll need to override onActivityResult too
      /*

      buyWithGoogleButton.initialize(requireActivity())
```

### Set the price and shipping cost

```kotlin
     buyWithGoogleButton.setPrice(200)
     buyWithGoogleButton.setShippingCost(50)
     // OR
     buyWithGoogleButton.price = 200
     buyWithGoogleButton.shippingCost = 50
```

### Set the indicator text color

```kotlin

      buyWithGoogleButton.setTextColor(ResourcesCompat.getColorStateList(resources, R.color.white, null)!!)

```

### Set the text typeface

```kotlin

       buyWithGoogleButton.setTypeface(R.font.space_grotesk_regular)

```

### Override the onActivityResult 

```kotlin
    override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {
        super.onActivityResult(requestCode, resultCode, data)
        when (requestCode) {
            // Value passed in AutoResolveHelper
            GOOGLE_PAY_REQUEST_CODE -> {
                when (resultCode) {
                    RESULT_OK ->
                        data?.let { intent ->
                            PaymentData.getFromIntent(intent)?.let(::handlePaymentSuccess)
                        }

                    RESULT_CANCELED -> {
                        // The user cancelled the payment attempt
                    }

                    AutoResolveHelper.RESULT_ERROR -> {
                        AutoResolveHelper.getStatusFromIntent(data)?.let {
                            handleError(it.statusCode)
                        }
                    }
                }
            }
        }
    }
```

### Add methods to handle payment success and error

```kotlin
   indicator.stopAnimation()
   
   // Always stop the animation in onPause()
   fun handlePaymentSuccess(paymentData: PaymentData) {
       //....perform an action
   }

   fun handleError(statusCode: Int) {
        //...perform an action e.g
        Log.w("loadPaymentData failed", "Error code: $statusCode")
    }
```

### Misc
- You can also set the
  - Text
  - Text color
  - Text size
- But remember to always follow the [google pay brand guideline](https://developers.google.com/pay/api/android/guides/brand-guidelines)
 
## Contribute 🤝
- Please create an issue if you find something wrong
- Feel free to contibute. See [Contributing Guidelines](CONTRIBUTION.md).


### Licensed under the [Apache-2.0 License](LICENCE)

```

Copyright 2023 Samson Achiaga

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

```
