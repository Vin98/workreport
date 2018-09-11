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
  
  
  