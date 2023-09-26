---
layout: post
title: "【Python】利用定向网络爬虫爬取豆瓣电影top250"
author: "Yingwei Guo"
categories: 
tags: 爬虫
image: 
---
> 最近在外地实习，闲来无事学了一下requests库和BeautifulSoup，掌握基本用法之后试着爬取了一下豆瓣电影top250，中间也参考了不少其他大佬的博客，所以最后写出来的代码也都大同小异吧，就当聊以自慰了。

## 简介
   利用requests库和bs4中的BeautifulSoup，实现对豆瓣电影top250的爬取，最后将电影信息写入一个文本文件中。
 ## 代码如下
 

```python
'''
@Author     : Guo Yingwei
@Date       : 2019-07-06 18:10:29
@E-mail     : willgyw@126.com
@Description: crawl douban Movies Top250 ,
              and write the information to a file. 
'''

import requests
from bs4 import BeautifulSoup
import codecs


def get_page(url):
    try:
        r = requests.get(url)
        r.raise_for_status()
        r.encoding = r.apparent_encoding
        return r.text
    except:
        return ''
    
def parse_html(html, return_list):
    soup = BeautifulSoup(html, 'html.parser')    
    movie_list = soup.find('ol', attrs={'class': 'grid_view'}).find_all('li')
    for movie in movie_list:
        title = movie.find('span', attrs={'class': 'title'}).get_text()
        rating_num = movie.find('span', attrs={'class': 'rating_num'}).get_text()
        inq = movie.find('span', attrs={'class': 'inq'}).get_text()
        return_list.append([title, rating_num, inq])
    return return_list

def write_file(return_list):   
    with codecs.open('doubanMoviesTop250.txt','wb+', encoding='utf-8') as f:
        tplt = u'{0:<4}\t{1:{4}^16}\t{2:{4}^10}\t{3:<20}'
        f.write(tplt.format('排名', '名称', '评分', '一句话简介', chr(12288)) + '\n')
        count = 0
        for i in return_list:
            count += 1
            f.write(tplt.format(count, i[0], i[1],i[2], chr(12288)) + '\n')
                   
def main():
    start_url='https://movie.douban.com/top250'
    movie_list = []    
    for i in range(10):
        html = get_page(start_url + '?start=' + str(25*i))
        parse_html(html, movie_list)
    write_file(movie_list)    

if __name__ == '__main__':
    main()
```

## 结果如下
![爬取结果](https://img-blog.csdnimg.cn/2019071821070388.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjM1ODA3Nw==,size_16,color_FFFFFF,t_70)

部分网站源码如下：

```
<ol class="grid_view">
        <li>
            <div class="item">
                <div class="pic">
                    <em class="">1</em>
                    <a href="https://movie.douban.com/subject/1292052/">
                        <img width="100" alt="肖申克的救赎" src="https://img3.doubanio.com/view/photo/s_ratio_poster/public/p480747492.webp" class="">
                    </a>
                </div>
                <div class="info">
                    <div class="hd">
                        <a href="https://movie.douban.com/subject/1292052/" class="">
                            <span class="title">肖申克的救赎</span>
                                    <span class="title">&nbsp;/&nbsp;The Shawshank Redemption</span>
                                <span class="other">&nbsp;/&nbsp;月黑高飞(港)  /  刺激1995(台)</span>
                        </a>


                            <span class="playable">[可播放]</span>
                    </div>
                    <div class="bd">
                        <p class="">
                            导演: 弗兰克·德拉邦特 Frank Darabont&nbsp;&nbsp;&nbsp;主演: 蒂姆·罗宾斯 Tim Robbins /...<br>
                            1994&nbsp;/&nbsp;美国&nbsp;/&nbsp;犯罪 剧情
                        </p>

                        
                        <div class="star">
                                <span class="rating5-t"></span>
                                <span class="rating_num" property="v:average">9.6</span>
                                <span property="v:best" content="10.0"></span>
                                <span>1477697人评价</span>
                        </div>

                            <p class="quote">
                                <span class="inq">希望让人自由。</span>
                            </p>
                    </div>
                </div>
            </div>
        </li>
        <li>
            <div class="item">
                <div class="pic">
                    <em class="">2</em>
                    <a href="https://movie.douban.com/subject/1291546/">
                        <img width="100" alt="霸王别姬" src="https://img3.doubanio.com/view/photo/s_ratio_poster/public/p1910813120.webp" class="">
                    </a>
                </div>
                <div class="info">
                    <div class="hd">
                        <a href="https://movie.douban.com/subject/1291546/" class="">
                            <span class="title">霸王别姬</span>
                                <span class="other">&nbsp;/&nbsp;再见，我的妾  /  Farewell My Concubine</span>
                        </a>


                            <span class="playable">[可播放]</span>
                    </div>
                    <div class="bd">
                        <p class="">
                            导演: 陈凯歌 Kaige Chen&nbsp;&nbsp;&nbsp;主演: 张国荣 Leslie Cheung / 张丰毅 Fengyi Zha...<br>
                            1993&nbsp;/&nbsp;中国大陆 香港&nbsp;/&nbsp;剧情 爱情 同性
                        </p>

                        
                        <div class="star">
                                <span class="rating5-t"></span>
                                <span class="rating_num" property="v:average">9.6</span>
                                <span property="v:best" content="10.0"></span>
                                <span>1094842人评价</span>
                        </div>

                            <p class="quote">
                                <span class="inq">风华绝代。</span>
                            </p>
                    </div>
                </div>
            </div>
        </li>
```


博主也是刚刚接触Python的新手，如有错误，欢迎指正。


