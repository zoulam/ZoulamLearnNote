# note1

搭建环境

```
npx react-native init CommonApp
npm install @react-navigation/native -S // 路由管理
```

[插件](https://reactnavigation.org/docs/getting-started) 【全是navigation的插件】

```
npm install react-native-reanimated react-native-gesture-handler react-native-screens react-native-safe-area-context @react-native-community/masked-view -S
```

[初始化代码](https://reactnavigation.org/docs/hello-react-navigation) 同时还需要安装下面的依赖

```
npm install @react-navigation/stack -S
```

# 1、登录页面

## ①路由

```javascript
function HomeScreen({ navigation }) {
    return (
        <View>
            <Text>Home Screen</Text>
            <Button
                title="go detail"
        		{/* 页面跳转 */}	
                onPress={() => { navigation.navigate('Details') }}>
                {/*  类组件页面跳转 */}	
                onPress={() => { this.props.navigation.navigate('Details') }}>
            </Button>
        </View>
    );
}

function Nav() {
    return (
        <NavigationContainer>
            {/* headerMode顶部导航栏，如果关闭后撤的按钮也会消失 */}
            {/* initialRouteName初始化路由页面【即首页】*/}
            {/* <Stack.Navigator headerMode="none" initialRouteName="Home"> */}
            <Stack.Navigator initialRouteName="Field" headerMode="none">
                <Stack.Screen name="field" component={Field} />
                <Stack.Screen name="Login" component={Login} />
            </Stack.Navigator>
        </NavigationContainer>
    );
}
```

## ②`<StatusBar/>`

```javascript
// 状态栏设置为透明，translucent设置为true能把背景图片扩展到状态栏
<StatusBar backgroundColor="transparent" translucent={true} />
```

## ③单位 **dp**

> `px to dp` ，为了样式能自适应的函数

```javascript
// 设计稿的宽度 / 元素的宽度 = 手机屏幕宽度 / 手机中元素的宽度
// 目标值是手机中元素的宽度
// 手机屏幕元素宽度 = 手机屏幕宽度 * 元素的宽度/ 设计稿的宽度
import { Dimensions } from 'react-native'
/**
 * @description 屏幕宽度
 */
export const screenWidth = Dimensions.get("window").width
/**
 * @description 屏幕高度
 */
export const screenHeight = Dimensions.get("window").height
/**
 * @description 屏幕dp
 */
export const pxToDp = (elePx) => screenWidth * elePx / 375
```

## ④[react-native-elements](https://reactnativeelements.com/)

```javascript
安装框架 和图表库
npm install react-native-elements -S
npm i react-native-vector-icons -S
react-native link react-native-vector-icons // react-native 的脚手架直接链接图标库【或者使用下面配置】
```

### 修改配置使用图标库

`android/app/build.gradle`

```javascript
// 图标支持
apply from: "../../node_modules/react-native-vector-icons/fonts.gradle"
// 字体支持
project.ext.vectoricons = [
    iconFontNames: [ 'MaterialIcons.ttf', 'EvilIcons.ttf']
]
```

## ⑤`<Input>` 

```JavaScript
                        <Input
                            placeholder="手机号码" 
                            // 字符串最大长度
                            maxLength={11} 
                            // 打开数字菜单
                            keyboardType="phone-pad"
                            value={phoneNumber}
                            inputStyle={{ color: '#333' }}
                            onChange={this.phoneNumberChangeText}
                            // 错误信息提示
                            errorMessage={phoneValidate ? "" : "手机号码格式不正确"}
                            // 提交的钩子
                            onSubmitEditing={this.phoneNumberSubmitEditing}
                            // 插入到左边的图标
                            leftIcon={{ type: 'font-awesome', name: 'phone', color: '#ccc', size: pxToDp(20) }}
                        />
```

## ⑥接口

[接口地址](http://157.122.54.189:9089/swagger.html)

```javascript
安装axios
npm install axios -S
```

## ⑦Loading

> `Teaset` 和 `axios`的拦截 
>
> [teaset仓库](https://github.com/rilyu/teaset)

```
npm install --save teaset
```

 toast:弹窗自定义封装

```JavaScript
import { extendObservable } from 'mobx';
import React from 'react'
import { ActivityIndicator } from 'react-native'
import { Toast, Theme } from 'teaset'

let customKey = null;

Toast.showLoading = (text) => {
    if (customKey) return;
    customKey = Toast.show({
        text,
        icon: <ActivityIndicator size='large' color={Theme.toastIconTintColor} />,
        position: 'center',
        duration: 10000,
    });
}

Toast.hideLoading = () => {
    if (!customKey) return;
    Toast.hide(customKey);
    customKey = null;
}

export default Toast
```

## ⑦渐变按钮

>  [react-native-linear-gradinet](https://www.npmjs.com/package/react-native-linear-gradient)

```
npm install react-native-linear-gradient --save
```

## ⑧验证码输入框

> [react-native-confirmation-code-field](https://www.npmjs.com/package/react-native-confirmation-code-field)

```
npm install react-native-confirmation-code-field -S
```

# 2、用户信息页面

## ①引入svg图片

```
npm install react-native-svg react-native-svg-uri -S
```

去新建一个iconfont项目并拷贝到本地，然后打开示例

>  将资源的 `ttf`和 `xx.json` 存入即可使用

```
复制指定的 outerhtml并粘贴
```

## ②生日选择

```
npm install react-native-datepicker -S
```

## ③定位

### Ⅰ[申请 高度地图的key](https://lbs.amap.com/api/android-location-sdk/guide/create-project/get-key)

> 新建应用两个api，
>
> 一个 `web_api` **注：应该选web服务而不是web端**，一个是 `CommonApp` android的应用

<img src="https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201108190307150.png" alt="web_api" style="zoom:67%;" />

```
填入keyName
服务平台
白名单为空：即全部人都可用【上线再设置】
统一协议
```

[获取sha1](https://lbs.amap.com/faq/android/map-sdk/create-project/43112/?_t=1604833589509)

<img src="https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20201108191223968.png" alt="生成过程" style="zoom: 67%;" />

```
填写两次密钥，然后一直回车
最后面跟图示一样输入y
再一直回车就成功了
```

```bash
cd  .android
调试版sha1
第一次使用会出现不存在的情况
生成sha:
keytool -genkey -v -keystore debug.keystore -alias androiddebugkey -keyalg RSA -validity 10000

keytool -list -v -keystore debug.keystore 
发布版sha1
keytool -genkey -v -keystore apk.keystore -alias privatekeyentry -keyalg RSA -validity 10000
keytool -list -v -keystore apk.keystore
```

### Ⅱ安装

```bash
npm install  react-native-amap-geolocation -S
自动link 
npx react-native link react-native-amap-geolocation
```

### Ⅲ配置文件

**上面的方法无法link的时候才使用**

1. 编辑 `android/settings.gradle`，设置项目路径：

    ```diff
    + include ':react-native-amap-geolocation'
    + project(':react-native-amap-geolocation').projectDir = new File(rootProject.projectDir, '../node_modules/react-native-amap-geolocation/lib/android')
    ```

2. 编辑 `android/app/build.gradle`，新增依赖：

    ```diff
    dependencies {
    +   // 高德地图api支持
    +   implementation project(':react-native-amap-geolocation')
    }
    ```

3. 编辑 `MainApplication.java`：

    ```diff
    + import cn.qiuxiang.react.geolocation.AMapGeolocationPackage;
    
    public class MainApplication extends Application implements ReactApplication {
      @Override
            protected List<ReactPackage> getPackages() {
              @SuppressWarnings("UnnecessaryLocalVariable")
              List<ReactPackage> packages = new PackageList(this).getPackages();
              // Packages that cannot be autolinked yet can be added manually here, for example:
    +         packages.add(new AMapGeolocationPackage());
              return packages;
            }
    }
    ```

    ### 4、编写代码

    [react-native-amap-geolocation](https://github.com/qiuxiang/react-native-amap-geolocation)

    [接口文档](https://qiuxiang.github.io/react-native-amap-geolocation/api/globals.html) 需要翻墙

```JavaScript
import { PermissionsAndroid, Platform } from "react-native";
import { init, Geolocation } from "react-native-amap-geolocation";
import axios from "axios";
class Geo {
    async initGeo() {
        if (Platform.OS === "android") {
            await PermissionsAndroid.request(PermissionsAndroid.PERMISSIONS.ACCESS_COARSE_LOCATION);
        }
        await init({
            // 来自于 高德地图中的第二个应用 android 应用key
            ios: "871e76d5d551780a8140b421791433a0",
            android: "871e76d5d551780a8140b421791433a0"
        });
        return Promise.resolve();
    }
    async getCurrentPosition() {
        return new Promise((resolve, reject) => {
            console.log("开始定位");
            Geolocation.getCurrentPosition(({ coords }) => {
                resolve(coords);
            }, reject);
        })
    }
    async getCityByLocation() {
        // Toast.showLoading("努力获取中")
        const { longitude, latitude } = await this.getCurrentPosition();
        const res = await axios.get("https://restapi.amap.com/v3/geocode/regeo", {
            // key  高德地图中第一个应用的key
            params: { location: `${longitude},${latitude}`, key: "c87d810fdc6b9f78bd80ed37049bda4d", }
        });
        // Toast.hideLoading();
        return Promise.resolve(res.data);
    }
}


export default new Geo();
```

## ④选择地址

[react-native-picker](https://www.npmjs.com/package/react-native-picker)

```
npm install react-native-picker -S
```

## ⑤头像选择

> 1、校验
>
> 2、图片裁切
>
> 3、上传图片
>
> 4、提交个人信息
>
> 5、极光注册登录（一个实时聊天的服务提供商）

### ⑥上传

```
npm install react-native-image-crop-picker -S
```

```JavaScript
import ImagePicker from 'react-native-image-crop-picker'

        const image = await ImagePicker.openPicker({
            width: 300,
            height: 400,
            cropping: true
        })
        console.log(image);
```

### ②审核状态

[teaset overlay](https://github.com/rilyu/teaset/blob/master/docs/cn/Overlay.md)

```javascript
let overlayView = (
  <Overlay.View
    style={{alignItems: 'center', justifyContent: 'center'}}
    modal={true}
    overlayOpacity={0}
    ref={v => this.overlayView = v}
    >
    <View style={{backgroundColor: '#333', padding: 40, borderRadius: 15, alignItems: 'center'}}>
      <Label style={{color: '#eee'}} size='xl' text='Overlay' />
      <View style={{height: 20}} />
      <Button title='Close' onPress={() => this.overlayView && this.overlayView.close()} />
    </View>
  </Overlay.View>
);
Overlay.show(overlayView);
```

### ③上传

>  **关闭调试工具，**不然会被拦截`http post`

# **8、错误**

```
"react-native > use-subscription@1.5.0" has incorrect peer dependency "react@^17.0.0".

react-native-elements 需要 react 17.0.0 以上的版本支持
解决方式：提升react版本或者降低react-native-elements版本
```

### 图片引入需要重新打包

```
npm run android
```

### Input的errorMessage是字符换

```JavaScript
errorMessage={phoneValidate ? "" : "手机号码格式不正确"}
errorMessage={!phoneValidate && "手机号码格式不正确"} // 错误，正确的时候会传入undefined
```

### 无法看到弹窗

```
关闭debugger
```

### 弹出警告

> 开发时一般不用关闭，将下面代码粘贴到根页面的 `index.js` 内容

```
console.ignoredYellowBox = ['Warning: BackAndroid is deprecated. Please use BackHandler instead.','source.uri should not be an empty string','Invalid props.style key'];
 
console.disableYellowBox = true // 关闭全部黄色警告
```

### 虚拟方法不可用

```
java.lang.NullPointerException: Attempt to invoke virtual method '........' on a null object reference
引入错误组件，调整一下代码就好了
```

### 编译出错

使用 `react-native-image-crop-picker`插件时出现，直接用方法二

**app:processDebugResources FAILED**

1、清除缓存

```
cd android
gradlew clean // 清空缓存
```

2、上面方法依旧不能成功

[解决方式](https://github.com/ivpusic/react-native-image-crop-picker/issues/1416)

```diff
dependencies {
        // classpath("com.android.tools.build:gradle:3.5.3")
+ 		classpath("com.android.tools.build:gradle:4.0.1")
        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files
}
```

