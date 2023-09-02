---
  title: zzd's blog
  date: '2023-08-15'
---

- 最近使用Hexo搭建一个私人博客,用于记录生活与工作中发生的乐趣.之前使用过VuePress静态建站系统,后来升级为VitePress,时间忙碌,不得不放弃了.
- 目前使用Hexo搭建的博客已经托管到Vercel上,欢迎大家访问[Zzd's Blog](https://blog-rho-ruby.vercel.app/),这里作者使用的是Hexo主题[Cactus, 仙人掌](https://github.com/probberechts/hexo-theme-cactus),整体风格简约
### 开始

>安装Hexo
[点击进入Hexo官方网站](https://hexo.io/)

1. 安装Hexo
    安装Node.js,点击对应版本(LTS版本为稳定版本)
  windows下载: [node](https://nodejs.org/en/download)
  mac下载: [node](https://nodejs.org/en/download)
    安装Git
  windows下载: [git](https://git-scm.com/downloads)
  mac下载: [git](https://git-scm.com/downloads)
    全局安装Hexo
	```
    npm install -g hexo-cli
  ```

2. 初始化Hexo
  安装 Hexo 完成后，请执行下列命令，Hexo 将会在指定文件夹中新建所需要的文件。
  ```
    hexo init <file name>
    cd <file name>
    npm install
  ```
3. 运行Hexo
  在package.json中,使用`npm run server`命令启动Hexo,控制台打开http://localhost:4000/
  ```
  "scripts": {
    "build": "hexo generate",
    "clean": "hexo clean",
    "deploy": "hexo deploy",
    "server": "hexo server"
  }
  ```

>安装Hexo主题
[点击进入Hexo官方主题网站](https://hexo.io/themes/)
选择心意的主题,这里作者使用的主题为cactus,作为例子,主题安装步骤基本相同。

1. 安装主题
在安装目录中,
```
$ git clone https://github.com/probberechts/hexo-theme-cactus.git themes/cactus
```
2. 更改主题配置
```
cd themes/cactus/_config.yml
# theme: landscape
theme: cactus
```
3. 创建页面和文章目录,这将在source目录中生成markdown文件
```
hexo new post hello-hexo
```
4. 当需要在本地调试Hexo时,修改文件时,hexo并不会热更新,需要手动执行hexo server命令,会浪费些不必要的时间,可以安装`hexo-browsersync`插件来自动热更新
```
npm install hexo-browsersync --save
```
