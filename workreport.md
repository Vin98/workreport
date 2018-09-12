## 1. 任务描述      
我在58集团安居客实习，主要从事iOS开发，由于实习时间长，完成任务较多，包括但不限于以下几个方面：

- 学习xcode使用，熟悉iOS开发 
- 学习cocoapods集成三方库的方法  
- 学习git版本控制系统的使用，熟悉团队合作的机制  
- 熟悉公司代码，进行实际的小规模个人开发  
- 参与公司项目开发，实现产品需求

## 2. 技术性工作
### 2.1 cocoapods集成第三方库   

- 因为cocoapods由ruby编写，因此少不了ruby环境，首先打开终端查看当前ruby版本，版本大于等于2.2.2即可，否则[点击这里](https://www.jianshu.com/p/f43b5964f582)查看详细信息
 
  ```
  ruby -v
  
  //我的版本如下
  ruby 2.3.7p456 (2018-03-28 revision 63024) [universal.x86_64-darwin17]
  ```
- 因为ruby源国内被墙，因此将ruby源更换为ruby-china（淘宝源也已停止维护）

  ```
  gem sources --remove https://rubygems.org/
  gem sources --add https://gems.ruby-china.org/
  ```
  
- 验证ruby源是且仅是ruby-china,执行如下命令查看
  
  ```
  gem sources -l
  //得到如下结果即为正确
  
  *** CURRENT SOURCES ***

  https://gems.ruby-china.org/
  ```
- 开始安装cocoapods
 
  ```
  sudo gem install -n /usr/local/bin cocoapods
  ```
- 安装本地库

  ```
  pod setup
  ```
- 执行结果,等待过程较为漫长

  ```
  Setting up CocoaPods master repo
  $ /usr/bin/git clone https://github.com/CocoaPods/Specs.git master --progress
  Cloning into 'master'...
  remote: Counting objects: 1879515, done.        
  remote: Compressing objects: 100% (321/321), done.        
  Receiving objects:  21% (404525/1879515), 73.70 MiB | 22.00 KiB/
  ```
  
- 完成后可搜索一个三方库检验是否成功
 
  ```
  pod search masonry
  ```
  
- cocoapods具体在工程中的使用

  在xcode工程目录下，创建Podfile文件：
  
  ```
  pod init
  ```
  在文件目录生成一个Podfile文件，例如要添加masonry,Realm库,在文件中编写如下代码
  
  ``` ruby
  #platform :ios, '9.0'
  
  target 'projectname' do
        pod 'Masonry', '~> 1.1.0'
        pod 'Realm',    '3.3.0'
  end
  ```
  
  > 更新cocoapods版本：
  `gem install cocapods`
  
  > 更新本地库：
  `pod repo update`
  
### 2.2 版本控制系统git的使用
- git简介
  
  git是目前世界上那个最先进的版本控制系统，git是分布式的版本控制系统，文件不需要保存再中央服务器，这就意味着你完全可以在本地使用git带来的便利。git可以记录文件的每次改动，并可以让你随时会退到任何版本。不仅仅是这样，git的更大便利体现在多人协作上，大家可以统一git仓库上进行开发，大家在本地进行开发，然后将更新提交到远程仓库中，远程仓库默认只有一个master分支，而master分支的文件相对比较重要，如果提交导致了master的损坏，后果将不堪设想，因此我们在工作中常常使用如下方法使用git：
  
  1. 将共有仓库(以learngit为例)fork到个人的仓库中-在GitHub的网站上点击fork即可
  2. 将本地的ssh key添加到个人远程仓库中
  3. 将个人远程仓库克隆到本地`git clone https://github.com/Vin98/learngit`
  4. 将远程仓库与本地仓库关联`git remote add upstream https://github.com/Vin98/learngit`
  5. 使用`git remote -v`可查看当前关联的远程库
  6. 拉取远程仓库中有的本地没有的文件`git fetch upstream`
  7. 假设共有仓库上有一个分支dev_1.0，切换到这个分支`git checkout upstream/dev_1.0`
  8. 创建自己的分支`git checkout -b lijiale_dev_1.0`
  9. 更新pod库`pod update`
  10. 提交文件修改时`git add .`将所有更新保存到暂存区，`git commit -m "更新的内容"`将暂存区中的文件提交到本地仓库
  11. 拉取远程有的而本地没有的文件，此步骤主要是保证本地为最新文件，因为别人也有可能进行的提交`git fetch upstream`，将拉取到的新文件与本地文件合并`git rebase upstream/dev_1.0`,若成功则提交文件`git push origin lijiale_dev_1.0`，若不成功则多为你的更新与别人的更新发生了冲突，Xcode中会用`c`标识出冲突的文件，解决冲突之后再`git add .`然后`git rebase --continue`，再进行提交即可
  
  > git中常用的一些操作:   
  
  > 初始化仓库`git init`    
  > 查看当前仓库状态`git status`   
  > 查看具体修改内容 `git diff filename`   
  > 回退到上一个版本：`git reset --hard HEAD^`
    或者使用具体的commit-id `git reset --hard commit_id`    
  > 查看工作区和版本库最新版本的区别`git diff HEAD -- filename`   
  > 丢弃工作区的修改：`git checkout -- filename` 即将filename文件在工作区的修	 改全部撤销，当没有被放到暂存区时回退到版本库中的版本,当放到暂存区后会回退到暂存	 区中的版本    
  > 撤销暂存区的修改，将暂存区的修改回退到工作区  `git reset HEAD filename`   
  > 当在工作区删除文件时（假设文件已在版本库中）   
	 第一种情况：将文件从版本库中也删除`git rm/add filename`
              `git commit -m "log"`   
 	 第二种情况：恢复误删的文件也就是撤销工作区的修改`git checkout -- filename`
 	 
 	 
### 2.3 个人小规模开发     
- 项目功能：实现一个房源信息列表，并可以根据价格，区域板块进行筛选，收藏功能,分享功能  

- 项目设计,用AppDelegate代理监听系统事件并设置根视图，列表及收藏页面采用`UITableview`，并重写`UITableviewCell`，写房源model `HLCHouseItem`,并使用一个单例`HLCHouseItemStore`保存房源信息，具体实现如下,由于代码较长，只贴.h文件部分

1. AppDelegate:设置APP代理，监听系统事件   
  
  ```objc
  - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    // Override point for customization after application launch.
    	self.window = [[UIWindow alloc] initWithFrame:[UIScreen mainScreen].bounds];
    	HLCListViewController *listViewController = [[HLCListViewController alloc] init];
    	HLCMyCollectionViewController *mycollection = [[HLCMyCollectionViewController alloc] init];
    	UINavigationController *listNavController = [[UINavigationController alloc] initWithRootViewController:listViewController];
    	UINavigationController *collectionNavController = [[UINavigationController alloc] initWithRootViewController:mycollection];
    	UITabBarController *tabBarController = [[UITabBarController alloc] init];
    	tabBarController.viewControllers = @[listNavController,collectionNavController];
    	self.window.rootViewController = tabBarController;
    	self.window.backgroundColor = [UIColor whiteColor];
    	[self.window makeKeyAndVisible];
    	return YES;
}
  ```
  
2. 房源信息model`HLCHouseItem` 类  
  
  ```objc
  //
//  HLCHouseItem.h
//  
//
//  Created by lijiale on 2018/7/30.
//


	#import <Foundation/Foundation.h>
@interface HLCHouseItem : NSObject
@property (nonatomic) NSUInteger houseID;
@property (nonatomic, copy)NSString *houseName;
@property (nonatomic, copy)NSString *houseArea;
@property (nonatomic, copy)NSString *houseBlock;
@property (nonatomic, copy)NSString *houseImage;
@property (nonatomic, copy)NSString *housePrice;
@property (nonatomic, copy)NSString *houseAddress;
@property (nonatomic)BOOL isCollected;

	- (instancetype)initWithHouseID:(NSUInteger)ID houseName:(NSString *)name houseArea:(NSString *)area houseBlock:(NSString*)block housePrice:(NSString *)price houseAddress:(NSString *)address houseImage:(NSString *)image;
@end

  ```
  
3. 房源管理单例`HLCHouseItemStore`类   

	```objc
//
//  HLCHouseItemStore.h
//  HouseList&Collection
//
//  Created by lijiale on 2018/7/30.
//  Copyright © 2018年 lijiale. All rights reserved.
//

	#import <Foundation/Foundation.h>
typedef NS_ENUM(NSInteger, priceSort){
    	priceSortByDefault = 0,
    	priceSortByAsc = 1,
    	priceSortByDesc = 2
};
@class HLCHouseItem;
@interface HLCHouseItemStore : NSObject
@property (nonatomic, strong) NSArray *HouseList;
@property (nonatomic, strong) NSArray *collectionList;
@property (nonatomic, copy) NSString *currentSelectedArea;
@property (nonatomic, copy) NSString *currebtSelectedBlock;
@property (nonatomic) priceSort currentPriceSort;
@property (nonatomic)NSUInteger houseCount;
+ (instancetype)sharedStore;
- (HLCHouseItem *)addWithHouseID:(NSUInteger)ID houseName:(NSString *)name houseArea:(NSString *)area houseBlock:(NSString *)block housePrice:(NSString *)price houseAddress:(NSString *)address houseImage:(NSString *)image isCollected:(BOOL)iscolleted;
- (void)setCurrentSelectedArea:(NSString *)area Block:(NSString *)block;
- (void)setPriceSort:(priceSort)price;
- (void)clearFliter;
- (void)fliterHouseList;
- (void)sortHouseList;
- (BOOL)updateList;
- (void)changeCollectStateWithHouseID:(NSInteger)ID;
@end

	```
4. 房源列表页Controller
	
	```objc
	//
//  HLCListViewController.m
//  HouseList&Collection
//
//  Created by lijiale on 2018/7/26.
//  Copyright © 2018年 lijiale. All rights reserved.
//
	#define MAS_SHORTHAND_GLOBALS
#import "HLCListViewController.h"
#import "HLCHouseTableViewCell.h"
#import "DOPDropDownMenu.h"
#import "HLCHouseItem.h"
#import "HLCHouseItemStore.h"
#import <SDWebImage/UIImageView+WebCache.h>

	@interface HLCListViewController ()<DOPDropDownMenuDataSource, DOPDropDownMenuDelegate>
@property (nonatomic, strong) NSArray *areas;
@property (nonatomic, strong) NSArray *blocks;
@property (nonatomic, strong) NSArray *sorts;
@property (nonatomic, strong) DOPDropDownMenu *menu;
@end

	@implementation HLCListViewController

	- (instancetype)init{
    self = [super init];
    self.tableView.separatorStyle = UITableViewCellSeparatorStyleNone;
    if(self){
        self.tabBarItem.title = @"房源";
        UIImage *house = [[UIImage imageNamed:@"house"] imageWithRenderingMode:UIImageRenderingModeAlwaysOriginal];
        self.tabBarItem.image = house;
        UIImage *house_selected = [[UIImage imageNamed:@"house_selected"] imageWithRenderingMode:UIImageRenderingModeAlwaysOriginal];
        self.tabBarItem.selectedImage = house_selected;
    }
    return self;
}
	#pragma mark - view life cycle
	- (void)viewDidLoad {
    [super viewDidLoad];
    //[NSThread sleepForTimeInterval:(1.0)];
    self.navigationItem.title = @"房源列表";
    //设置UIView从导航栏下面开始
    self.navigationController.navigationBar.translucent = NO;
    self.navigationItem.rightBarButtonItem = [[UIBarButtonItem alloc]initWithTitle:@"重置" style:UIBarButtonItemStylePlain target:self action:@selector(menuReloadData)];
    DOPDropDownMenu *menu = [[DOPDropDownMenu alloc] initWithOrigin:CGPointMake(0, 0) andHeight:44];
    menu.delegate = self;
    menu.dataSource = self;
    _menu = menu;
    [self.tableView registerClass:[HLCHouseTableViewCell  class] forCellReuseIdentifier:@"HouseCell"];
    
	}
	- (void)viewWillAppear:(BOOL)animated{
    [super viewWillAppear:YES];
    [self.tableView reloadData];
}
	#pragma mark - action
- (void)menuReloadData
{
    [_menu reloadData];
    [[HLCHouseItemStore sharedStore] clearFliter];
    [[HLCHouseItemStore sharedStore] fliterHouseList];
    [[HLCHouseItemStore sharedStore] sortHouseList];
    [self.tableView reloadData];
}
	#pragma mark - dropDownMenu
- (NSInteger)numberOfColumnsInMenu:(DOPDropDownMenu *)menu{
    return 2;
}
- (NSInteger)menu:(DOPDropDownMenu *)menu numberOfRowsInColumn:(NSInteger)column{
    if(column == 0){
        return self.blocks.count;
    }
    else{
        return self.sorts.count;
    }
}
-(NSString *)menu:(DOPDropDownMenu *)menu titleForRowAtIndexPath:(DOPIndexPath *)indexPath{
    if(indexPath.column == 0){
        return self.areas[indexPath.row];
    }
    else{
        return self.sorts[indexPath.row];
    }
}
- (NSInteger)menu:(DOPDropDownMenu *)menu numberOfItemsInRow:(NSInteger)row column:(NSInteger)column{
    if(column == 0){
        if(row == 0){
            return 0;
        }
        NSArray *items = self.blocks[row];
        return items.count;
    }
    else{
        return 0;
    }
}
- (NSString *)menu:(DOPDropDownMenu *)menu titleForItemsInRowAtIndexPath:(DOPIndexPath *)indexPath{
    if(indexPath.column == 0){
        NSArray *items = self.blocks[indexPath.row];
        return items[indexPath.item];
    }
    return 0;
}
- (void)menu:(DOPDropDownMenu *)menu didSelectRowAtIndexPath:(DOPIndexPath *)indexPath
{
    HLCHouseItemStore *store = [HLCHouseItemStore sharedStore];
    if(indexPath.column == 0){
        if (indexPath.item >= 0) {
            if([store.currentSelectedArea isEqualToString:self.areas[indexPath.row]] &&
               [store.currebtSelectedBlock isEqualToString:self.blocks[indexPath.row][indexPath.item]]){
                return;
            }
            [store setCurrentSelectedArea:self.areas[indexPath.row] Block:self.blocks[indexPath.row][indexPath.item]];
        }else {
            if(indexPath.row == 0){
                if([store.currentSelectedArea isEqualToString:@"不限"] &&
                   [store.currebtSelectedBlock isEqualToString:@"不限"]){
                    return;
                }
                [store setCurrentSelectedArea:@"不限" Block:@"不限"];
            }
        }
        [store fliterHouseList];
    }
    else if(indexPath.column == 1){
        priceSort price = indexPath.row;
        [store setPriceSort:price];
    }
    [store sortHouseList];
    [self.tableView reloadData];
}
	#pragma mark - cell
- (UIView *)tableView:(UITableView *)tableView viewForHeaderInSection:(NSInteger)section{
    return self.menu;
}
- (CGFloat)tableView:(UITableView *)tableView heightForHeaderInSection:(NSInteger)section{
    return 44;
}
- (void)didReceiveMemoryWarning {
    [super didReceiveMemoryWarning];
    // Dispose of any resources that can be recreated.
}
- (NSInteger)numberOfSectionsInTableView:(UITableView *)tableView{
    return 1;
}
- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section{
    NSLog(@"count of houselist is %lu",[[[HLCHouseItemStore sharedStore] HouseList] count]);
    return [[[HLCHouseItemStore sharedStore] HouseList] count];
}
- (CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath{
    return 120;
}
- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath{
    HLCHouseTableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:@"HouseCell" forIndexPath:indexPath];
    if(!cell){
        cell = [[HLCHouseTableViewCell alloc] initWithStyle:UITableViewCellStyleSubtitle reuseIdentifier:@"HouseCell"];
    }
    NSArray *houseList = [HLCHouseItemStore sharedStore].HouseList;
    HLCHouseItem *house = houseList[indexPath.row];
    cell.houseName.text = house.houseName;
    cell.housePrice.text = [house.housePrice stringByAppendingString:@"万"];
    cell.houseBlock.text = house.houseBlock;
    cell.houseCommunity.text = house.houseAddress;  
    //cell.houseLoaction.text = [house.houseArea stringByAppendingString:house.houseBlock];
    cell.houseCollection.tag = house.houseID;
    NSURL *imageUrl = [NSURL URLWithString:house.houseImage];
    //imageUrl = [NSURL URLWithString:@"https://pic1.ajkimg.com/display/anjuke/600675-%E6%88%91%E7%88%B1%E6%88%91%E5%AE%B6/ead933be14ffc87240b9fc4d1c664763-599x450.jpg?t=1"];
    [cell.houseImage sd_setImageWithURL:imageUrl completed:^(UIImage * _Nullable image, NSError * _Nullable error, SDImageCacheType cacheType, NSURL * _Nullable imageURL) {
        NSLog(@"load image complete");
    }];
    [cell.houseCollection addTarget:self action:@selector(collectHouse:) forControlEvents:UIControlEventTouchUpInside];
    if(house.isCollected){
        [cell.houseCollection setImage:[UIImage imageNamed:@"star_collected"] forState:UIControlStateNormal];
    }
    else{
        [cell.houseCollection setImage:[UIImage imageNamed:@"star_notcollect"] forState:UIControlStateNormal];
    }
//    if(indexPath.row == houseList.count - 2){
//        if([[HLCHouseItemStore sharedStore] updateList] == YES){
//            [self.tableView reloadData];
//        }
//    }
    return cell;
}
- (void)collectHouse:(UIButton *)sender{
    UIAlertController *alertController = [UIAlertController alertControllerWithTitle:@"👌" message:@"收藏成功" preferredStyle:UIAlertControllerStyleAlert];
    UIAlertAction *okAction = [UIAlertAction actionWithTitle:@"确定" style:UIAlertActionStyleCancel handler:nil];
    NSArray *houselist = [[HLCHouseItemStore sharedStore] HouseList];
    for (HLCHouseItem *house in houselist) {
        if(house.houseID == sender.tag){
            if(house.isCollected == YES){
                [sender setImage:[UIImage imageNamed:@"star_notcollect"] forState:UIControlStateNormal];
                [alertController setMessage:@"取消收藏成功"];
            }
            else{
                [sender setImage:[UIImage imageNamed:@"star_collected"] forState:UIControlStateNormal];
                [alertController setMessage:@"收藏成功"];
            }
            break;
        }
    }
    [alertController addAction:okAction];
    [self presentViewController:alertController animated:YES completion:nil];
    [[HLCHouseItemStore sharedStore] changeCollectStateWithHouseID:sender.tag];
}
- (NSArray *)sorts{
    _sorts = @[@"默认价格",@"价格升序",@"价格降序"];
    return _sorts;
}
	```
5. 收藏页Controller

	```objc
	//
//  HLCMyCollectionViewController.m
//  HouseList&Collection
//
//  Created by lijiale on 2018/7/28.
//  Copyright © 2018年 lijiale. All rights reserved.
//

	#import "HLCMyCollectionViewController.h"
#import "HLCHouseTableViewCell.h"
#import "HLCHouseItemStore.h"
#import "HLCHouseItem.h"
#import <SDWebImage/UIImageView+WebCache.h>

	@implementation HLCMyCollectionViewController

	/*
// Only override drawRect: if you perform custom drawing.
// An empty implementation adversely affects performance during animation.
- (void)drawRect:(CGRect)rect {
    // Drawing code
}
*/
- (instancetype)init{
    self = [super init];
    if(self){
        self.tableView.separatorStyle = UITableViewCellSeparatorStyleNone;
        self.tabBarItem.title = @"我";
        UIImage *user = [[UIImage imageNamed:@"user"] imageWithRenderingMode:UIImageRenderingModeAlwaysOriginal];
        self.tabBarItem.image = user;
        UIImage *user_selected = [[UIImage imageNamed:@"user_selected"] imageWithRenderingMode:UIImageRenderingModeAlwaysOriginal];
        self.tabBarItem.selectedImage = user_selected;
    }
    return self;
}
- (void)viewDidLoad{
    [super viewDidLoad];
    self.navigationItem.title = @"我的收藏";
    NSLog(@"View did Load");
    //设置UIView从导航栏下面开始
    self.navigationController.navigationBar.translucent = NO;
    self.navigationItem.rightBarButtonItem = [[UIBarButtonItem alloc]initWithTitle:@"分享" style:UIBarButtonItemStylePlain target:self action:@selector(collectionShare:)];
    [self.tableView registerClass:[HLCHouseTableViewCell  class] forCellReuseIdentifier:@"CollectionCell"];
}
- (void)viewWillAppear:(BOOL)animated{
    [super viewWillAppear:YES];
    [self.tableView reloadData];
}
#pragma mark - cell
- (NSInteger)numberOfSectionsInTableView:(UITableView *)tableView{
    return 1;
}
- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section{
    return [[HLCHouseItemStore sharedStore] collectionList].count;
}
- (CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath{
    return 120;
}
- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath{
    HLCHouseTableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:@"CollectionCell" forIndexPath:indexPath];
    if(!cell){
        cell = [[HLCHouseTableViewCell alloc] initWithStyle:UITableViewCellStyleSubtitle reuseIdentifier:@"CollectionCell"];
    }
    NSArray *houseList = [HLCHouseItemStore sharedStore].collectionList;
    HLCHouseItem *house = houseList[indexPath.row];
    cell.houseName.text = house.houseName;
    cell.housePrice.text = [house.housePrice stringByAppendingString:@"万"];
    cell.houseLoaction.text = [house.houseArea stringByAppendingString:house.houseBlock];
    cell.houseCollection.tag = house.houseID;
    NSURL *imageUrl = [NSURL URLWithString:house.houseImage];
    //imageUrl = [NSURL URLWithString:@"https://pic1.ajkimg.com/display/anjuke/600675-%E6%88%91%E7%88%B1%E6%88%91%E5%AE%B6/ead933be14ffc87240b9fc4d1c664763-599x450.jpg?t=1"];
    [cell.houseImage sd_setImageWithURL:imageUrl completed:^(UIImage * _Nullable image, NSError * _Nullable error, SDImageCacheType cacheType, NSURL * _Nullable imageURL) {
        NSLog(@"load image complete");
    }];
    [cell.houseCollection addTarget:self action:@selector(collectHouse:) forControlEvents:UIControlEventTouchUpInside];
    if(house.isCollected){
        [cell.houseCollection setImage:[UIImage imageNamed:@"star_collected"] forState:UIControlStateNormal];
    }
    else{
        [cell.houseCollection setImage:[UIImage imageNamed:@"star_notcollect"] forState:UIControlStateNormal];
    }
    return cell;
}
- (void)collectHouse:(UIButton *)sender{
    UIAlertController *alertController = [UIAlertController alertControllerWithTitle:@"👌" message:@"收藏成功" preferredStyle:UIAlertControllerStyleAlert];
    UIAlertAction *okAction = [UIAlertAction actionWithTitle:@"确定" style:UIAlertActionStyleCancel handler:nil];
    NSArray *houselist = [[HLCHouseItemStore sharedStore] HouseList];
    for (HLCHouseItem *house in houselist) {
        if(house.houseID == sender.tag){
            if(house.isCollected == YES){
                [sender setImage:[UIImage imageNamed:@"star_notcollect"] forState:UIControlStateNormal];
                [alertController setMessage:@"取消收藏成功"];
            }
            else{
                [sender setImage:[UIImage imageNamed:@"star_collected"] forState:UIControlStateNormal];
                [alertController setMessage:@"收藏成功"];
            }
            break;
        }
    }
    [alertController addAction:okAction];
    [self presentViewController:alertController animated:YES completion:nil];
    [[HLCHouseItemStore sharedStore] changeCollectStateWithHouseID:sender.tag];
    [self.tableView reloadData];
}
//截图
- (UIImage *)getCurrentScreenShot{
    UIGraphicsBeginImageContextWithOptions([[[UIApplication sharedApplication] keyWindow] bounds].size, NO, 0.0);
    [[[UIApplication sharedApplication] keyWindow].layer renderInContext:UIGraphicsGetCurrentContext()];
    UIImage *image = UIGraphicsGetImageFromCurrentImageContext();
    UIGraphicsEndImageContext();
    
    return image;
}
- (void)collectionShare:(UIButton *)sender{
    UIImage *shareImage = [[UIImage alloc] init];
    shareImage = [self getCurrentScreenShot];
    UIActivityViewController *share = [[UIActivityViewController alloc] initWithActivityItems:@[shareImage] applicationActivities:nil];
    [self presentViewController:share animated:YES completion:nil];
    [share setCompletionWithItemsHandler:^(UIActivityType  _Nullable activityType, BOOL completed, NSArray * _Nullable returnedItems, NSError * _Nullable activityError) {
        NSLog(@"当前分享平台 %@",activityType);
        if(completed){
            NSLog(@"分享成功");
        }
        else{
            NSLog(@"分享失败");
        }
    }];
}
@end

	```
6. 自定义TableViewCell类    

	```objc
	//
//  HLCHouseTableViewCell.m
//  HouseList&Collection
//
//  Created by lijiale on 2018/7/26.
//  Copyright © 2018年 lijiale. All rights reserved.
//

	#import "HLCHouseTableViewCell.h"

	@implementation HLCHouseTableViewCell

	- (void)awakeFromNib {
    [super awakeFromNib];
    // Initialization code
}
	- (void)setSelected:(BOOL)selected animated:(BOOL)animated {
    [super setSelected:selected animated:animated];

    // Configure the view for the selected state
}
	- (void)setFrame:(CGRect)frame{
    frame.size.height = 120;
    frame.size.width = [UIScreen mainScreen].bounds.size.width;
    [super setFrame:frame];
}
- (instancetype)initWithStyle:(UITableViewCellStyle)style reuseIdentifier:(NSString *)reuseIdentifier{
    self = [super initWithStyle:style reuseIdentifier:reuseIdentifier];
    if(self){
        self.backgroundColor = [UIColor clearColor];
        //self.layer.cornerRadius = 10;
        self.layer.masksToBounds = YES;
        //self.layer.shouldRasterize = YES;
        self.layer.borderColor = [UIColor grayColor].CGColor;
        self.layer.borderWidth = 0.1;
        //创建houseImage
        self.houseImage = [[UIImageView alloc] init];
        [self.houseImage setContentMode:UIViewContentModeScaleAspectFill];
        //self.houseImage.translatesAutoresizingMaskIntoConstraints = NO;
        self.houseImage.backgroundColor = [UIColor clearColor];
        self.houseImage.contentMode = UIViewContentModeScaleAspectFill;
        [self.contentView addSubview:self.houseImage];
        [self.houseImage mas_makeConstraints:^(MASConstraintMaker *make) {
            make.height.equalTo(@100);
            make.width.equalTo(@100);
            make.left.equalTo(self.mas_left).with.offset(10);
            make.top.equalTo(self.mas_top).with.offset(10);
        }];
        //创建houseName
        self.houseName = [[UILabel alloc] init];
        self.houseName.backgroundColor = [UIColor clearColor];
        self.houseName.contentMode = UIViewContentModeBottom;
        self.houseName.numberOfLines = 0;
        [self.contentView addSubview:self.houseName];
        [self.houseName mas_makeConstraints:^(MASConstraintMaker *make) {
            make.left.equalTo(self.houseImage.mas_right).with.offset(40);
            make.top.equalTo(self.houseImage.mas_top);
            make.right.equalTo(self.mas_right).with.offset(-20);
            make.height.equalTo(@44);
        }];
        //创建tag
        //创建houseBlock
        self.houseBlock = [[UILabel alloc] init];
        self.houseBlock.backgroundColor = [UIColor clearColor];
        [self.houseBlock sizeToFit];
        self.houseBlock.alpha = 0.5;
        self.houseBlock.font = [UIFont systemFontOfSize:14];
        [self.contentView addSubview:self.houseBlock];
        [self.houseBlock mas_makeConstraints:^(MASConstraintMaker *make) {
            make.left.equalTo(self.houseName.mas_left);
            make.height.equalTo(@20);
            make.width.lessThanOrEqualTo(@60);
            make.top.equalTo(self.houseName.mas_bottom).with.offset(10);
        }];
        //创建houseCommunity
        self.houseCommunity = [[UILabel alloc] init];
        self.houseCommunity.backgroundColor = [UIColor clearColor];
        [self.houseCommunity sizeToFit];
        self.houseCommunity.alpha = 0.5;
        self.houseCommunity.font = [UIFont systemFontOfSize:14];
        [self.contentView addSubview:self.houseCommunity];
        [self.houseCommunity mas_makeConstraints:^(MASConstraintMaker *make) {
            make.left.equalTo(self.houseBlock.mas_right).with.offset(10);
            make.height.equalTo(@20);
            make.width.lessThanOrEqualTo(@120);
            make.top.equalTo(self.houseBlock.mas_top);
        }];
        //创建housePrice
        self.housePrice = [[UILabel alloc] init];
        self.housePrice.backgroundColor = [UIColor clearColor];
        [self.housePrice sizeToFit];
        self.housePrice.font = [UIFont systemFontOfSize:20];
        self.housePrice.textColor = [UIColor redColor];
        [self.contentView addSubview:self.housePrice];
        [self.housePrice mas_makeConstraints:^(MASConstraintMaker *make) {
            make.left.equalTo(self.houseName.mas_left);
            make.height.equalTo(@20);
            make.bottom.equalTo(self.houseImage);
        }];
        //h创建tag
        self.tag1 = [[UILabel alloc] init];
        self.tag1.backgroundColor = [UIColor orangeColor];
        [self.tag1 sizeToFit];
        self.tag1.font = [UIFont systemFontOfSize:14];
        self.tag1.text = @"近地铁";
        [self.contentView addSubview:self.tag1];
        [self.tag1 mas_makeConstraints:^(MASConstraintMaker *make) {
            make.left.equalTo(self.housePrice.mas_right).with.offset(20);
            make.height.equalTo(@20);
            make.bottom.equalTo(self.houseImage);
        }];
        //创建收藏按钮
        self.houseCollection = [[UIButton alloc] init];
        self.houseCollection.backgroundColor = [UIColor clearColor];
        [self.houseCollection setImage:[UIImage imageNamed:@"star_notcollect"] forState:UIControlStateNormal];
        [self.contentView addSubview:_houseCollection];
        [self.houseCollection mas_makeConstraints:^(MASConstraintMaker *make) {
            make.right.equalTo(self.mas_right).offset(-10);
            make.height.equalTo(self.housePrice);
            make.width.equalTo(self.houseCollection.mas_height);
            make.top.equalTo(self.housePrice);
        }];
    }
    return self;
}
@end

	```   
	
- 项目结果截图
- 

 
 
    
  
  
  
  
  