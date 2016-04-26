---
layout: post
title: "关于使用CYLTabBarController未登陆时跳转到登陆界面"
description: "处理CYLTabBarController不能调用代理方法的问题"
tags: [CYLTabBarController,界面跳转]
---

**CYLTabBarController**是开源的一款可快速集成的底部栏控制器，只需传入少许参数便可实现功能，并允许个性化自定义。
但是在我需要实现一个功能：**用户未定义时，点击底部“我的”，便需要跳转到登陆界面，方便用户直接登陆。**
因此我觉得理所应当的便实现了**CYLTabBarController**的代理**UITabBarControllerDelegate**，（因为**CYLTabBarController**继承自**UITabBarController**），然后实现代理方法：

``` objectivec 
- (void)tabBarController:(UITabBarController *)tabBarController didSelectViewController:(UIViewController *)viewController {
    if (mineVCtrl) {
        if (viewController == mineVCtrl) {
            [mineVCtrl login];
        }
    }
}
```

但是在运行后发现，无论如何都不会调用该代理方法。最后想来，应该是此类实现机制导致的问题。因此便换一种思路，改作如下代码，顺利完成了功能：

``` objectivec
 - (void) viewWillAppear:(BOOL)animated {
    if(appToken.length) {
    } else {
        BOOL isCurrentOnMine = NO;
        if ([CurrentShowingViewCtrl.navigationController.topViewController isKindOfClass:[MineVCtrl class]]) {
            isCurrentOnMine = YES;
        }
        if (!isCurrentOnMine) {
            [self login];
        }
    }
}
```

