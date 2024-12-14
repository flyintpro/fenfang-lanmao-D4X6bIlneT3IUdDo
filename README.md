
随着社交媒体平台的快速发展，用户对内容分享的需求不断增加，社交分享按钮在数字营销和搜索引擎优化（SEO）策略中也成为提升网站流量和内容曝光率的重要工具。此外，图片、视频和文件的传输在协同工作和朋友共享中的使用也越来越频繁。


HarmonyOS SDK[分享服务](https://github.com "分享服务")（Share Kit）为应用提供文本、图片、视频等内容跨应用分享能力，用于将内容发送到其他应用。


应用把需要分享的内容和预览样式配置给分享服务，分享服务将根据分享的数据类型、数量等信息构建分享面板，为用户提供内容预览、推荐分享联系人、关联应用及操作界面，便于用户快速选择分享应用或操作，将内容分发到目标应用。


### 场景介绍


**智慧办公**


用户可以快速将文件和文档从一个设备无缝传输到另一个设备。此外，系统提供的多种能力如打印，中转站等能力为处理文件提供了极大便利，优化了文档管理流程，使日常办公更加高效。


**影音娱乐**


用户可以轻松将图片、视频等内容分享至他人或设备，如智慧屏。此外，推荐功能能够根据用户的行为习惯推荐联系人，提升个性化分享体验。


![image](https://img2024.cnblogs.com/blog/2396482/202412/2396482-20241213173650531-1453973880.png)


手机分享面板效果图


![image](https://img2024.cnblogs.com/blog/2396482/202412/2396482-20241213173700629-1289239784.png)


2in1设备上Popup形式效果图


### 功能优势


**1\.无缝跨设备连接**


HarmonyOS SDK分享服务能够在不同设备之间实现无缝连接，包括智能手机、平板电脑、笔记本电脑以及其他兼容的智能设备。用户只需简单的操作，即可在多个设备间快速传输文件，不受设备类型和品牌的限制。


**2\.快速传输速度**


相比传统的蓝牙传输，HarmonyOS SDK分享服务利用更高效的传输协议，显著提升了文件传输速度。大文件的传输时间大幅缩短，让用户在处理繁重任务时更加高效。


**3\.智能识别和推荐**


HarmonyOS SDK分享服务采用智能识别技术，能够自动识别接收设备，并根据内容类型推荐适合的分享方式。例如，当你尝试分享一张照片时，系统会自动选择最佳的接收设备并提供优化的显示选项。


**4\.隐私保护和安全**


HarmonyOS SDK分享服务注重用户隐私和数据安全，所有传输过程均经过加密处理，确保数据在传输过程中的安全性。此外，用户可以根据需求设定分享权限，进一步保护个人隐私。


**5\.便捷的操作体验**


操作简便是HarmonyOS SDK分享服务的一大特点。用户只需通过简单的拖拽、点击或选择，即可完成文件的分享，不需要复杂的设置或繁琐的步骤。这种直观的操作方式极大地提升了用户体验。


**6\.跨平台兼容性**


不仅支持HarmonyOS设备间的分享，还能与其他主流操作系统如Android和iOS进行跨平台文件共享。无论你使用哪种操作系统，都可以享受到HarmonyOS SDK分享服务带来的便利。


**7\.支持多种文件格式**


HarmonyOS SDK分享服务支持多种文件格式的传输，包括图片、视频、文档等。用户无需担心文件格式不兼容的问题，可以自由分享各种类型的文件，满足不同场景下的需求。


### 开发步骤


**一、宿主应用发起分享**


1\)手机应用发起系统分享


1\.导入相关模块。



```
 import { common } from '@kit.AbilityKit';
 import { systemShare } from '@kit.ShareKit';
 import { uniformTypeDescriptor as utd } from '@kit.ArkData';

```

2\.构造分享数据，可添加多条分享记录。



```
// 构造ShareData，需配置一条有效数据信息
let data: systemShare.SharedData = new systemShare.SharedData({
  utd: utd.UniformDataType.PLAIN_TEXT,
  content: 'Hello HarmonyOS'
});
// 添加多条记录
data.addRecord({
  utd: utd.UniformDataType.PNG,
  uri: 'file://.../test.png' 
});

```

3\.启动分享面板。



```
// 构建ShareController
let controller: systemShare.ShareController = new systemShare.ShareController(data);
// 获取UIAbility上下文对象
let context: common.UIAbilityContext = getContext(this) as common.UIAbilityContext;
// 注册分享面板关闭监听
 controller.on('dismiss', () => {
   console.info('Share panel closed');
   // 分享结束，可处理其他业务。
 });
// 进行分享面板显示
controller.show(context, {
  previewMode: systemShare.SharePreviewMode.DETAIL,
  selectionMode: systemShare.SelectionMode.SINGLE
});

```

2\)2in1应用发起系统分享


1\.导入相关模块。



```
import { common } from '@kit.AbilityKit';
import { systemShare } from '@kit.ShareKit';
import { uniformTypeDescriptor as utd } from '@kit.ArkData';

```

2\.构造分享数据，可添加多条分享记录。



```
// 构造ShareData，需配置一条有效数据信息
let data: systemShare.SharedData = new systemShare.SharedData({
  utd: utd.UniformDataType.PLAIN_TEXT,
  content: 'Hello HarmonyOS'
});
// 额外再添加一条记录
data.addRecord({
  utd: utd.UniformDataType.PNG,
  uri: 'file://.../test.png'
});

```

3\.启动分享面板，必须要配置分享面板显示的位置信息或关联的组件ID，面板将以Popup形式展示。



```
// 构建ShareController
let controller: systemShare.ShareController = new systemShare.ShareController(data);
// 获取UIAbility上下文对象
let context: common.UIAbilityContext = getContext(this) as common.UIAbilityContext;
// 注册分享面板关闭监听
controller.on('dismiss', () => {
  console.info('Share panel closed');
  // 分享结束，可处理其他业务。
});

// 进行分享面板显示
// 方法一：配置分享面板关联的控件ID 
controller.show(context, {
  anchor: 'shareButtonId'
});
// 方法二：配置分享面板显示的坐标
controller.show(context, {
  anchor: {
    // 必选， 锚点坐标
    windowOffset: {x: 100, y: 100},
    // 可选，组件的宽高，配置后会综合计算组件的大小
    size: {width: 0, height: 0}
  }
});

```

**二、目标应用处理分享内容**


1\)应用内处理分享内容


1\.导入相关模块。



```
import { AbilityConstant, UIAbility, Want } from '@kit.AbilityKit';
import { window } from '@kit.ArkUI';
import { systemShare } from '@kit.ShareKit';
import { BusinessError } from '@kit.BasicServicesKit';

```

2\.目标应用可实现UIAbility，并从want中获取分享数据。



```
export default class TestUIAbility extends UIAbility {
  onCreate(want: Want, launchParam: AbilityConstant.LaunchParam): void {
    systemShare.getSharedData(want)
      .then((data: systemShare.SharedData) => {
        data.getRecords().forEach((record: systemShare.SharedRecord) => {
          // 处理分享数据
        });
      })
      .catch((error: BusinessError) => {
        console.error(`Failed to getSharedData. Code: ${error.code}, message: ${error.message}`);
        this.context.terminateSelf();
      });
  }
  onWindowStageCreate(windowStage: window.WindowStage): void {
    // Main window is created, set main page for this ability
    windowStage.loadContent('pages/Index', (err) => {
      if (err.code) {
        console.error('Failed to load the content. Cause: ' + JSON.stringify(err) ?? '');
        return;
      }
      console.info('Succeeded in loading the content.');
    });
  }
}

```

3\.构建完UIAbility，需要在应用配置文件（src/main/module.json5）的skills配置中注册。配置actions为ohos.want.action.sendData；uris需穷举所有支持的数据类型。



```
"abilities": [
  {
    "name": "TestUIAbility",
    "srcEntry": "./ets/entryability/TestUIAbility.ets",
    "description": "$string:EntryAbility_desc",
    "icon": "$media:layered_image",
    "label": "$string:EntryAbility_label",
    "startWindowIcon": "$media:startIcon",
    "startWindowBackground": "$color:start_window_background",
    "exported": true,
    "skills": [
      {
        "actions": [
          "ohos.want.action.sendData"
        ],
        // 目标应用在配置支持接收的数据类型时，需穷举支持的UTD，比如：支持全部图片类型，可声明：general.image
        // maxFileSupported 支持的最大数量 不填默认为0
        "uris": [
          {
            "scheme": "file",
            "utd": "general.text",
            "maxFileSupported": 1
          },
          {
            "scheme": "file",
            "utd": "general.png",
            "maxFileSupported": 1
          },
          {
            "scheme": "file",
            "utd": "general.jpeg",
            "maxFileSupported": 1
          }        
        ]
      }
    ]
  }
]

```

2\)二级面板处理分享内容


1\.导入相关模块。



```
import { Want, ShareExtensionAbility, UIExtensionContentSession } from '@kit.AbilityKit';
import { systemShare } from '@kit.ShareKit';
import { BusinessError } from '@kit.BasicServicesKit';

```

2\.目标应用可以使用ShareExtensionAbility为基类构建分享能力Ability。在Ability被系统启动时，Ability会收到want数据，并从want中获取分享数据。



```
export default class TestShareAbility extends ShareExtensionAbility {
  onSessionCreate(want: Want, session: UIExtensionContentSession) {
    systemShare.getSharedData(want)
      .then((data: systemShare.SharedData) => {
        data.getRecords().forEach((record: systemShare.SharedRecord) => {        
          // 处理分享数据
        });
        session.loadContent('pages/Index');
      })
      .catch((error: BusinessError) => {
        console.error(`Failed to getSharedData. Code: ${error.code}, message: ${error.message}`);
        session.terminateSelf();
      });
  }
}

```

3\.构建完分享能力Ability，需要在应用配置文件（src/main/module.json5）的skills配置中注册。配置actions为ohos.want.action.sendData，并且uris需穷举所有支持的数据类型。



```
"extensionAbilities": [  
  {
    "name": "TestShareAbility",
    "srcEntry": "./ets/abilities/TestShareAbility.ts",
    "type": "share", // 支持分享数据处理
    "description": "xxx",
    "exported": true,
    "label": "$string:xx_label",
    "icon": "$media:icon",
    "skills": [
      {
        "actions": [
          "ohos.want.action.sendData"
        ],
        // 目标应用在配置支持接收的数据类型时，需穷举支持的UTD，比如：支持全部图片类型，可声明：general.image
        // maxFileSupported 支持的最大数量 不填默认为0
        "uris": [
          {
            "scheme": "file",
            "utd": "general.text",
            "maxFileSupported": 1
          },
          {
            "scheme": "file",
            "utd": "general.png",
            "maxFileSupported": 1
          },
          {
            "scheme": "file",
            "utd": "general.jpeg",
            "maxFileSupported": 1
          }
        ]
      }
    ]
  }
]

```

3\)二级面板关闭分享面板


1\.导入相关模块。



```
import { ShareExtensionAbility, UIExtensionContentSession, Want } from '@kit.AbilityKit';
import { systemShare } from '@kit.ShareKit';

```

2\.目标应用可以通过terminateSelfWithResult接口，设置resultCode值为systemShare.ShareAbilityResultCode.CLOSE，以关闭分享面板。



```
export default class TestShareAbility extends ShareExtensionAbility {
  async onSessionCreate(want: Want, session: UIExtensionContentSession) {
    session.terminateSelfWithResult({
      resultCode: systemShare.ShareAbilityResultCode.CLOSE
    });
  }
}

```

**了解更多详情\>\>**


访问[分享服务联盟官网](https://github.com "分享服务联盟官网"):[slower加速器](https://chundaotian.com)


获取[分享服务开发指导文档](https://github.com "分享服务开发指导文档")


