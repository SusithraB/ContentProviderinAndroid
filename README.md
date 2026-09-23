
# Ex.No:3 Create Your Own Content Providers to get Contacts details.


## AIM:

To create your own content providers to get contacts details using Android Studio.

## EQUIPMENTS REQUIRED:

Android Studio(Min. required Artic Fox)

## ALGORITHM:

Step 1: Open Android Stdio and then click on File -> New -> New project.

Step 2: Then type the Application name as “ex.no.3″ and click Next. 

Step 3: Then select the Minimum SDK as shown below and click Next.

Step 4: Then select the Empty Activity and click Next. Finally click Finish.

Step 5: Design layout in activity_main.xml.

Step 6: Get contacts details and Display details give in MainActivity file.

Step 7: Save and run the application.

## PROGRAM:
```
Program to print SUSITHRA B
Registeration Number : 212223220113
```
## activity_main.xml
```
<?xml version="1.0" encoding="utf-8"?>

<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="24dp">

    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Contact Details"
        android:textSize="28sp"
        android:textStyle="bold"
        android:gravity="center"
        android:layout_marginTop="30dp"
        android:layout_marginBottom="20dp" />

    <Button
        android:id="@+id/btnLoadContacts"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Load Contacts"
        android:layout_marginBottom="20dp" />

    <ScrollView
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_weight="1">

        <TextView
            android:id="@+id/txtContacts"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:textSize="18sp"
            android:padding="10dp" />

    </ScrollView>

</LinearLayout>
```

## AndroidManifest.xml
```
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">
    <uses-permission android:name="android.permission.READ_CONTACTS" />

    <application
        android:allowBackup="true"
        android:dataExtractionRules="@xml/data_extraction_rules"
        android:fullBackupContent="@xml/backup_rules"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.ContentProvider">
        <activity
            android:name=".MainActivity"
            android:exported="true"
            android:windowSoftInputMode="adjustResize">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```
## MainActivity.java
```
package com.aishwarya.contentprovider;

import android.Manifest;
import android.content.pm.PackageManager;
import android.database.Cursor;
import android.os.Bundle;
import android.provider.ContactsContract;
import android.util.Log;
import android.widget.Button;
import android.widget.TextView;
import android.widget.Toast;

import androidx.activity.result.ActivityResultLauncher;
import androidx.activity.result.contract.ActivityResultContracts;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.content.ContextCompat;

public class MainActivity extends AppCompatActivity {

    private TextView txtContacts;

    private final ActivityResultLauncher<String> permissionLauncher =
            registerForActivityResult(
                    new ActivityResultContracts.RequestPermission(),
                    isGranted -> {
                        if (isGranted) {
                            loadContacts();
                        } else {
                            Toast.makeText(
                                    this,
                                    "Contacts permission denied",
                                    Toast.LENGTH_SHORT
                            ).show();
                        }
                    }
            );

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        Button btnLoadContacts = findViewById(R.id.btnLoadContacts);
        txtContacts = findViewById(R.id.txtContacts);

        btnLoadContacts.setOnClickListener(view -> {

            if (ContextCompat.checkSelfPermission(
                    this,
                    Manifest.permission.READ_CONTACTS
            ) == PackageManager.PERMISSION_GRANTED) {

                loadContacts();

            } else {
                permissionLauncher.launch(Manifest.permission.READ_CONTACTS);
            }
        });
    }

    private void loadContacts() {

        StringBuilder contactList = new StringBuilder();

        Cursor cursor = getContentResolver().query(
                ContactsContract.CommonDataKinds.Phone.CONTENT_URI,
                new String[]{
                        ContactsContract.CommonDataKinds.Phone.DISPLAY_NAME,
                        ContactsContract.CommonDataKinds.Phone.NUMBER
                },
                null,
                null,
                ContactsContract.CommonDataKinds.Phone.DISPLAY_NAME + " ASC"
        );

        if (cursor != null) {

            int nameIndex = cursor.getColumnIndex(
                    ContactsContract.CommonDataKinds.Phone.DISPLAY_NAME
            );

            int numberIndex = cursor.getColumnIndex(
                    ContactsContract.CommonDataKinds.Phone.NUMBER
            );

            while (cursor.moveToNext()) {

                String name = cursor.getString(nameIndex);
                String number = cursor.getString(numberIndex);

                // Display in Logcat
                Log.i(
                        "CONTACT_DETAILS",
                        "Contact Name ::: " + name +
                                " Phone Number ::: " + number
                );

                // Display inside app
                contactList.append("Contact Name: ")
                        .append(name)
                        .append("\nPhone Number: ")
                        .append(number)
                        .append("\n\n");
            }

            cursor.close();
        }

        if (contactList.length() == 0) {
            txtContacts.setText("No contacts found");
        } else {
            txtContacts.setText(contactList.toString());
        }
    }
}
```

## OUTPUT

<img width="720" height="1600" alt="image" src="https://github.com/user-attachments/assets/72e37ae9-91bc-4530-8e83-df3eb1f0beef" />

<img width="720" height="1600" alt="image" src="https://github.com/user-attachments/assets/c69a67f4-b7aa-4ad5-bc7e-b73d581cf29d" />

<img width="1917" height="1078" alt="Screenshot 2026-07-29 111627" src="https://github.com/user-attachments/assets/0db770d7-c6ae-4892-925e-5e862d9d8430" />


## RESULT
Thus a Simple Android Application create your own content providers to get contacts details using Android Studio is developed and executed successfully.
