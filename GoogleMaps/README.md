# Карты Google


[Инфраструктура ПриватБанка. Банкоматы](https://api.privatbank.ua/#p24/atm)

* Добавить API ключ в файл `AndroidManifest.xml` [В соответсвии с инструкцией](https://developers.google.com/maps/documentation/android-sdk/start). 
Самый простой способ это добавить активность по шаблону **Google Maps Activity** и открыть ссылку из файла res/values/google_maps_api.xml в барузере

* Добавить зависимости в `build.gradle` [инструкция](https://developers.google.com/android/guides/setup) :

``` gradle

apply plugin: 'com.android.application'
...

dependencies {
    implementation 'com.google.android.gms:play-services-location:16.0.0'
    implementation 'com.google.android.gms:play-services-maps:16.0.0'
    implementation 'com.google...'
}

```

* Добавить в `AndroidManifest.xml` права:
```xml

<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>

```

