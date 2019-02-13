

## <div dir="rtl">آموزش راه اندازی کتاب‌خانه TapsellPlus در Unity</div>

#### <div dir="rtl">برای استفاده از این کتابخانه باید از build system گردل استفاده کنید. همچنین سعی کنید از آخرین نسخه unity استفاده کنید.</div>

### <div dir="rtl">اضافه کردن کتابخانه به پروژه</div>

<div dir="rtl">ابتدا <a href="https://storage.backtory.com/tapsell-server/sdk/tapsellplus/unity/tapsellPlus-v0.0.2.1.unitypackage">unity package</a> تپسل را دانلود کنید و به پروژه اضافه کنید.</div>


<div dir="rtl">از player settings قسمت publishing settings تیک custom gradle template رو بزارید.</div>
<div dir="rtl">خطوط زیر را در بخش dependencies فایل mainTemplate.gradle در مسیر Assets/Plugins/Android اضافه کنید.</div>

```gradle
dependencies {
    implementation fileTree(dir: 'libs', include: ['*.jar'])

    implementation 'com.google.code.gson:gson:2.8.5'
    implementation 'com.squareup.retrofit2:retrofit:2.5.0'
    implementation 'com.squareup.retrofit2:converter-gson:2.5.0'
    implementation 'com.squareup.okhttp3:logging-interceptor:3.12.1'
    implementation 'ir.tapsell.sdk:tapsell-sdk-android:4.1.4'

    implementation 'com.google.android.gms:play-services-ads:15.0.1'
    implementation 'com.google.android.gms:play-services-basement:15.0.1'
    implementation 'com.google.android.gms:play-services-ads-identifier:15.0.1'
    implementation 'com.google.android.gms:play-services-location:15.0.1'
    implementation 'com.unity3d.ads:unity-ads:3.0.0'
}
```

<div dir="rtl">تابع زیر را در یکی از اسکریپت‌های ابتدایی برنامه بزارید.</div>

```cs
void Start () {
  TapsellPlus.initialize (TAPSELL_KEY);
}
```

## <div dir="rtl">آموزش تبلیغات ویدیو جایزه‌ای</div>

<div dir="rtl">ابتدا از پنل یک تبلیغگاه (zone) ویدیو جایزه‌ای بسازید و zoneId رو زمان درخواست و نمایش تبلیغ استفاده کنید</div>

<div dir="rtl">مطابق کد زیر درخواست تبلیغ دهید</div>

```cs
public void Request () {
  TapsellPlus.requestRewardedVideo (ZONE_ID,
    (string zoneId) => {
      Debug.Log ("on response " + zoneId);
    },
    (TapsellError error) => {
      Debug.Log ("Error " + error.error);
    }
  );
}
```

<div dir="rtl">بعد از اجرای متد response تبلیغ آماده نمایش است و میتوانید مطابق روش زیر نمایش دهید</div>

```cs
public void Show () {
  TapsellPlus.showAd (ZONE_ID,
    (string zoneId) => {
      Debug.Log ("onOpenAd " + zoneId);
    },
    (string zoneId) => {
      Debug.Log ("onCloseAd " + zoneId);
    },
    (string zoneId) => {
      Debug.Log ("onReward " + zoneId);
    }
  );
}
```

## <div dir="rtl">آموزش تبلیغات بینابینی</div>

<div dir="rtl">مطابق تبلیغات جایزه‌ای پیش برید فقط زمان درخواست تبلیغ از متد TapsellPlus.requestInterstitial استفاده کنید</div>


## <div dir="rtl">آموزش تبلیغات بنر استاندارد</div>

<div dir="rtl">مطابق کد زیر میتونید بنر استاندارد نمایش دهید</div>

```cs
public void Show () {
  TapsellPlus.showBannerAd (ZONE_ID, BANNER_TYPE, VERTICAL_GRAVITY, HORIZONTAL_GRAVITY);
}
```

<div dir="rtl">BANNER_TYPE سایز نمایش بنر هست و میتواند مقادیر زیر باشد</div>

|     keyword    |   size  |
|:--------------:|:-------:|
|  BANNER_320x50 |  320x50 |
| BANNER_320x100 | 320x100 |
| BANNER_250x250 | 250x250 |
| BANNER_300x250 | 300x250 |
|  BANNER_468x60 |  468x60 |
|  BANNER_728x90 |  728x90 |


<div dir="rtl">VERTICAL_GRAVITY و HORIZONTAL_GRAVITY موقعیت قرار گیری بنر در صفحه هست و میتواند مقادیر زیر باشد</div>

```
Gravity.TOP - Gravity.BOTTOM - Gravity.LEFT - Gravity.RIGHT - Gravity.CENTER
```

<div dir="rtl">به عنوان مثال میتونید به این شکل درخواست تبلیغ دهید</div>

```cs
public void Show () {
  TapsellPlus.showBannerAd (ZONE_ID, BannerType.BANNER_320x50, Gravity.BOTTOM, Gravity.CENTER);
}
```

## <div dir="rtl">آموزش تبلیغات همنما بنری</div>

<div dir="rtl">مطابق کد زیر درخواست تبلیغ دهید</div>

```cs
public void Request () {
  TapsellPlus.requestNativeBanner (this, ZONE_ID,
    (TapsellNativeBannerAd result) => {
      Debug.Log ("on response");
      NativeBannerScene.nativeAd = result;
    },
    (TapsellError error) => {
      Debug.Log ("Error " + error.error);
    }
  );
}
```

<div dir="rtl">متغیر برگردانده شده در on response محتویات تبلیغ هست و برای نمایش تبلیغ باید مطابق جدول زیر ازش استفاده کنید</div>

|           function          |     usage     |
|:---------------------------:|:-------------:|
|         getTitle  ()        |     عنوان     |
|      getDescription  ()     |    توضیحات    |
|         getIcon  ()         |      آیکن     |
| getLandscapeBannerImage  () |   تصویر افقی  |
|  getPortraitBannerImage  () |  تصویر عمودی  |
|     getCallToAction  (),    | متن دکمه کلیک |

<div dir="rtl">برای باز کردن تبلیغ زمان کلیک کاربر میتونید از این متد استفاده کنید</div>

```cs
nativeAd.clicked ();
```

برای دیدن یک نمونه پیاده سازی شده میتونید همین پروژه در گیت‌هاب را بررسی کنید
