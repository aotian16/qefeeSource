title: 20180502_从零开始的android持久库room其一
categories: android
tags: room
date: 2018-05-02 22:38:47

---

<!--head-->

[Room](https://developer.android.google.cn/topic/libraries/architecture/room?hl=zh-cn)是android的一个持久化库，SQLite的抽象层，便于使用。推荐用Room替代SQLite。

<!--more-->

<!--body-->

[TOC]

## 引入room库

**在project的gradle中加入google仓库**

```groovy
allprojects {
    repositories {
        jcenter()
        google()
    }
}
```

**在app的gradle中加入room依赖**

其中最后3个是用来支持rxjava2的，方便使用

```groovy
dependencies {
    // Room (use 1.1.0-beta3 for latest beta)
    implementation "android.arch.persistence.room:runtime:1.0.0"
    annotationProcessor "android.arch.persistence.room:compiler:1.0.0"
    // RxJava support for Room (use 1.1.0-beta3 for latest beta)
    implementation "android.arch.persistence.room:rxjava2:1.0.0"
    implementation 'io.reactivex.rxjava2:rxandroid:2.0.2'
    implementation "io.reactivex.rxjava2:rxjava:2.1.13"
}
```

**初始化**

在`Application`中初始化数据库，记得在`AndroidManifest.xml`中配置`App`

```java
public class App extends Application {

    private static App instance;
    private static AppDatabase db;

    @Override
    public void onCreate() {
        super.onCreate();

        instance = this;

        initDb();
    }

    private void initDb() {
        // 这里初始化room
        db = Room.databaseBuilder(this,
                AppDatabase.class, "database-name").build();
    }

    public static App getInstance() {
        return instance;
    }

    public static AppDatabase getDb() {
        return db;
    }
}
```

## Room的3大组件

1. Database 数据库
2. Entity 实体类
3. DAO 数据访问对象

![db](https://developer.android.google.cn/images/training/data-storage/room_architecture.png?hl=zh-cn)

概念不是很难，直接看代码更加方便

```java
@Database(entities = {Student.class}, version = 1) // 数据库注解，必须。entities指定实体类，version指定数据库版本
public abstract class AppDatabase extends RoomDatabase {
    public abstract StudentDAO studentDao();
}
```

```java
@Entity // 实体类注解，必须
public class Student {
    @PrimaryKey(autoGenerate = true) // 主键, autoGenerate表示自增长
    private long uid;

    @ColumnInfo(name = "first_name") // name指定的是表的字段名
    private String firstName;

    @ColumnInfo(name = "last_name")
    private String lastName;

    private int age; // 可以不指定ColumnInfo，那么表的字段名和属性名一致

    // ... get set略
}
```

```java
@Dao
public interface StudentDAO {
    @Query("SELECT * FROM student")
    Single<List<Student>> getAll();

    @Query("SELECT * FROM student WHERE uid IN (:ids)")
    Single<List<Student>> loadAllByIds(int[] ids);

    @Query("SELECT * FROM student WHERE uid = :id")
    Single<Student> findById(int id);

    @Query("SELECT * FROM student WHERE first_name LIKE :first AND "
            + "last_name LIKE :last LIMIT 1")
    Maybe<Student> findByName(String first, String last);

    @Insert
    List<Long> insertAll(Student... students);

    @Update
    void update(Student student);

    @Delete
    void delete(Student student);
}
```

## 数据访问（增删查改）

### Insert

```java
Student s = new Student();
s.setFirstName(firstName);
s.setLastName(lastName);
s.setAge(ageInt);

Callable<List<Long>> callable = () -> App.getDb().studentDao().insertAll(s);
Single.fromCallable(callable)
        .subscribeOn(Schedulers.io())
        .observeOn(AndroidSchedulers.mainThread())
        .subscribe(new SingleObserver<List<Long>>() {
            @Override
            public void onSubscribe(Disposable d) {

            }

            @Override
            public void onSuccess(List<Long> longs) {
                Log.i(TAG, String.format("onSuccess: insert success ids = %s", Arrays.toString(longs.toArray())));
                Snackbar.make(et_first_name, "add student success", Snackbar.LENGTH_SHORT).show();
            }

            @Override
            public void onError(Throwable e) {
                Log.e(TAG, "onError: insert error", e);
                Snackbar.make(et_first_name, "add student fail", Snackbar.LENGTH_SHORT).show();
            }
        });
```

### Delete

```java
final Student s = studentList.get(position);
Action action = () -> App.getDb().studentDao().delete(s);

Completable.fromAction(action)
        .subscribeOn(Schedulers.io())
        .observeOn(AndroidSchedulers.mainThread())
        .subscribe(new CompletableObserver() {
            @Override
            public void onSubscribe(Disposable d) {

            }

            @Override
            public void onComplete() {
                Log.i(TAG, String.format("onComplete: delete student success id = %d", s.getUid()));
                Snackbar.make(rv_list, "delete student success", Snackbar.LENGTH_SHORT).show();
            }

            @Override
            public void onError(Throwable e) {
                Log.e(TAG, "onError: delete student fail", e);
                Snackbar.make(rv_list, "delete student fail", Snackbar.LENGTH_SHORT).show();
            }
        });
            }
        });
```

### Select

```java
App.getDb().studentDao().getAll()
        .subscribeOn(Schedulers.io())
        .observeOn(AndroidSchedulers.mainThread())
        .subscribe(new SingleObserver<List<Student>>() {
            @Override
            public void onSubscribe(Disposable d) {

            }

            @Override
            public void onSuccess(List<Student> students) {
                Log.i(TAG, String.format("onSuccess: students.size = %d", students.size()));
                studentList.addAll(students);
                adapter.notifyDataSetChanged();
            }

            @Override
            public void onError(Throwable e) {
                Log.e(TAG, "onError: get all student error", e);
                Snackbar.make(rv_list, "select student fail", Snackbar.LENGTH_SHORT).show();
            }
        });
```

### Update

```java
Action action = () -> {
    Student s = new Student();
    s.setUid(idInt);
    if (!TextUtils.isEmpty(firstName)) {
        s.setFirstName(firstName);
    }
    if (!TextUtils.isEmpty(lastName)) {
        s.setLastName(lastName);
    }
    if (!TextUtils.isEmpty(age)) {
        s.setAge(ageInt);
    }
    App.getDb().studentDao().update(s);
};

Completable.fromAction(action)
        .subscribeOn(Schedulers.io())
        .observeOn(AndroidSchedulers.mainThread())
        .subscribe(new CompletableObserver() {
            @Override
            public void onSubscribe(Disposable d) {

            }

            @Override
            public void onComplete() {
                Log.i(TAG, "onComplete: update student success");
                Snackbar.make(et_id, "update student success", Snackbar.LENGTH_SHORT).show();
            }

            @Override
            public void onError(Throwable e) {
                Log.e(TAG, "onError: update student fail", e);
                Snackbar.make(et_id, "update student fail", Snackbar.LENGTH_SHORT).show();
            }
        });
```

到这里，我们已经可以简单的增删查改数据库了。

## 注意事项

遇到下面的这个警告，说的是找不到地方导出schema

```shell
警告: Schema export directory is not provided to the annotation processor so we cannot export the schema. You can either provide `room.schemaLocation` annotation processor argument OR set exportSchema to false.
```

**解决方案**

在app的gradle文件中添加

```groovy
android {
    // 略
    defaultConfig {
        // 略

        // for room, 这里添加room的schemas导出地点
        javaCompileOptions {
            annotationProcessorOptions {
                arguments = ["room.schemaLocation": "$projectDir/schemas".toString()]
            }
        }
    }
    // 略
}
```

## 源码地址

[AndroidRoomStudy](https://github.com/aotian16/AndroidRoomStudy)

## 参考文章

[android room](https://developer.android.google.cn/training/data-storage/room/?hl=zh-cn)