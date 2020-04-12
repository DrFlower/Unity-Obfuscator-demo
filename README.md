# Unity-Obfuscator-demo

## 简介
本项目为使用[Unity-Obfuscator][1]插件进行混淆的演示项目，游戏项目为Unity AssetStore上的官方资源[2D Rouguelike][2]。

## 配置详情

### 基本设置
![ObfuscatorConfig][3]
### 黑白名单配置
 > WhiteList-NameObfuscate-NamespaceNameOnly.txt
 >> Completed
 
 > WhiteList-NameObfuscate-ClassNameOnly.txt
 >> Completed|BoardManager  
 >> Completed|Enemy  
 >> Completed|GameManager  
 >> Completed|Loader  
 >> Completed|MovingObject  
 >> Completed|Player  
 >> Completed|SoundManager  
 >> Completed|Wall  
 
 > WhiteList-NameObfuscate-Method.txt
 >> Completed|GameManager|HideLevelImage  
 >> Completed|GameManager|CallbackInitialization  
 >> Completed|Player|Restart  
 
 
 
## 配置说明
 因项目较小，可以逐一排除不能混淆的部分，设置上选择白名单模式，实际项目若部分区域的代码难以全部排除出不能混淆的代码，建议用黑白名单混用模式。  
 项目所有脚本都继承自MonoBehavior，并直接挂在Prefab上，故所有脚本类名都不能做混淆，且所有类的处于Completed命名空间内，Completed命名空间本身名字也不能做混淆处理。
 
 ```c#
 Invoke("HideLevelImage", levelStartDelay);//GameManager line:93
 ```
 Invoke调用不能混淆，故GameManager类中HideLevelImage方法需要放到白名单
 
 ```c#
 Invoke ("Restart", restartLevelDelay);//Player line:182
 ```
 Invoke调用不能混淆，故Player类中Restart方法需要放到白名单
 
  ```c#
  [RuntimeInitializeOnLoadMethod(RuntimeInitializeLoadType.AfterSceneLoad)]
  static public void CallbackInitialization()//GameManager line:60
  {
     //register the callback to be called everytime the scene is loaded
     SceneManager.sceneLoaded += OnSceneLoaded;
  }
 ```
特性基于反射调用，故GameManager类中的CallbackInitialization方法需要放到白名单

<br>黑白名单规则请参考[Unity-Obfuscator][4]的README中的**黑白名单配置-配置规则**</br>

## 混淆结果

混淆前代码：
![混淆前代码][5]

混淆后代码：
![混淆后代码][6]

可见配置了白名单的Completed命名空间名以及各个类名都没变，但除白名单内的3个方法以及Unity生命周期方法外，其他所有类成员名字都发生了变化，混淆结果符合预期。

---
![游戏运行图][7]

游戏正常运行，混淆成功。


  [1]: https://github.com/DrFlower/Unity-Obfuscator "Unity-Obfuscator"
  [2]: https://assetstore.unity.com/packages/essentials/tutorial-projects/2d-roguelike-29825 "2D Rouguelike"
  [3]: https://github.com/DrFlower/Unity-Obfuscator-demo/blob/master/Doc/ObfuscatorConfig.png "ObfuscatorConfig"
  [4]: https://github.com/DrFlower/Unity-Obfuscator "Unity-Obfuscator"
  [5]: https://github.com/DrFlower/Unity-Obfuscator-demo/blob/master/Doc/ILSpyBeforeObfuscate.png "ILSpyBeforeObfuscate"
  [6]: https://github.com/DrFlower/Unity-Obfuscator-demo/blob/master/Doc/ILSpyAfterObfuscate.png "ILSpyAfterObfuscate"
  [7]: https://github.com/DrFlower/Unity-Obfuscator-demo/blob/master/Doc/Game.png "Game"