```java
public static Activity getCurrentActivity() {

    Class activityThreadClass = null;

    try {

        activityThreadClass = Class.forName("android.app.ActivityThread");

        Object activityThread = activityThreadClass.getMethod("currentActivityThread").invoke(null);


        Field activitiesField = activityThreadClass.getDeclaredField("mActivities");

        activitiesField.setAccessible(true);

        Map activities = (Map) activitiesField.get(activityThread);

        for (Object activityRecord : activities.values()) {

            Class activityRecordClass = activityRecord.getClass();

            Field pausedField = activityRecordClass.getDeclaredField("paused");

            pausedField.setAccessible(true);

            if (!pausedField.getBoolean(activityRecord)) {

                Field activityField = activityRecordClass.getDeclaredField("activity");

                activityField.setAccessible(true);

                Activity activity = (Activity) activityField.get(activityRecord);

                Log.d(TAG, " 当前 activity 是 " + activity);

                return activity;

            }

        }

    } catch (ClassNotFoundException e) {

        e.printStackTrace();

    } catch (NoSuchMethodException e) {

        e.printStackTrace();

    } catch (IllegalAccessException e) {

        e.printStackTrace();

    } catch (Exception e) {

        e.printStackTrace();

    }

    return null;
}
```