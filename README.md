## GKNavigationBarViewController --- iOS自定义导航栏-导航栏联动（二）

## 导航栏联动的实现方法
  [iOS自定义导航栏-导航栏联动（一）](http://www.jianshu.com/p/d348e562cb2e)

  [iOS自定义导航栏-导航栏联动（二）](http://www.jianshu.com/p/4eec02b48720)

## 说明：

现在大多数的APP都有导航栏联动效果，即滑动返回的时候导航栏也跟着一起返回，比如：网易新闻，网易云音乐，腾讯视频等等，于是通过查找一些资料及其他库的做法，自己也写了一个框架，可以让每一个控制器都拥有自己的导航栏，可以很方便的改变导航栏的样式等

## 介绍：(本框架的特性)

    * 支持自定义导航栏样式（隐藏、透明等）
    * 支持控制器开关返回手势
    * 支持控制器开关全屏返回手势
    * 支持控制器设置导航栏透明度，可实现渐变效果
    * 完美解决UITableView，UIScrollView滑动手势冲突
    * 可实现push，pop时控制器缩放效果（如：今日头条）
    * 可实现左滑push一个控制器的效果（如：网易新闻）

## Demo中部分截图如下

![今日头条](https://github.com/QuintGao/GKNavigationBarViewController/blob/master/GKNavigationBarViewControllerDemo/%E4%BB%8A%E6%97%A5%E5%A4%B4%E6%9D%A1.gif)

![网易云音乐](https://github.com/QuintGao/GKNavigationBarViewController/blob/master/GKNavigationBarViewControllerDemo/%E7%BD%91%E6%98%93%E4%BA%91%E9%9F%B3%E4%B9%90.gif)

![网易新闻](https://github.com/QuintGao/GKNavigationBarViewController/blob/master/GKNavigationBarViewControllerDemo/%E7%BD%91%E6%98%93%E6%96%B0%E9%97%BB.gif)


## 使用说明

1. 今日头条的实现

UINavigationController作为根控制器，包含一个UITabBarController，UITabBarController中包含以GKNavigationBarViewController为父类的子类

导航栏创建方式
```
GKToutiaoViewController *toutiaoVC = [GKToutiaoViewController new];

// 根控制器是导航控制器，需要缩放
UINavigationController *nav = [UINavigationController rootVC:toutiaoVC translationScale:YES];

```

2. 网易云音乐的实现

UITabBarController作为根控制器，包含带导航栏的以GKNavigationBarViewController为父类的子类

3. 网易新闻的实现

UITabBarController作为根控制器，包含带导航栏的以GKNavigationBarViewController为父类的子类
其中导航栏开启左滑push手势，主要代码如下：
```
// 导航栏开启左滑push
UINavigationController *nav = [UINavigationController rootVC:vc translationScale:NO];
nav.gk_openScrollLeftPush = YES;

// 单个控制器中设置左滑push代理，并实现方法
1. // 设置push的代理
self.gk_pushDelegate = self;

2. 实现代理方法
- (void)pushToNextViewController {
    GKWYNewsCommentViewController *detailVC = [GKWYNewsCommentViewController new];
    detailVC.hidesBottomBarWhenPushed = YES;
    [self.navigationController pushViewController:detailVC animated:YES];
}

```

4. 部分属性介绍

UINavigationController
```
/** 导航栏转场时是否缩放,此属性只能在初始化导航栏的时候有效，在其他地方设置会导致错乱 */
@property (nonatomic, assign, readonly) BOOL gk_translationScale;

/** 是否开启左滑push操作，默认是NO */
@property (nonatomic, assign) BOOL gk_openScrollLeftPush;

```

UIViewController
```
/** 是否禁止当前控制器的滑动返回(包括全屏返回和边缘返回) */
@property (nonatomic, assign) BOOL gk_interactivePopDisabled;

/** 是否禁止当前控制器的全屏滑动返回 */
@property (nonatomic, assign) BOOL gk_fullScreenPopDisabled;

/** 全屏滑动时，滑动区域距离屏幕左边的最大位置，默认是0：表示全屏都可滑动 */
@property (nonatomic, assign) CGFloat gk_popMaxAllowedDistanceToLeftEdge;

/** 设置导航栏的透明度 */
@property (nonatomic, assign) CGFloat gk_navBarAlpha;

/** push代理 */
@property (nonatomic, assign) id<GKViewControllerPushDelegate> gk_pushDelegate;
```

GKNavigationBarViewController
```
/**
自定义导航条
*/
@property (nonatomic, strong, readonly) UINavigationBar *gk_navigationBar;

/**
自定义导航栏栏目
*/
@property (nonatomic, strong, readonly) UINavigationItem *gk_navigationItem;

#pragma mark - 额外的快速设置导航栏的属性
@property (nonatomic, strong) UIColor *gk_navBarTintColor;
@property (nonatomic, strong) UIColor *gk_navBackgroundColor;
@property (nonatomic, strong) UIImage *gk_navBackgroundImage;
@property (nonatomic, strong) UIColor *gk_navTintColor;
@property (nonatomic, strong) UIView *gk_navTitleView;
@property (nonatomic, strong) UIColor *gk_navTitleColor;
@property (nonatomic, strong) UIFont *gk_navTitleFont;

@property (nonatomic, strong) UIBarButtonItem *gk_navLeftBarButtonItem;
@property (nonatomic, strong) NSArray<UIBarButtonItem *> *gk_navLeftBarButtonItems;

@property (nonatomic, strong) UIBarButtonItem *gk_navRightBarButtonItem;
@property (nonatomic, strong) NSArray<UIBarButtonItem *> *gk_navRightBarButtonItems;
```

## Cocoapods(已支持)
pod 'GKNavigationBarViewController'

## 缺陷及不足
* 不能使用系统导航栏的各种属性及方法


## 时间记录

* 2017.7.13 框架实现完成，发布
* 2017.7.14 支持cocoapods



