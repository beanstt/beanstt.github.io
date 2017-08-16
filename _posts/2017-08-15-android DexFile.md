---
layout: post
title:  "Android中获取指定包名下的所有类"
date:   2017-08-15 17:00:00 +0800
categories: Android
---
通过DexFile这个类就实现Android中获取指定包名下的所有类。

废话不多说，上代码：
```java
/**
 * Android中获取指定包名下的所有类
 * @param packageName 指定报名 com.xx.xx.xx.adapter
 * @param packageCodePath  activity.getPackageCodePath();
 * @return
 */
List<String> getClassName(String packageName, String packageCodePath){
    List<String>classNameList=new ArrayList<>();
    try {
        DexFile df = new DexFile(packageCodePath);//通过DexFile查找当前的APK中可执行文件
        Enumeration<String> enumeration = df.entries();//获取df中的元素  这里包含了所有可执行的类名 该类名包含了包名+类名的方式
        while (enumeration.hasMoreElements()) {
            String className = enumeration.nextElement();
            if (className.contains(packageName)) {
                classNameList.add(className);
                Log.d(TAG, MessageFormat.format("加载{}",className));
            }
        }
    } catch (IOException e) {
        e.printStackTrace();
    }
    return  classNameList;
}
```
执行后日志
```java
01-03 16:10:03.020 11461-11481/com.xxx.xxx D/ModeAdapterLoader: 加载com.xxx.xxx.xxx.adapter.AdjustAdapter
01-03 16:10:03.020 11461-11481/com.xxx.xxx D/ModeAdapterLoader: 加载com.xxx.xxx.xxx.adapter.RegisterAdapter
01-03 16:10:03.020 11461-11481/com.xxx.xxx D/ModeAdapterLoader: 加载com.xxx.xxx.xxx.adapter.WorkAdapter
01-03 16:10:03.030 11461-11481/com.xxx.xxx D/ModeAdapterLoader: 共加载adapter3个
```