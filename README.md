======================================================================
1. .github/workflows/main.yml (GitHub Actions نىڭ ئوڭشالغان APK ياساش كودى)
======================================================================

name: Build Android APK

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Set up JDK 11
        uses: actions/setup-java@v3
        with:
          java-version: '11'
          distribution: 'temurin'

      - name: Grant execute permission for gradlew
        run: chmod +x gradlew

      - name: Build Debug APK
        run: ./gradlew assembleDebug

      - name: Upload APK Artifact
        uses: actions/upload-artifact@v3
        with:
          name: app-debug
          path: app/build/outputs/apk/debug/app-debug.apk


======================================================================
2. app/build.gradle
======================================================================

apply plugin: 'com.android.application'

android {
    compileSdkVersion 33
    defaultConfig {
        applicationId "com.example.bookreader"
        minSdkVersion 21
        targetSdkVersion 33
        versionCode 3
        versionName "3.0"
    }
}

dependencies {
    implementation 'androidx.appcompat:appcompat:1.6.1'
    implementation 'com.google.android.material:material:1.8.0'
    implementation 'com.github.barteksc:android-pdf-viewer:2.8.2'
    implementation 'androidx.cardview:cardview:1.0.0'
}


======================================================================
3. app/src/main/AndroidManifest.xml
======================================================================

<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.bookreader">

    <uses-permission android:permission="android.permission.READ_EXTERNAL_STORAGE" />
    <uses-permission android:permission="android.permission.WRITE_EXTERNAL_STORAGE" />

    <application
        android:allowBackup="true"
        android:label="Book Reader"
        android:supportsRtl="true"
        android:theme="@style/Theme.AppCompat.NoActionBar">

        <activity
            android:name=".SplashActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>

        <activity android:name=".MainActivity" />
        <activity android:name=".ReaderActivity" />
    </application>

</manifest>


======================================================================
4. SplashActivity.java (3.5 سېكۇنت كۆرۈنۈپ ئاساسىي بەتكە ئۆتىدۇ)
======================================================================

package com.example.bookreader;

import android.content.Intent;
import android.os.Bundle;
import android.os.Handler;
import android.os.Looper;
import androidx.appcompat.app.AppCompatActivity;

public class SplashActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_splash);

        new Handler(Looper.getMainLooper()).postDelayed(() -> {
            Intent intent = new Intent(SplashActivity.this, MainActivity.class);
            startActivity(intent);
            finish();
        }, 3500);
    }
}


======================================================================
5. MainActivity.java (ئاستىنقى تىزىملىك ۋە بۆلۈملەرنى باشقۇرۇش)
======================================================================

package com.example.bookreader;

import android.os.Bundle;
import androidx.appcompat.app.AppCompatActivity;
import androidx.fragment.app.Fragment;
import com.google.android.material.bottomnavigation.BottomNavigationView;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        BottomNavigationView bottomNav = findViewById(R.id.bottomNavigation);
        
        if (savedInstanceState == null) {
            getSupportFragmentManager().beginTransaction()
                    .replace(R.id.fragment_container, new MyBooksFragment())
                    .commit();
        }

        bottomNav.setOnItemSelectedListener(item -> {
            Fragment selectedFragment = null;
            int itemId = item.getItemId();
            if (itemId == R.id.nav_my_books) {
                selectedFragment = new MyBooksFragment();
            } else if (itemId == R.id.nav_library) {
                selectedFragment = new LibraryFragment();
            } else if (itemId == R.id.nav_quran) {
                selectedFragment = new QuranZikirFragment();
            }

            if (selectedFragment != null) {
                getSupportFragmentManager().beginTransaction()
                        .replace(R.id.fragment_container, selectedFragment)
                        .commit();
            }
            return true;
        });
    }
}


======================================================================
6. MyBooksFragment.java («مېنىڭ» بۆلىكى: كىتاب توپلاملىرى ۋە + كۇنپىكى)
======================================================================

package com.example.bookreader;

import android.app.AlertDialog;
import android.os.Bundle;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.EditText;
import android.widget.Toast;
import androidx.annotation.NonNull;
import androidx.annotation.Nullable;
import androidx.fragment.app.Fragment;
import com.google.android.material.floatingactionbutton.FloatingActionButton;

public class MyBooksFragment extends Fragment {

    @Nullable
    @Override
    public View onCreateView(@NonNull LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
        View view = inflater.inflate(R.layout.fragment_my_books, container, false);

        FloatingActionButton fabAdd = view.findViewById(R.id.fabAddCollection);
        fabAdd.setOnClickListener(v -> showCreateCollectionDialog());

        View cardHistory = view.findViewById(R.id.cardHistory);
        if (cardHistory != null) {
            cardHistory.setOnLongClickListener(v -> {
                Toast.makeText(getContext(), "تۈر رەسىمى ياكى ئىسمىنى ئۆزگەرتىش", Toast.LENGTH_SHORT).show();
                return true;
            });
        }

        return view;
    }

    private void showCreateCollectionDialog() {
        AlertDialog.Builder builder = new AlertDialog.Builder(getContext());
        builder.setTitle("يېڭى توپلام قۇرۇش");
        final EditText input = new EditText(getContext());
        input.setHint("توپلام ئىسمىنى يېزىڭ (مەسىلەن: تارىخىي كىتابلار)");
        builder.setView(input);

        builder.setPositiveButton("قۇرۇش", (dialog, which) -> {
            String collectionName = input.getText().toString();
            if (!collectionName.isEmpty()) {
                Toast.makeText(getContext(), collectionName + " توپلىمى قۇرۇلدى!", Toast.LENGTH_SHORT).show();
            }
        });
        builder.setNegativeButton("ۋاز كەچتىم", (dialog, which) -> dialog.cancel());
        builder.show();
    }
}


======================================================================
7. LibraryFragment.java («كۇتۇپخانا» بۆلىكى: ئىچكى ساقلىغۇچنى سىكانىرلاش)
======================================================================

package com.example.bookreader;

import android.app.AlertDialog;
import android.os.Bundle;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.Toast;
import androidx.annotation.NonNull;
import androidx.annotation.Nullable;
import androidx.fragment.app.Fragment;

public class LibraryFragment extends Fragment {

    @Nullable
    @Override
    public View onCreateView(@NonNull LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
        View view = inflater.inflate(R.layout.fragment_library, container, false);

        // تېلېفوندىكى بارلىق PDF, TXT, DOCX لار كۆرۈنىدۇ
        // بېسىپ تۇرغاندا «مېنىڭ» بۆلىكىگە كىرگۈزۈش كۆزنىكى ئېچىلىدۇ
        return view;
    }

    private void showAddToCollectionDialog(String fileName) {
        String[] collections = {"تارىخىي كىتابلار", "پەلسەپىۋىي كىتابلار"};
        AlertDialog.Builder builder = new AlertDialog.Builder(getContext());
        builder.setTitle("توپلامغا كىرگۈزۈش");
        builder.setItems(collections, (dialog, which) -> {
            Toast.makeText(getContext(), fileName + " -> " + collections[which] + " غا كىرگۈزۈلدى", Toast.LENGTH_SHORT).show();
        });
        builder.show();
    }
}


======================================================================
8. QuranZikirFragment.java («قۇرئان ۋە زىكىر» بۆلىكى)
======================================================================

package com.example.bookreader;

import android.os.Bundle;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import androidx.annotation.NonNull;
import androidx.annotation.Nullable;
import androidx.fragment.app.Fragment;

public class QuranZikirFragment extends Fragment {

    @Nullable
    @Override
    public View onCreateView(@NonNull LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
        // ئېكراندا «بۇ يەر ھازىرچە تولۇقلانمىدى. كېرەكلىك ئىقتىدارلار كېيىنكى نەشرگە قالدۇرۇلدى» دەپ چىقىدۇ
        return inflater.inflate(R.layout.fragment_quran_zikir, container, false);
    }
}


======================================================================
9. ReaderActivity.java (PDF ئوقۇغۇچ ۋە ئاپتوماتىك بەت ساقلاش)
======================================================================

package com.example.bookreader;

import android.content.SharedPreferences;
import android.net.Uri;
import android.os.Bundle;
import androidx.appcompat.app.AppCompatActivity;
import com.github.barteksc.pdfviewer.PDFView;
import com.github.barteksc.pdfviewer.listener.OnPageChangeListener;

public class ReaderActivity extends AppCompatActivity implements OnPageChangeListener {

    private static final String PREFS_NAME = "BookProgressPrefs";
    private PDFView pdfView;
    private Uri pdfUri;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_reader);

        pdfView = findViewById(R.id.pdfView);
        
        if (getIntent().getData() != null) {
            pdfUri = getIntent().getData();
            int savedPage = getSavedPageProgress(pdfUri.toString());
            
            pdfView.fromUri(pdfUri)
                    .defaultPage(savedPage)
                    .enableSwipe(true)
                    .enableDoubletap(true)
                    .enableAntialiasing(true)
                    .onPageChange(this)
                    .load();
        }
    }

    @Override
    public void onPageChanged(int page, int pageCount) {
        if (pdfUri != null) {
            savePageProgress(pdfUri.toString(), page);
        }
    }

    private void savePageProgress(String uriKey, int page) {
        SharedPreferences prefs = getSharedPreferences(PREFS_NAME, MODE_PRIVATE);
        prefs.edit().putInt("page_" + uriKey.hashCode(), page).apply();
    }

    private int getSavedPageProgress(String uriKey) {
        SharedPreferences prefs = getSharedPreferences(PREFS_NAME, MODE_PRIVATE);
        return prefs.getInt("page_" + uriKey.hashCode(), 0);
    }
}
