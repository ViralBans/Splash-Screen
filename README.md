# Splash-Screen

Splash Screen (прим.: тут и далее — экран загрузки) просто отнимает ваше время, верно? Вооруженные этим знанием, заставьте ваш Splash Screen работать правильно. Не тратьте время пользователей попусту, но дайте им то, на что им будет приятно смотреть пока они ждут.
***

|![Screenshot](https://github.com/VitaliBov/Screenshots-for-README/blob/master/S70209-13490777.jpg)|![Screenshot](https://github.com/VitaliBov/Screenshots-for-README/blob/master/S70209-13491383.jpg)|
| ------------- |:------------------:|

***

[Оригинал](https://habrahabr.ru/post/312516/)

[Примеры с анимациями для красивых переходов](http://saulmm.github.io/avoding-android-cold-starts)

***

Реализация Splash Screen


Реализация Splash Screen правильным способом немного отличается от того что вы можете себе приставить. Представление Splash Screen, который вы видите, должно быть готово немедленно, даже прежде чем вы можете раздуть (прим.: inflate) файл макета в вашей Splash Activity (прим.: Activity — активность, деятельность).

Поэтому мы не будем использовать файл макета. Вместо этого мы укажем фон нашего Splash Screen в фоне темы своей Activity. Для этого, сначала необходимо создать XML drawable в res/drawable.

```java
<?xml version="1.0" encoding="utf-8"?>
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">

    <item
        android:drawable="@color/gray"/>

    <item>
        <bitmap
            android:gravity="center"
            android:src="@mipmap/ic_launcher"/>
    </item>

</layer-list>
```

Здесь я задал цвет фона и изображение.

Дальше, вы должны установить этот drawable в качестве фона для темы вашего Splash Screen Activity. Перейдите в файл styles.xml и добавьте новую тему для Splash Screen Activity:

```java
<resources>

    <!-- Base application theme. -->
    <style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">
        <!-- Customize your theme here. -->
    </style>

    <style name="SplashTheme" parent="Theme.AppCompat.NoActionBar">
        <item name="android:windowBackground">@drawable/background_splash</item>
    </style>
</resources>
```

В вашей новой SplashTheme установите в качестве фона ваш XML drawable. И установите эту тему в своей Splash Screen Activity в вашем AndroidManifest.xml:

```java
<activity
    android:name=".SplashActivity"
    android:theme="@style/SplashTheme">
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />

        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
</activity>
```

И, наконец, ваш класс SplashActivity должен перенаправить вас в ваше основное Activity:

```java
public class SplashActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        Intent intent = new Intent(this, MainActivity.class);
        startActivity(intent);
        finish();
    }
}
```

Обратите внимание, что вы не настраивает вид для SplashActivity. Представление берется непосредственно из темы. Когда вы задаете вид вашей Splash Screen Activity через тему, он доступен немедленно.

Если у вас есть файл макета для вашей Splash Activity, он будет показан только после того как ваше приложение будет полностью инициализировано, а это что очень поздно. Ведь мы хотим чтобы Splash Screen отображался только небольшой промежуток времени, до того как приложение будет инициализировано.
