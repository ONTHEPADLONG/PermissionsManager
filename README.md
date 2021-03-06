## 应用

使用起来的逻辑也比较清晰简单，一共3步：

### 1. 初始化权限管理器

``` java
// 初始化
mPermissionsManager = new PermissionsManager(this) {
    @Override
    public void authorized(int requestCode) {
        Toast.makeText(getApplicationContext(), requestCode + " ： 全部权限通过", Toast.LENGTH_SHORT).show();
    }

    @Override
    public void noAuthorization(int requestCode, String[] lacksPermissions) {
        Toast.makeText(getApplicationContext(), requestCode + " ： 有权限没有通过！需要授权", Toast.LENGTH_SHORT).show();
        for (String permission : lacksPermissions) {
            Log.i(TAG, "noAuthorization: " + permission);
        }
    }
};
```

### 2. 检查权限

``` java
// 要校验的权限
String[] PERMISSIONS = new String[]{Manifest.permission.RECORD_AUDIO, Manifest.permission.CAMERA};
// 检查权限
mPermissionsManager.checkPermissions(0, PERMISSIONS);
```

### 3. 复查权限

用户对权限申请的提示做出选择以后，要重写TargetActivity的`onRequestPermissionsResult`方法来复查权限，检查权限是否通过。

``` java
@Override
public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {
    // 用户做出选择以后复查权限，判断是否通过了权限申请
    mPermissionsManager.recheckPermissions(requestCode, permissions, grantResults);
}
```

进入应用设置页面

最后，权限没有通过，是不能使用的，如果一定要用，一般要提示用户缺少权限，到应用设置页面去把权限打开，再回来使用。
对话框就不写了，进入到应用的设置页面可以直接调用`PermissionsManager`里的`startAppSettings`方法，进入到该应用的设置页面，修改权限

``` java
PermissionsManager.startAppSettings(getApplicationContext());
```