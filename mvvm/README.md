# MVVM Archetecture Pattern

![MVVM](https://cdn-images-1.medium.com/max/1600/1*BpxMFh7DdX0_hqX6ABkDgw.png)

## Steps

1. Add dependency in app module [link](https://developer.android.com/topic/libraries/architecture/adding-components#pre-androidx)

```gradle
dependencies {
    ...
    def lifecycle_version = "1.1.1"

    // ViewModel and LiveData
    implementation "android.arch.lifecycle:extensions:$lifecycle_version"
    // alternatively - just ViewModel
    implementation "android.arch.lifecycle:viewmodel:$lifecycle_version"
    ...
}
```

2. Update gradle.properties

```properties
android.databinding.enableV2=true
```