Please create an issue for the module want me to back-ported including:

- The xposed module source code link. This is required.
- Explain what the module does, it's best to include screenshot, and/or thread.


## For Developer

I'm glad you want to back-port some xposed module. Although I have back-ported the [xposed framework](https://github.com/liudongmiao/XposedInstaller) and some xposed module([XPrivacy](http://github.com/liudongmiao/XPrivacy), [XposedAppSettings](https://github.com/liudongmiao/XposedAppSettings)), I don't think I'm experienced.

Here is some steps for me:

0. modify the `minSdkVersion` to 10.

1. check the hook api, if its not work for gingerbread, then please just give up.

   Just look the source for `assets/xposed_init`, and find the Xposed Class, and Check whether it exists on GingerBread or not.

   And if the xposed module hook many class, and some work, some not. Then just add a condition like this:

        if (Build.VERSION.SDK_INT > Build.VERSION_CODES.GINGERBREAD_MR1) {
            // here is original codes ....
        }

2. Check other class. Most of them, the hook will work, however, the settings for the module may use newer api.

   - Check `Fragment` and `ActionBar`. There can be replaced by [android support library](http://developer.android.com/tools/support-library/index.html)

   - Check `PreferenceFragment`. This can be replaced by [preference-fragment-compat](https://github.com/liudongmiao/preference-fragment-compat)

   - Check `getStringSet`. There is no good solution. If you find, please tell me. Currently I just use a trick.

   - Check `ArrayAdapter.addAll`.

   - Check `NotificationBuilder`. GingerBread doesn't support empty Intent. So if there is no intent, just add one.

            NotificationBuilder.setContentIntent(PendingIntent.getActivity(context, 0, new Intent(), 0));

   And I think you may refer to the xposed modules back-ported by me:

   - [XPrivacy](http://github.com/liudongmiao/XPrivacy): Its difficult.
   - [XposedAppSettings](http://github.com/liudongmiao/XposedAppSettings): Its difficult.
   - [xperia_phone_vibrator](https://github.com/liudongmiao/xperia_phone_vibrator/compare/itandy:master...master): It's easy.

*NOTICE*:  `NEVER` forget to test on gingerbread device.
