---
layout: post
title: Koin to Hilt
date: '2020-06-17 11:17:21'
intro_paragraph: ''
categories: android
---
# Koin to Hilt

Google 에서 새로운 DI(Dependency Injection) 인 [Hilt](https://medium.com/androiddevelopers/dependency-injection-on-android-with-hilt-67b6031e62d) 를 내놓았다.

DI 를 처음 적용 할때 Dagger 를 배우다 너무나도 어렵고, Kotlin 에 최적화 되어 있지 않은 듯한 내용들에

조금 느리지만 훨씬 직관적이고 관리가 쉬운 [Koin](https://insert-koin.io/) 을 선택했었는데,

그러한 것들을 개선 했다는 내용에 한번 적용해 보기로 했다.

[Dependency injection with Hilt](https://developer.android.com/training/dependency-injection/hilt-android) 를 보면 자세한 내용들이 많은데...

간단히 Koin 에서 Hilt 로 전환 하는 과정만 나열해 본다.

root build.gradle

```xml
buildscript {
    ...
    dependencies {
        ...
        classpath 'com.google.dagger:hilt-android-gradle-plugin:2.28-alpha'
    }
}
```

app/build.gradle

```xml
...
apply plugin: 'kotlin-kapt'
apply plugin: 'dagger.hilt.android.plugin'

android {
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}

dependencies {
    implementation "com.google.dagger:hilt-android:2.28-alpha"
    kapt "com.google.dagger:hilt-android-compiler:2.28-alpha"
		implementation "androidx.hilt:hilt-lifecycle-viewmodel:1.0.0-alpha01"
    kapt "androidx.hilt:hilt-compiler:1.0.0-alpha01"
}
```

Application Class

1. @HiltAndroidApp 추가
2. 기존 startKoin 삭제

```kotlin
@HiltAndroidApp
class App : Application() {
    override fun onCreate() {
        super.onCreate()
//        startKoin{
//            androidContext(this@App)
//            modules(appModule)
//        }
    }
}
```

Activity 에 @AndroidEntryPoint 추가

```kotlin
@AndroidEntryPoint
class MainActivity : AppCompatActivity() {
		...
}
```

di/AppModule.kt 여기가 중요한 부분인데..

1. @Module 을 만들고
2. single, factory 는 function 으로 생성하고
3. viewModel 쪽은 각각의 viewModel 생성자에 @ViewModelInject 처리 해준다.

이런 느낌?

아래는 Koin 소스

```kotlin
val appModule = module {
    single { AppDatabase.getInstance(androidContext()).itemDao() }
    single {
        Retrofit.Builder()
            .baseUrl(Define.URL_HOST)
            .addConverterFactory(GsonConverterFactory.create(GsonBuilder().create()))
            .apply {
                client(apiInterceptor)
            }
            .build().create(ItemService::class.java)
    }
    viewModel { MainViewModel() }
    viewModel { RoomPagedListViewModel(get()) }
    viewModel {
        NetworkPagedListViewModel(
            get()
        )
    }
}
```

변경된 Hilt 소스

```kotlin
@Module
@InstallIn(ApplicationComponent::class)
object AppModule {

    @Singleton
    @Provides
    fun provideItemDao(@ApplicationContext context: Context): ItemDao {
        return AppDatabase.getInstance(context).itemDao()
    }

    @Singleton
    @Provides
    fun provideItemService(@ApplicationContext context: Context): ItemService {
        return Retrofit.Builder()
            .baseUrl(Define.URL_HOST)
            .addConverterFactory(GsonConverterFactory.create(GsonBuilder().create()))
            .apply {
                client(apiInterceptor)
            }
            .build().create(ItemService::class.java)
    }
}
```

Fragment Class

1. @AndroidEntryPoint 추가
2. by viewModels 로 ViewModel 선언

```kotlin
@AndroidEntryPoint
class RoomPagedListFragment : BaseFragment<FragmentListPagedRoomBinding>() {
		//기존 koin ext
		//private val viewModel by viewModel<RoomPagedListViewModel>()

		//androidx.fragment.app.viewModels 이용
    private val viewModel by viewModels<RoomPagedListViewModel>()
		...
}
```

ViewModel class

1. @ViewModelInject 선언된 생성자 추가

```kotlin
class RoomPagedListViewModel @ViewModelInject constructor(private val itemDao: ItemDao) :
    ViewModel() {
		...
}
```

끝.

변경된 전체 소스는 [Github AndroidSample](https://github.com/junsucom/AndroidSample/commit/8b611ed139067ec183c924b936536308c1046ce5) 에.

생각보다 몇 줄 안되는 코드로 koin 에서 hilt 로 변경이 되었다.

아직 alpha 인 만큼 api 의 변경이 많이 있을 수 있으니.. 변경되는 사항들을 지켜보자.