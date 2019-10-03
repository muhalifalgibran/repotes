# Navigation Architecture

Navigasi mengacu pada interaksi yang memungkinkan pengguna menavigasi behavior layar seperti, melintasi, masuk, dan mundur dari berbagai konten dalam aplikasi.Navigation Component Android Jetpack membantu kita dalam menerapkan navigasi, mulai dari klik tombol sederhana hingga pola yang lebih kompleks, seperti bilah aplikasi dan laci navigasi.

**Masukan Dependensi**

```kotlin
dependencies {
  def nav_version = "2.1.0"

  // Java
  implementation "androidx.navigation:navigation-fragment:$nav_version"
  implementation "androidx.navigation:navigation-ui:$nav_version"

  // Kotlin
  implementation "androidx.navigation:navigation-fragment-ktx:$nav_version"
  implementation "androidx.navigation:navigation-ui-ktx:$nav_version"

}
```
Main Activity.xml

```xml
        <?xml version="1.0" encoding="utf-8"?>
        <androidx.constraintlayout.widget.ConstraintLayout
            xmlns:android="http://schemas.android.com/apk/res/android"
            xmlns:app="http://schemas.android.com/apk/res-auto"
            xmlns:tools="http://schemas.android.com/tools"
            android:layout_width="match_parent"
            android:id="@+id/drawer_layout"
            android:layout_height="match_parent"
            tools:context=".MainActivity">

            <fragment
                android:id="@+id/home_nav_host_fragment"
                android:name="androidx.navigation.fragment.NavHostFragment"
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                app:defaultNavHost="true"
                android:layout_marginBottom="20dp"
                app:layout_constraintBottom_toTopOf="@+id/bottom_nav_view"
                app:layout_constraintVertical_bias="0.0"
                app:navGraph="@navigation/main_nav" />

            <com.google.android.material.bottomnavigation.BottomNavigationView
                android:id="@+id/bottom_nav_view"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:background="@android:color/white"
                app:elevation="10dp"
                app:layout_constraintBottom_toBottomOf="parent"
                android:backgroundTint="@color/colorPrimaryDark"
                app:itemTextAppearanceInactive="@color/colorPrimaryDark"
                app:labelVisibilityMode="labeled"
                app:menu="@menu/bottom_nav_menu" />

        </androidx.constraintlayout.widget.ConstraintLayout>
```

*MainActivity.kt*
```
package id.alif.belajarnavarch

import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import android.view.MenuItem
import androidx.constraintlayout.widget.ConstraintLayout
import androidx.navigation.NavController
import androidx.navigation.findNavController
import androidx.navigation.fragment.NavHostFragment
import androidx.navigation.ui.AppBarConfiguration
import androidx.navigation.ui.NavigationUI.setupWithNavController
import androidx.navigation.ui.onNavDestinationSelected
import androidx.navigation.ui.setupActionBarWithNavController
import androidx.navigation.ui.setupWithNavController
import com.google.android.material.bottomnavigation.BottomNavigationView

class MainActivity : AppCompatActivity() {

    private lateinit var mNavController: NavController
    private lateinit var host: NavHostFragment
    private lateinit var appBarConfiguration: AppBarConfiguration
    private lateinit var mBottomNav: BottomNavigationView

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        setupNavController()
        setupActionBar(mNavController)
    }

    override fun onOptionsItemSelected(item: MenuItem): Boolean {
        return item.onNavDestinationSelected(findNavController(R.id.main_nav))
                ||super.onOptionsItemSelected(item)
    }

    private fun setupNavController(){
        host = supportFragmentManager
            .findFragmentById(R.id.home_nav_host_fragment) as NavHostFragment? ?: return
        mNavController = host.navController

        appBarConfiguration = AppBarConfiguration(
            setOf(R.id.detailPersonFragment, R.id.workspaceFragment)
        )

        mBottomNav = findViewById(R.id.bottom_nav_view)
        mBottomNav.setupWithNavController(mNavController)

    }

    private fun setupActionBar(navController: NavController){
        setupActionBarWithNavController(navController)
    }

}
```
