# Refleks Oyunu (Android Studio - Kotlin)

Bu proje, Android Studio ile yazılmış küçük çaplı bir refleks ölçme oyunudur.

## Özellikler
- Oyun ekranı başlangıçta mor renkte gelir.
- 1-7 saniye arasında rastgele bir sürede ekran yeşile döner.
- Yeşil ekran geldiğinde kullanıcı dokunduğunda ne kadar hızlı tepki verdiği saniye cinsinden gösterilir.
- Tepki süresi "Tebrikler! X,XX saniye içinde tepki verdiniz" formatında yazılır.
- Oyun sonunda "Ana Menüye Dön" butonu ile tekrar oynanabilir.

## Kullanılan Teknolojiler
- Kotlin
- Android SDK
- XML

## Ekranlar
- Ana menü
- Oyun ekranı
- Sonuç ekranı

# activity_main.xml

<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:gravity="center"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#1E1E1E"
    android:padding="16dp">

    <Button
        android:id="@+id/startGameButton"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Oyunu Başlat" />

</LinearLayout>

# AndroidManifest.xml

<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">

    <application
        android:allowBackup="true"
        android:dataExtractionRules="@xml/data_extraction_rules"
        android:fullBackupContent="@xml/backup_rules"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.KadirOyun"
        tools:targetApi="31">
        <activity android:name=".GameActivity" />

        <activity
            android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>

# MainActivity.kt

package com.example.kadiroyun

import android.content.Intent
import android.os.Bundle
import android.widget.Button
import androidx.appcompat.app.AppCompatActivity

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        val startGameButton = findViewById<Button>(R.id.startGameButton)
        startGameButton.setOnClickListener {
            val intent = Intent(this, GameActivity::class.java)
            startActivity(intent)
        }
    }
}


# GameActivity.kt

package com.example.kadiroyun

import android.content.Intent
import android.graphics.Color
import android.os.*
import android.widget.*
import androidx.appcompat.app.AppCompatActivity
import kotlin.random.Random

class GameActivity : AppCompatActivity() {
    private lateinit var rootLayout: RelativeLayout
    private lateinit var resultTextView: TextView
    private lateinit var backToMenuButton: Button
    private var canClick = false
    private var startTime: Long = 0L

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_game)

        rootLayout = findViewById(R.id.rootLayout)
        resultTextView = findViewById(R.id.resultTextView)
        backToMenuButton = findViewById(R.id.backToMenuButton)

        val delay = Random.nextLong(1000, 7000)

        Handler(Looper.getMainLooper()).postDelayed({
            rootLayout.setBackgroundColor(Color.GREEN)
            canClick = true
            startTime = SystemClock.elapsedRealtime()
        }, delay)

        rootLayout.setOnClickListener {
            if (canClick) {
                val reactionTime = (SystemClock.elapsedRealtime() - startTime).toFloat() / 1000
                resultTextView.text = "Tebrikler! $reactionTime saniye içinde tepki verdiniz"
                resultTextView.visibility = TextView.VISIBLE
                backToMenuButton.visibility = Button.VISIBLE
                canClick = false
            }
        }

        backToMenuButton.setOnClickListener {
            val intent = Intent(this, MainActivity::class.java)
            startActivity(intent)
            finish()
        }
    }
}

# activitygame.xml

<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/rootLayout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#800080">

    <TextView
        android:id="@+id/resultTextView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text=""
        android:textSize="24sp"
        android:textColor="#FFFFFF"
        android:layout_centerHorizontal="true"
        android:layout_marginTop="300dp"
        android:visibility="gone" />

    <Button
        android:id="@+id/backToMenuButton"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Ana Menüye Dön"
        android:layout_below="@id/resultTextView"
        android:layout_centerHorizontal="true"
        android:layout_marginTop="20dp"
        android:visibility="gone" />

</RelativeLayout>

# 
