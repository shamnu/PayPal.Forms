# PayPal.Forms
PayPal Plugin for Xamarin.Forms

# Setup

##iOS & Android

In MainActivity(Android)/AppDelegate(iOS) after "Forms.Init()"  call the Init method with your PayPal config value
```
...
global::Xamarin.Forms.Forms.Init ();
 CrossPaypalManager.Init(new PayPalConfiguration(
					PayPalEnvironment.NoNetwork,
					"Your PayPal ID from https://developer.paypal.com/developer/applications/"
					)
					{
					  //If you want to accept credit cards
					  AcceptCreditCards = true,
				  	//Your business name
					  MerchantName = "Test Store",
					  //Your privacy policy Url
					  MerchantPrivacyPolicyUri = "https://www.example.com/privacy",
					  //Your user agreement Url
					  MerchantUserAgreementUri = "https://www.example.com/legal"
				  }
			);
...

```

##iOS

From official PayPal SDK page (https://github.com/paypal/PayPal-iOS-SDK#with-or-without-cocoapods) follow the two steps:

* Add the open source license acknowledgments
* Add squemas into Info.plist

##Android

Is very important to add the following code inside your OnActivityResult and OnDestroy on your main activity class
```
protected override void OnActivityResult (int requestCode, Result resultCode, Intent data)
{
	base.OnActivityResult (requestCode, resultCode, data);
	PayPalManagerImplementation.Manager.OnActivityResult(requestCode, resultCode, data);
}

protected override void OnDestroy()
{
	base.OnDestroy();
	PayPalManagerImplementation.Manager.Destroy();
}
```

# Usage

##Single Item

```
var result = await CrossPaypalManager.Current.Buy (new PayPalItem ("Test Product", new Decimal (12.50), "USD"), new Decimal (0));
if (result.Status == PayPalStatus.Cancelled) {
	Debug.WriteLine ("Cancelled");
}else if(result.Status == PayPalStatus.Error){
	Debug.WriteLine (result.ErrorMessage);
}else if(result.Status == PayPalStatus.Successful){
	Debug.WriteLine (result.ServerResponse.Response.Id);
}
```

##List of Items

```
var result = await CrossPaypalManager.Current.Buy (new PayPalItem[] {
				new PayPalItem ("sample item #1", 2, new Decimal (87.50), "USD",
					"sku-12345678"), 
				new PayPalItem ("free sample item #2", 1, new Decimal (0.00),
					"USD", "sku-zero-price"),
				new PayPalItem ("sample item #3 with a longer name", 6, new Decimal (37.99),
					"USD", "sku-33333") 
			}, new Decimal (20.5), new Decimal (13.20));
if (result.Status == PayPalStatus.Cancelled) {
	Debug.WriteLine ("Cancelled");
}else if(result.Status == PayPalStatus.Error){
	Debug.WriteLine (result.ErrorMessage);
}else if(result.Status == PayPalStatus.Successful){
	Debug.WriteLine (result.ServerResponse.Response.Id);
}
```

##Future Payments

```
var result = await CrossPaypalManager.Current.RequestFuturePayments();
if (result.Status == PayPalStatus.Cancelled) {
	Debug.WriteLine ("Cancelled");
}else if(result.Status == PayPalStatus.Error){
	Debug.WriteLine (result.ErrorMessage);
}else if(result.Status == PayPalStatus.Successful){
	//Print Authorization Code
	Debug.WriteLine(result.ServerResponse.Response.Code);
}
```

##Profile sharing

```
var result = await CrossPayPalManager.Current.AuthorizeProfileSharing();
if (result.Status == PayPalStatus.Cancelled) {
	Debug.WriteLine ("Cancelled");
}else if(result.Status == PayPalStatus.Error){
	Debug.WriteLine (result.ErrorMessage);
}else if(result.Status == PayPalStatus.Successful){
	Debug.WriteLine (result.ServerResponse.Response.Code);
}
```

##Obtain a Client Metadata ID

```
//Print Client Metadata Id
Debug.WriteLine(CrossPaypalManager.Current.ClientMetadataId);
```
# Nuget
* Nuget Package (https://www.nuget.org/packages/PayPal.Forms)

# Known Issues
* On Xamarin Studio 5.10.X you will receive an alert that xamarin cant resolve "xamarin.paypal" and/or "paypal.forms" lib name this will be fixed by upgrading to Xamarin Studio 6 in alpha channel(recommended) or downgrading to Xamarin Studio 5.9(not recommended)

# TODO
* Add app provided shipping address.
* Enable shipping address retrieval.
