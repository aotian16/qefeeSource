title: android简单学习6.0的动态权限申请
categories: android
tags: permission
date: 2016-07-18 17:39:33

---

<!--head-->

**推荐阅读**

* [**Android 6.0 运行时权限处理完全解析**](http://blog.csdn.net/lmj623565791/article/details/50709663)
* [**Android M 新的运行时权限开发者需要知道的一切**](http://www.jianshu.com/p/e1ab1a179fbb)



android6.0以后一部分危险权限需要动态申请权限，

下面通过一个打电话demo来演示(源于上述推荐阅读)。

<!--more-->

<!--body-->

`MainActivity`

```java
package com.qefee.pj.testpermissionutil;

import android.content.Intent;
import android.content.pm.PackageManager;
import android.net.Uri;
import android.os.Bundle;
import android.support.annotation.NonNull;
import android.support.v7.app.AppCompatActivity;
import android.view.View;
import android.widget.Button;
import android.widget.Toast;

import com.qefee.pj.testpermissionutil.epermission.PermissionUtil;

public class MainActivity extends AppCompatActivity {

    private static final int MY_PERMISSIONS_REQUEST_CALL_PHONE = 1;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        Button submitButton = (Button) findViewById(R.id.submitButton);

        submitButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                testCall();
            }
        });
    }

    public void testCall() {
        if (PermissionUtil.hasCallPhonePerm(this)) { // 是否有权限
            callPhone();
        } else {
            if (PermissionUtil.hasCallPhonePermReason(this)) { // 是否显示申请权限的原因(被拒一次后再次申请时候)
                Toast.makeText(MainActivity.this, "没有授予打电话权限的话,打不了电话哦!", Toast.LENGTH_SHORT).show();

                PermissionUtil.requestCallPhonePerm(this, MY_PERMISSIONS_REQUEST_CALL_PHONE); // 申请权限
            } else {
                PermissionUtil.requestCallPhonePerm(this, MY_PERMISSIONS_REQUEST_CALL_PHONE);
            }
        }
    }

    private void callPhone() {
        Intent intent = new Intent(Intent.ACTION_CALL);
        Uri data = Uri.parse("tel:" + "10086");
        intent.setData(data);
        startActivity(intent);
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {
        super.onRequestPermissionsResult(requestCode, permissions, grantResults);

        switch (requestCode) {
            case MY_PERMISSIONS_REQUEST_CALL_PHONE:
                handleCallPermission(grantResults[0]);
                break;
            default:
                Toast.makeText(MainActivity.this, "unknown request code", Toast.LENGTH_SHORT).show();
        }
    }

    private void handleCallPermission(int grantResult) {
        if (grantResult == PackageManager.PERMISSION_GRANTED) {
            callPhone();
        } else {
            // Permission Denied
            Toast.makeText(MainActivity.this, "Permission Denied", Toast.LENGTH_SHORT).show();
        }
    }
}
```

`PermissionUtil`

```java
package com.qefee.pj.testpermissionutil.epermission;

import android.Manifest;
import android.app.Activity;
import android.content.Context;
import android.content.pm.PackageManager;
import android.support.annotation.NonNull;
import android.support.v4.app.ActivityCompat;
import android.support.v4.content.ContextCompat;

/**
 * PermissionUtil.
 * <ul>
 * <li>date: 16/7/18</li>
 * </ul>
 *
 * @author tongjin
 */
public class PermissionUtil {
    /**
     * 是否拥有权限.
     *
     * @param context    context
     * @param permission permission
     * @return 判断结果
     */
    public static boolean has(@NonNull Context context, @NonNull String permission) {
        return ContextCompat.checkSelfPermission(context, permission)
                == PackageManager.PERMISSION_GRANTED;
    }

    public static boolean hasReason(@NonNull Activity activity,
                                    @NonNull String permission) {
        return ActivityCompat.shouldShowRequestPermissionRationale(activity,
                permission);
    }

    /**
     * 申请权限.
     *
     * @param activity    activity
     * @param permissions permissions
     * @param requestCode requestCode
     */
    public static void request(final @NonNull Activity activity,
                               final @NonNull String[] permissions,
                               final int requestCode) {
        ActivityCompat.requestPermissions(activity,
                permissions,
                requestCode);
    }

    /**
     * 申请权限.
     *
     * @param activity    activity
     * @param permission  permission
     * @param requestCode requestCode
     */
    public static void request(final @NonNull Activity activity,
                               final @NonNull String permission,
                               final int requestCode) {
        String[] permissions = {permission};
        request(activity,
                permissions,
                requestCode);
    }

    /**
     * 是否拥有打电话权限.
     *
     * @param context context
     * @return 判断结果
     */
    public static boolean hasCallPhonePerm(@NonNull Context context) {
        return has(context, Manifest.permission.CALL_PHONE);
    }

    /**
     * 是否有申请打电话权限的原因.
     *
     * @param activity activity
     * @return 判断结果
     */
    public static boolean hasCallPhonePermReason(@NonNull Activity activity) {
        return hasReason(activity,
                Manifest.permission.CALL_PHONE);
    }

    /**
     * 请求打电话权限.
     *
     * @param activity    activity
     * @param requestCode requestCode
     */
    public static void requestCallPhonePerm(final @NonNull Activity activity,
                                            final int requestCode) {
        request(activity,
                Manifest.permission.CALL_PHONE,
                requestCode);
    }
}
```

