
# Ex.No 4: Create Your Own Content Providers to Get Contacts Details

## Aim
To create an Android application that uses a content provider to fetch and display the contact details (name and phone number) stored on the device, using Android Studio.

## Equipment Required
- Android Studio (Arctic Fox or higher)
- Android Emulator or a physical Android device

## Algorithm
1. Open Android Studio and click **File -> New -> New Project**.
2. Select **Empty Views Activity** and enter the application name.
3. Choose the language as **Java** and the minimum SDK, then click **Finish**.
4. Design the layout in `activity_main.xml` with a TextView and a Button.
5. Add the `READ_CONTACTS` permission in `AndroidManifest.xml`.
6. In `MainActivity.java`, check and request the permission at runtime.
7. Query `ContactsContract.CommonDataKinds.Phone.CONTENT_URI` using `ContentResolver`.
8. Read the name and number from the `Cursor` and display them in the TextView.
9. Save and run the application.

## Project Structure
```
app/
 ├── manifests/
 │    └── AndroidManifest.xml
 ├── java/com.example.content_provider/
 │    └── MainActivity.java
 └── res/
      ├── layout/activity_main.xml
      └── values/strings.xml
```

## Permission Used
```xml
<uses-permission android:name="android.permission.READ_CONTACTS"/>
```

## Activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#F4F6FC"
    android:padding="20dp"
    tools:context=".MainActivity">

    <TextView
        android:id="@+id/textView"
        android:layout_width="0dp"
        android:layout_height="0dp"
        android:layout_marginBottom="20dp"
        android:background="#FFFFFF"
        android:gravity="top"
        android:padding="16dp"
        android:scrollbars="vertical"
        android:text="@string/contact_details_header"
        android:textColor="#5E35B1"
        android:textSize="20sp"
        android:textStyle="bold"
        app:layout_constraintBottom_toTopOf="@+id/button"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <Button
        android:id="@+id/button"
        android:layout_width="220dp"
        android:layout_height="55dp"
        android:layout_marginBottom="30dp"
        android:backgroundTint="#3F51B5"
        android:onClick="btnGetContactPressed"
        android:text="@string/btn_get_contacts"
        android:textAllCaps="false"
        android:textColor="#FFFFFF"
        android:textSize="18sp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent" />

</androidx.constraintlayout.widget.ConstraintLayout>
```
## MainActivity.java
```java
package com.example.exp4_contentprovider;

import android.Manifest;
import android.content.pm.PackageManager;
import android.database.Cursor;
import android.os.Bundle;
import android.provider.ContactsContract;
import android.text.method.ScrollingMovementMethod;
import android.view.View;
import android.widget.TextView;
import android.widget.Toast;

import androidx.activity.EdgeToEdge;
import androidx.annotation.NonNull;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.app.ActivityCompat;
import androidx.core.content.ContextCompat;
import androidx.core.graphics.Insets;
import androidx.core.view.ViewCompat;
import androidx.core.view.WindowInsetsCompat;

public class MainActivity extends AppCompatActivity {

    private TextView textView;
    private static final int PERMISSIONS_REQUEST_READ_CONTACTS = 1;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        EdgeToEdge.enable(this);
        setContentView(R.layout.activity_main);
        ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.main), (v, insets) -> {
            Insets systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars());
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom);
            return insets;
        });

        textView = findViewById(R.id.textView);
        textView.setMovementMethod(new ScrollingMovementMethod());
    }

    public void btnGetContactPressed(View view) {
        getPhoneContacts();
    }

    private void getPhoneContacts() {
        if (ContextCompat.checkSelfPermission(this, Manifest.permission.READ_CONTACTS) != PackageManager.PERMISSION_GRANTED) {
            ActivityCompat.requestPermissions(this, new String[]{Manifest.permission.READ_CONTACTS}, PERMISSIONS_REQUEST_READ_CONTACTS);
            return;
        }

        StringBuilder stringBuilder = new StringBuilder();
        stringBuilder.append(getString(R.string.contact_details_header));
        
        Cursor cursor = getContentResolver().query(
                ContactsContract.CommonDataKinds.Phone.CONTENT_URI,
                null,
                null,
                null,
                ContactsContract.CommonDataKinds.Phone.DISPLAY_NAME + " ASC"
        );

        if (cursor != null) {
            while (cursor.moveToNext()) {
                String name = cursor.getString(cursor.getColumnIndexOrThrow(ContactsContract.CommonDataKinds.Phone.DISPLAY_NAME));
                String number = cursor.getString(cursor.getColumnIndexOrThrow(ContactsContract.CommonDataKinds.Phone.NUMBER));
                
                stringBuilder.append(name).append(" : ").append(number).append("\n\n");
            }
            cursor.close();
        }

        textView.setText(stringBuilder.toString());
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {
        super.onRequestPermissionsResult(requestCode, permissions, grantResults);
        if (requestCode == PERMISSIONS_REQUEST_READ_CONTACTS) {
            if (grantResults.length > 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED) {
                getPhoneContacts();
            } else {
                Toast.makeText(this, "Permission to read contacts was denied", Toast.LENGTH_SHORT).show();
            }
        }
    }
}

```


## OUTPUT

<img width="1535" height="863" alt="image" src="https://github.com/user-attachments/assets/00e2f187-ec3a-4011-8869-658b24275a99" />

<img width="1535" height="862" alt="image" src="https://github.com/user-attachments/assets/58657207-b596-4212-b58f-06f364003dfb" />


## Result
Thus, a simple Android application to get contact details using a content provider was developed and executed successfully using Android Studio.


## RESULT
Thus a Simple Android Application create your own content providers to get contacts details using Android Studio is developed and executed successfully.
