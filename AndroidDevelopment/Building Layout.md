---
title: Building Layout
updated: 2022-11-22 12:05:30Z
created: 2022-10-29 14:15:23Z
latitude: 33.72938820
longitude: 73.09314610
altitude: 0.0000
---

- As said earlier, app layouts reside in the **res/layout** folders which contains .xml files in which layouts are written in descriptive manner.
- There is always a root **viewgroup** which holds other views or viewgroups. Module namespaces are also written inside of the root viewgroup. i.e., we take **LinearLayout** as root viewgroup for our example layout which would look like as

```XML
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">
</LinearLayout>
```

- **namespaces** for the above example are **android, tools**

# Layout Inflation

- When an XML layout resource is parsed and converted into a hierarchy of View objects is known as layout inflation.
- **No two views will have the same ID**

```XML
<ImageView
            android:id="@+id/star_image"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:contentDescription="@string/star_image_description"
            android:src="@android:drawable/btn_star_big_on" />
```

gets converted to kotlin var references like this

```kotlin
var starImage : ImageView = drawable/btn_star_big_on // (image source)
```

# Layout and Data Binding

- The era of static content has gone far away and in this age, dynamicity is the key to business success. Users wants to interact with our application, e.g., if he/she taps on a button, it should do something or display something.
- We are usually required to change attributes of some views on run-time to provide user-interaction like changing the button text upon clicking. For this, we are required to have **view reference** in our business logic. It can be done in numerous ways

## 1\. findViewByID()

- In this process, we pass the **view id** to the findViewByID() function and it'll traverse the whole layout tree finding the view matching the given id. This searching operation is costly and too much ids to find will cause our app to lag as it'll have to traverse the view hierarchy/layout tree repeatedly. Example is demonstrated below,

```kotlin
val imgRef : ImageView = findViewByID(R.id.star_image) or
val imgRef = findViewByID<ImageView>(R.id.star_image) or
findViewByID<ImageView>(R.id.star_image)
```

and our onCreate() method look like as follow,

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    
    setContentView(R.layout.activity_main)

}

```

## 2\. View Binding

- To use view binding feature, enable it in the **build.gradle(app:module)** file like this

```
buildFeatures{
    viewBinding true
}
```

and then click on the **sync** button appearing on the right top of the IDE. It'll fetch all the newly added features or dependencies added recently from the configured repositories.

- By using, view binding you can achieve significant performance gain. It'll generate class files for every xml layout resource file, and we can reference those attributes to get reference to our layout views. Our MainActivity.kt file will look like this,

```kotlin
class MainActivity : AppCompatActivity() {
    private lateinit var binding: ActivityMainBinding

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        
        binding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(binding.root)
                 
        // some examples
        binding.name_text = "Hello World!"	// refering to name_text view
        binding.age_text = "21"
        binding.submit_button = "Submit"
        binding.submit_button { "TODO" }
    }
}

```

## 3\. Data Binding

### 1\. Why Data Binding?

1.  **Updating data and then updating the data displayed in views is cumbersome and a source of errors**
2.  **Data binding solves both of these problems. You keep data in a data class. You add a `<data>` block to the `<layout>` to identify the data as variables to use with the views. Views reference the variables.**
3.  **The compiler generates a binding object that binds the views and data.**
4.  **In your code, you reference and update the data through the binding object, which updates the data, and thus what is displayed in the view.**

* * *

- findViewById is a costly operation because it traverses the view hierarchy every time it is called.
- With data binding enabled, the compiler creates references to all views in a `<layout>` that have an id, and gathers them in a binding object/class.
- In our code, we can create an instance of Binding object and then reference views through the binding object with no extra boilerplate required.

* * *

### 2\. How to Achieve?

- To enable data binding, open `buil.gradle(app:module)` inside the android section,
    
    ```
    buildFeatures{
        dataBinding true
    }
    ```
    
- Wrap all the views in your `<activity-name>.xml` into a `<layout>` tag, and move the namespace declarations to the `<layout>` tag.
- In the `<activity-name>.xml`, create a new binding object,
    `private lateint val binding : ActivityNameBinding`

* * *

- Our onCreate() method will look like this,

```kotlin
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
// using DataBindingUtil to set the content view		
        binding = DataBindingUtil.setContentView(this, R.layout.activity_main)

        // some examples
        binding.name_text = "Hello World!"	// refering to name_text view
        binding.doneButton.setOnClickListener….etc

    }

```

### Note: We can use apply() in the click handler to make our code more concise and readable

* * *

### 2\. Binding Data

#### 1\. Create a data class with the required fields/instance variables. e.g., in kotlin

```kotlin
data class MyName(var name : String = "", var nickName : String = "")
```

#### 2\. Add a `<data>` block to `<activity_name>.xml` file in the `<layout>` tag but before view tags. Inside the `<data>` block, create a `<variable>` tag for `MyName` class,

```
<data>
    <!-- Declare a variable by specifying a name and a data type. -->
    <!-- Use fully qualified name for the type. -->

    <variable>
        name = "myName"
        type = "com.example.android.aboutMe.MyName" />
</data>

```

#### 3\. Replace the references to string text resources with references to the variables, for example,

```kotlin
android:text="@={myName.name}"

```

#### 4\. Create an instance of MyName in the ActivityMain.kt file,

```kotlin
// Instance of MyName data class.

private val name : MyName = MyName("Muhammad Salman")
```

#### 5\. Bind the data to the view by setting binding.myName to name, e.g.,

```kotlin
// binding.<variable-name-set-in-the-data-block> = instance variable
binding.myName = name
```

### Note: We can call inValidateAll() to Invalidate all binding expressions and request a new rebind to refresh UI. (Optional)