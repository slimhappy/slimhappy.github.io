---
layout: post
title:  "Image_retrieval"
date:   2018-05-19 19:00:00
categories: MasterDegree
---

## 闲来无事想用ruby写个爬虫试试。

目标：获取B站连载榜单

### 1.尝试nokogiri

首先尝试了一波nokogiri，
试了半天发现这玩意儿只能解析静态的网页文本。
果断不符合要求，榜单类的信息肯定是从后台调取的，不可能是固定写死在页面里面。

### 2.尝试selenium-webdriver

通过去rubychina看看dalao们对ruby爬虫的信息整理，  
其中有一篇整合得非常好，推荐一下：[Ruby 的爬虫世界](https://ruby-china.org/topics/31784)

#### 具体实现如下：

```ruby
#get hot rank list from bilibili.com 
#using safari development mod
#using begin...
# 	   rescue...
#      end
require 'rubygems'
# require 'open-uri'
# require 'Nokogiri'
# require 'watir'
require 'selenium-webdriver'
# page = Nokogiri::HTML(open("https://www.bilibili.com/v/anime/serial/?spm_id_from=333.334.primary_menu.8#/"))
# puts page.class
# link = page.css('div.r-con')
# puts link.css('li')
# link.each do |i|
# 	p link[i]["href"]
# end
dr = Selenium::WebDriver.for :safari
url = 'https://www.bilibili.com/v/anime/serial/?spm_id_from=333.334.primary_menu.8#/'
dr.navigate.to url
dr.get url

puts dr.title

links_hot1_3 = dr.find_element(:class,'rank-list hot-list').find_elements(:class,'rank-item  show-detail first highlight')
links_hot1_3.each do |i|
	puts i.text
end
links_hot4_7 = dr.find_element(:class,'rank-list hot-list').find_elements(:class,'rank-item  show-detail')
links_hot4_7.each do |i|
	puts i.text
end
```

#### 结果：

```ruby
JusticedeMacBook-Pro:crawler justice$ ruby bilibili_rank_hot.rb
连载动画 - 哔哩哔哩 (゜-゜)つロ 干杯~-bilibili
1【1月】OVERLORD 第二季 04【独家正版】综合评分：280.4万
2【1月】龙王的工作！ 04【独家正版】综合评分：129.7万
3【1月】卫宫家今天的饭 02【独家正版】综合评分：100.3万
1【1月】OVERLORD 第二季 04【独家正版】综合评分：280.4万
2【1月】龙王的工作！ 04【独家正版】综合评分：129.7万
3【1月】卫宫家今天的饭 02【独家正版】综合评分：100.3万
4【1月】紫罗兰永恒花园 04【独家正版】综合评分：75.5万
5【1月】刻刻 04【独家正版】综合评分：59.4万
6【1月】小木乃伊到我家 04【独家正版】综合评分：58.4万
7【1月】Fate/EXTRA Last Encore 02【独家正版】综合评分：43.3万
8【1月】博多豚骨拉面团 04【独家正版】综合评分：42.0万
9【1月】pop子和pipi美的日常 05【独家正版】综合评分：41.9万
10【1月】爱吃拉面的小泉同学 05综合评分：39.1万
```

#### 总结：

对于一个全新的功能块，要多用.methods方法获取有用信息，并去相关官网查找对应的文档。
附上[selenium-webdriver](http://www.rubydoc.info/gems/selenium-webdriver)的官方链接。
（有点像matlab里的help，只不过这方面的资料目前都是英文TAT，学好英文是关键啊！！！） 

