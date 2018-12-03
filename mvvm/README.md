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

3. Add model

```java
public class User {
    private String email;
    private String password;
    ...
}
```

4. Add ViewModel

```java
public class LoginViewModel extends ViewModel {
    MutableLiveData<String> email = new MutableLiveData<>();
    MutableLiveData<String> password = new MutableLiveData<>();

    MutableLiveData<User> userMutableLiveData = new MutableLiveData<>();

    // Getters and Setters

    public void onClickListener() {
        User user = new User(email.getValue(), password.getValue());
        userMutableLiveData.setValue(user);
    }
}
```

5. Write to activity

```java
public class LoginActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        ActivityLoginBinding binding = DataBindingUtil.setContentView(this, R.layout.activity_login);
        LoginViewModel loginViewModel = ViewModelProviders.of(this).get(LoginViewModel.class);
        binding.setLoginViewModel(loginViewModel);
        binding.setLifecycleOwner(this);

        loginViewModel.getUser().observe(this, new Observer<User>() {
            @Override
            public void onChanged(@Nullable User user) {
                // Get user data
            }
        });

    }
}
```

5. Edit layout

```xml
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tool"
    xmlns:app="http://schemas.android.com/apk/res-auto">

    <data>
        <variable
            name="loginViewModel"
            type="itstep.org.firebasedemo2.LoginViewModel" />
    </data>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        ...


        <ScrollView
            android:id="@+id/login_form"
            android:layout_width="match_parent"
            android:layout_height="match_parent">

            <LinearLayout
                android:id="@+id/email_login_form"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:orientation="vertical">

                <android.support.design.widget.TextInputLayout
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content">

                    <AutoCompleteTextView
                        android:id="@+id/email"
                        android:layout_width="match_parent"
                        android:layout_height="wrap_content"
                        android:hint="@string/prompt_email"
                        android:text="@={loginViewModel.email}"
                        android:inputType="textEmailAddress"
                        android:maxLines="1"
                        android:singleLine="true" />

                </android.support.design.widget.TextInputLayout>

                <android.support.design.widget.TextInputLayout
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    app:passwordToggleEnabled="true">

                    <EditText
                        android:id="@+id/password"
                        android:layout_width="match_parent"
                        android:layout_height="wrap_content"
                        android:hint="@string/prompt_password"

                        android:imeActionId="6"
                        android:imeActionLabel="@string/action_sign_in_short"
                        android:imeOptions="actionUnspecified"
                        android:inputType="textPassword"
                        android:maxLines="1"
                        android:singleLine="true"
                        android:text="@={loginViewModel.password}"
                        />

                </android.support.design.widget.TextInputLayout>

                <Button
                    android:id="@+id/email_sign_in_button"
                    style="?android:textAppearanceSmall"
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:layout_marginTop="16dp"
                    android:onClick="@{() -> loginViewModel.onClickListener()}"
                    android:text="@string/action_sign_in"
                    android:textStyle="bold" />

            </LinearLayout>
        </ScrollView>
    </LinearLayout>
</layout>
```