---
title: App Navigation
updated: 2022-12-31 04:33:59Z
created: 2022-11-22 09:56:21Z
latitude: 51.50735090
longitude: -0.12775830
altitude: 0.0000
---

# Navigation Components / Android JetPack Navigation
## 1\. Fragments

- A fragment represents a reusable portion of an application UI's. Fragments doesn't exsits independently. They've to be hosted in an activity or inside another fragment.
- **Fragments manages its own layout, has its own lifecycle and handle its own input events.**
- In fragments,we don't inflate views in onCreate() like in an Activity rather than it gets inflated in onCreateView() method.

### [Documentation](https://developer.android.com/guide/fragments)
### [Navigation CodeLab](https://developer.android.com/codelabs/android-navigation#1)

## 2\. Principles of Navigations

1.  **There's always a starting place.** This destination should also be the last screen when user returns to the default launcher when the back button is pressed.
2.  **You can always go back.** App should follow the LIFO/FILO principle(back-stack). Operations that manipulate the back-stack should be performed on the top of the navigation stack, either by pushing off a new navigation destination or by popping the top most destination off the stack.
3.  **Up goes back(mostly).**

#### It's important to implement these principles across wide range of android apps/devices to provide consistent and predictable experiences for users.

## 3\. Ways of Navigations
### [Start Here](https://developer.android.com/guide/navigation/navigation-getting-started)
### [Developers Documentations](https://developer.android.com/guide/navigation/navigation-navigate)
### a. Navigate using ID
```

viewTransactionsButton.setOnClickListener { view ->
   view.findNavController().navigate(R.id.viewTransactionsAction)
}


```


## 4\. Menu
### Optimize the imports after writing the following code snippet. This snippet is for showing the menu in a Fragment.
```

private fun setupMenu() {
        (requireActivity() as MenuHost).addMenuProvider(object : MenuProvider {

            override fun onCreateMenu(menu: Menu, menuInflater: MenuInflater) {
                menuInflater.inflate(R.menu.your_menu, menu)
            }

            override fun onMenuItemSelected(menuItem: MenuItem): Boolean {
                // Validate and handle the selected menu item
                return when (menuItem.itemId) {
                    R.id.menuItem_id(about_menu) -> {
                        view?.findNavController()?.navigate(R.id.aboutFragment)
                        true
                    }
                    else -> false
                }
            }
        }, viewLifecycleOwner, Lifecycle.State.RESUMED)
    }
	
```

## --- OR ---

```kotlin

    private fun setupMenu() {
        (requireActivity() as MenuHost).addMenuProvider(object : MenuProvider {

            override fun onCreateMenu(menu: Menu, menuInflater: MenuInflater) {
                menuInflater.inflate(R.menu.overflow_menu, menu)
            }

            override fun onMenuItemSelected(menuItem: MenuItem): Boolean {
                // Validate and handle the selected menu item

                return menuItem.onNavDestinationSelected(
                    findNavController()
                ) || if (menuItem.itemId == R.id.exit_menu) {
                    requireActivity().finish()
                    true
                } else {
                    false
                }
//                return when (menuItem.itemId) {
//                    R.id.about_menu -> {
//                        view?.findNavController()?.navigate(R.id.aboutFragment)
//                        true
//                    }
//                    R.id.exit_menu -> {
//                        requireActivity().finish()
//                        true
//                    }
//                    else -> false
//                }
            }
        }, viewLifecycleOwner, Lifecycle.State.RESUMED)
    }


```

## 5. Safe Args / Pass data between Destinations
- Fragments contains arguments in the form of android bundle which essentially is key value store.
- Allows to pass data in between different fragments or activities. Should be used when small amount of data is needed to be transferred like some variables. If an object is needed to be pass use View Model.

### a. Add safe-args dependency to project Gradle file
```
// Adding the safe-args dependency to the project Gradle file
buildscript {

    repositories {
        google()
    }
	
    dependencies {
        def nav_version = "2.5.3"
        classpath "androidx.navigation:navigation-safe-args-gradle-plugin:$nav_version"
    }

}
```


### b. Add safeargs plugin to app Gradle file
```

// Adding the apply plugin statement for safeargs
plugins {
    id 'kotlin-kapt'
    id 'androidx.navigation.safeargs'
}


```


### c. Use generated NavDirections for Navigating 
- Make new arguments in the destination -> set its name and type.
- While navigating to that destination, pass the data to the actions' method.

```

view.findNavController().navigate(GameFragmentDirections.actionGameFragmentToGameOverFragment(correctQuestion, totalQuestions))


```
### d. Receiving Data
- Get reference to the safe args bundle and use that reference to extract the data.
	- `val args = <SenderFragment>Args.fromBundle(requireArguments())`
- In this code snippet, am receiving data from the GameWonFragment about number of correct questions and total questions. And displaying it as Toast.

```
val args = GameWonFragmentArgs.fromBundle(requireArguments())
Toast.makeText(
            context,
            "Correct Answers: ${args.numCorrect}, Total Questions: ${args.numQuestions}",
            Toast.LENGTH_LONG
        ).show()


```
### [Further Reading / Documentation](https://developer.android.com/guide/navigation/navigation-pass-data#groovy)

## 5. Intents and Sharing
- **Intents can be**
	- Explicit - Launch specific activites within your application, specific local store in the world
	- Implicit - Specify what you want done and System does that Job for you, all stores within 5 miles of radius that sold organic cabbages
	- **A System might not have specific application that can handle an Implicit intent, just there might not be any local grocery store near you**
	- **If there are multiple apps that can handle the specified implicit intent, Android will pop up a chooser with all the compatible applications to choose from.**
	- Sharing text, images or game conquest to other apps are good examples of implicit intents.
- **Intent Actions**
	- Each implicit action must have an intent action
	- Type of thing that the app wants done on its behalf
		- Common intent actions are  ACTION_VIEW,  ACTION_DIAL  and ACTION_EDIT
- **Intent Category**
	- Adds a subtype to the action
	- Implicit intents have further categories and datatypes to further describe the operation 
		- CATEGORY_APP_GALLERY
		- CATEGORY_APP_MAPS
		- CATEGORY_APP_EMAIL
		- CATEGORY_APP_CALENDAR
- **Intent Datatypes / MIME Data Type**
	- Allows activites to support specific data types
- **Instructions for Multiple Activities**
	- All activites must be registered in the Android Manifest to be launched.
	- Activities can be navigated/started using startActivity()
	- Activites that are only launched **explicitly** can be declared with just an `<activity> ... </activity>` tag.
	- **Implicitly** launched activites require `<intent-filter> ... </intent-filter>`
		- An intent-filter is used to expose that your app/activity can respond to an implicit intent with a certain action, category or data type
### Exercise: Adding a Share Intent
### Written in Recursive Manner
1. Declare that our Fragment has a Menu
2. Override onMenuItemSelected() to link the menu to shareSucess()
```

override fun onMenuItemSelected(menuItem: MenuItem): Boolean {
        // Validate and handle the selected menu item
        return when (menuItem.itemId) {
               R.id.share -> {
                  shareSuccess()
                    true
               }
               else -> false
       }
}


```
3. Create a shareSucess() that starts the activity from the share intent
```

// Starting an Activity with our new Intent
private fun shareSuccess() {
   startActivity(getShareIntent())
}

```
4. Create a getShareIntent() method. Get the args and build the shareIntent inside.
```

    // Creating our Share Intent
    private fun getShareIntent(): Intent {
        val args = GameWonFragmentArgs.fromBundle(requireArguments())
        val shareIntent = Intent(Intent.ACTION_SEND)
        shareIntent.setType("text/plain").putExtra(
            Intent.EXTRA_TEXT,
            getString(R.string.share_success_text, args.numCorrect, args.numQuestions)
        )
        return shareIntent
    }


```

5. Showing the Menu Item Dynamically
```

	override fun onCreateMenu(menu: Menu, menuInflater: MenuInflater) {
                menuInflater.inflate(R.menu.winner_menu, menu)
                // check if the activity resolves
                if (null == getShareIntent().resolveActivity(activity!!.packageManager)) {
                    // hide the menu item if it doesn't resolve
                    menu.findItem(R.id.share)?.isVisible = false
                }
            }


```
## 6. Navigation Drawer
- Used when the app has 5 or more primary destinations
- Also known as Hamburger menu, usually contains header image and menu items
- **Navigation drawer is part of Material Design library** and we start by including its' dependancy to our **app gradle file** with following lines of code
```
implementation 'com.google.android.material:material:1.7.0'

```
### Navigation drawer is based upon menu resource
1. Create the navdrawer_menu and add as many items you want
2. Add the DrawerLayout into the activity_main layout containing the LinearLayout and navHostFragment.
3. Add the NavigationView at the bottom of the the DrawerLayout. Layout would look like this:
```xml
<?xml version="1.0" encoding="utf-8"?>

<layout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto">

    <androidx.drawerlayout.widget.DrawerLayout
        android:id="@+id/drawerLayout"
        android:layout_width="match_parent"
        android:layout_height="match_parent">

        <LinearLayout xmlns:tools="http://schemas.android.com/tools"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            tools:context=".MainActivity">

            <androidx.fragment.app.FragmentContainerView
                android:id="@+id/fragmentContainerView"
                android:name="androidx.navigation.fragment.NavHostFragment"
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                app:defaultNavHost="true"
                app:navGraph="@navigation/main_nav" />
        </LinearLayout>

        <com.google.android.material.navigation.NavigationView
            android:id="@+id/navView"
            android:layout_width="wrap_content"
            android:layout_height="match_parent"
            android:layout_gravity="start"
            app:menu="@menu/navdrawer_menu" />

    </androidx.drawerlayout.widget.DrawerLayout>

</layout>

```

4. Move to MainActivity and add private lateinit vars for drawerLayout and appBarConfiguration
```
private lateinit var drawerLayout: DrawerLayout
private lateinit var appBarConfiguration: AppBarConfiguration
```
5. 
6. 


## 7. Animations
- Animations can be called upon using NavOptions() and also via navigation actions
- NavigationUI or its kotlin extension are used for menu items

 