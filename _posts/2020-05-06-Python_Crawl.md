---
title: "유튜브 크롤링"
tags: [Python,TIL]
style: 
color:
description: 딩고프리스타일 크롤링
---
참고: [me뇽](https://shinminyong.tistory.com/2?category=835486)

### 유튜브 크롤링

유튜버들 중 한분을 선정하여 크롤링을 진행해봤습니다 <br/>
위의 블로그를 참고하여 진행해봤습니다.

저는 제가 좋아하는 "Dingo Freestyle" 채널을 크롤링했습니다.

Selenium과 BeautifulSoup을 모두 사용하여 진행했습니다.

#### 사용할 모듈 import


```python
import requests
from bs4 import BeautifulSoup
import time
import urllib.request 
import re
import pandas as pd
from selenium.webdriver import Chrome
from selenium.webdriver.common.keys import Keys
import datetime as dt
```

#### Youtube 접속


```python
browser =  Chrome(executable_path='E:/chromedriver_win32_1/chromedriver.exe')
```


```python
base_url = 'http://www.youtube.com'
browser.get(base_url)
browser.maximize_window()
```

#### 개발자도구를 통해 page source 보기

<img width="407" alt="주석 2020-05-06 140628" src="https://user-images.githubusercontent.com/57039464/81140081-0dab6900-8fa3-11ea-955a-4f689fbe038b.png">


#### 검색하고 싶은 유튜버를 입력한 후 enter 클릭


```python
# copy full xpath를 했습니다
# 추후에 xpath,css selector를 더 공부하여 손쉬운 접근법으로 개선하겠습니다.
```


```python
browser.find_elements_by_xpath('/html/body/ytd-app/div/div/ytd-masthead/div[3]/div[2]/ytd-searchbox/form/div/div[1]/div/div[2]/input')[0].click()
```


```python
browser.find_elements_by_xpath('/html/body/ytd-app/div/div/ytd-masthead/div[3]/div[2]/ytd-searchbox/form/div/div[1]/div/div[2]/input')[0].send_keys('dingo freestyle') #원하는 유튜버 입력
browser.find_elements_by_xpath('/html/body/ytd-app/div/div/ytd-masthead/div[3]/div[2]/ytd-searchbox/form/div/div[1]/div/div[2]/input')[0].send_keys(Keys.RETURN) #엔터
```

#### 이동한 화면에서 "Dingo FreeStyle" 클릭


```python
browser.find_element_by_xpath('/html/body/ytd-app/div/ytd-page-manager/ytd-search/div[1]/ytd-two-column-search-results-renderer/div/ytd-section-list-renderer/div[2]/ytd-item-section-renderer/div[3]/ytd-channel-renderer/div/div[2]/a/div[1]/ytd-channel-name/div/div/yt-formatted-string').click()
```

#### 동영상 클릭

<img width="452" alt="주석 2020-05-06 154107" src="https://user-images.githubusercontent.com/57039464/81144939-20786a80-8fb0-11ea-8ed1-cc9b1d0545ed.png">



```python
browser.find_element_by_xpath('//*[@class="scrollable style-scope paper-tabs"]/paper-tab[2]').click()
```

#### 스크롤 내리기

모든 동영상들의 page source를 보려면 스크롤을 내려야 하기 때문에 <br/>
이 작업을 진행해 줍니다. 30회정도로 한정하여 스크롤을 내려보겠습니다.


```python
body = browser.find_element_by_tag_name('body')
```


```python
num_of_pagedowns = 20
while num_of_pagedowns:
    body.send_keys(Keys.PAGE_DOWN)
    time.sleep(2)
    num_of_pagedowns -= 1
```

#### BeautifulSoup을 활용한 page source 추출

Selenium은 오래걸리기 때문에 BeautifulSoup을 활용하여 소스를 추출합니다.


```python
html0 = browser.page_source
```


```python
html = BeautifulSoup(html0,'html.parser')
```


```python
video_list2 = html.find_all('ytd-grid-video-renderer',{'class':'style-scope ytd-grid-renderer'})
```


```python
dingo_url = []
for i in range(len(video_list2)):
    url = base_url + video_list2[i].find('a',{'id':'thumbnail'})['href']
    dingo_url.append(url)
```


```python
dingo = pd.DataFrame({'name':[],
                     'thumbnail':[],
                     'view':[],
                     'previous_time':[],
                     'video_url':[],
                     'start_date':[],
                     'comment':[],
                     'likes_num':[],
                     'unlikes_num':[]})
```


```python
for i in range(0,20):
    #video 제목
    name = video_list2[i].find('a',{'id':'video-title'}).text
    
    #섬네일
    thum = video_list2[i].find('a',{'id':'thumbnail'}).find('img')['src']
    
    #url 정보
    url = base_url + video_list2[i].find('a',{'id':'thumbnail'})['href']
    
    meta0 = video_list2[i].find('div',{'id':'metadata-line'})
    
    #조회수는 숫자만 추출 가능
    view = meta0.find_all('span',{'class':'style-scope ytd-grid-video-renderer'})[0].text.split()[1]
    
    previous = meta0.find_all('span',{'class':'style-scope ytd-grid-video-renderer'})[1].text
    
    start_url = dingo_url[i]
    browser.get(start_url)
    body = browser.find_element_by_tag_name('body')
    
    time.sleep(25) #로딩시간 설정
    
    num_of_pagedowns = 2
    while num_of_pagedowns:
        body.send_keys(Keys.PAGE_DOWN)
        time.sleep(3)
        num_of_pagedowns -= 1
    
    html0 = browser.page_source
    html = BeautifulSoup(html0,'html.parser')
    
    start_date = html.find('div',{'id':'date'}).text
    comment = html.find('h2',{'id':'count'}).find('yt-formatted-string').text
    
    #좋아요수
    likes_num = html.find('yt-formatted-string',{'id':'text','class':'style-scope ytd-toggle-button-renderer style-text',
                                                'aria-label': re.compile('좋아요')}).text+'개'
    
    unlikes_num = html.find('yt-formatted-string',{'id':'text','class':'style-scope ytd-toggle-button-renderer style-text',
                                                'aria-label': re.compile('싫어요')}).text+'개'
    
    insert_data = pd.DataFrame({'name':[name],
                               'thumbnail':[thum],
                               'view': [view],
                               'previous_time' : [previous],
                               'video_url':[url],
                               'start_date':[start_date],
                               'comment':[comment],
                               'likes_num':[likes_num],
                               'unlikes_num':[unlikes_num]})
    dingo = dingo.append(insert_data)
```


    ---------------------------------------------------------------------------

    AttributeError                            Traceback (most recent call last)

    <ipython-input-24-705c63ecf9ae> in <module>
         32 
         33     start_date = html.find('div',{'id':'date'}).text
    ---> 34     comment = html.find('h2',{'id':'count'}).find('yt-formatted-string').text
         35 
         36     #좋아요수
    

    AttributeError: 'NoneType' object has no attribute 'find'



```python
dingo = dingo.reset_index(drop=True)
```


```python
dingo.drop('index',axis=1)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>thumbnail</th>
      <th>view</th>
      <th>previous_time</th>
      <th>video_url</th>
      <th>start_date</th>
      <th>comment</th>
      <th>likes_num</th>
      <th>unlikes_num</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>어린 나이에 부자되는 썰 | [DF SSUL] 영앤리치 레코즈</td>
      <td>https://i.ytimg.com/vi/nAEv_AS4RWY/hqdefault.j...</td>
      <td>16만회</td>
      <td>1일 전</td>
      <td>http://www.youtube.com/watch?v=nAEv_AS4RWY</td>
      <td>•2020. 5. 5.</td>
      <td>댓글 704개</td>
      <td>4.1천개</td>
      <td>66개</td>
    </tr>
    <tr>
      <th>1</th>
      <td>혹시... 트웰브를 아시나요? / [샤라웃] 트웰브 (twlv)</td>
      <td>https://i.ytimg.com/vi/l9Tkvm-KTDY/hqdefault.j...</td>
      <td>4.2만회</td>
      <td>2일 전</td>
      <td>http://www.youtube.com/watch?v=l9Tkvm-KTDY</td>
      <td>•2020. 5. 4.</td>
      <td>댓글 210개</td>
      <td>1천개</td>
      <td>7개</td>
    </tr>
    <tr>
      <th>2</th>
      <td>[4K] 수퍼비의 킬링벌스를 라이브로! | Heu!, +82 Bars, 5Gawd,...</td>
      <td>https://i.ytimg.com/vi/hua6kPSuOss/hqdefault.j...</td>
      <td>146만회</td>
      <td>4일 전</td>
      <td>http://www.youtube.com/watch?v=hua6kPSuOss</td>
      <td>•최초 공개: 2020. 5. 2.</td>
      <td>댓글 11,833개</td>
      <td>4.4만개</td>
      <td>797개</td>
    </tr>
    <tr>
      <th>3</th>
      <td>[#감동실화, #데뷔] 리짓군즈, MBC 쇼!음악중심 데뷔! I [DF INTERV...</td>
      <td>https://i.ytimg.com/vi/1fEYRzS-XV0/hqdefault.j...</td>
      <td>9만회</td>
      <td>5일 전</td>
      <td>http://www.youtube.com/watch?v=1fEYRzS-XV0</td>
      <td>•최초 공개: 2020. 5. 1.</td>
      <td>댓글 292개</td>
      <td>1.8천개</td>
      <td>25개</td>
    </tr>
    <tr>
      <th>4</th>
      <td>영상_엔딩포함_리플적용_치ㅗ종테스트.mov</td>
      <td>https://i.ytimg.com/vi/fFPeDJj351o/hqdefault.j...</td>
      <td>5.3만회</td>
      <td>6일 전</td>
      <td>http://www.youtube.com/watch?v=fFPeDJj351o</td>
      <td>•2020. 4. 30.</td>
      <td>댓글 441개</td>
      <td>868개</td>
      <td>30개</td>
    </tr>
    <tr>
      <th>5</th>
      <td>윤미래와 타이거JK가 음악으로 낳은 딸!  / [샤라웃] 비비 (BIBI)</td>
      <td>https://i.ytimg.com/vi/PyaYyY6e9dk/hqdefault.j...</td>
      <td>5.5만회</td>
      <td>1주 전</td>
      <td>http://www.youtube.com/watch?v=PyaYyY6e9dk</td>
      <td>•2020. 4. 29.</td>
      <td>댓글 142개</td>
      <td>878개</td>
      <td>10개</td>
    </tr>
    <tr>
      <th>6</th>
      <td>[4K] Kid Milli (키드밀리) 의 킬링벌스를 라이브로! I Levitate...</td>
      <td>https://i.ytimg.com/vi/HD9TG-jlAmA/hqdefault.j...</td>
      <td>109만회</td>
      <td>1주 전</td>
      <td>http://www.youtube.com/watch?v=HD9TG-jlAmA</td>
      <td>•최초 공개: 2020. 4. 28.</td>
      <td>댓글 6,156개</td>
      <td>2.9만개</td>
      <td>330개</td>
    </tr>
    <tr>
      <th>7</th>
      <td>[Official Video] LEGIT GOONS (리짓군즈) - Party &amp; ...</td>
      <td>https://i.ytimg.com/vi/dlg1ZgwTsJE/hqdefault.j...</td>
      <td>24만회</td>
      <td>1주 전</td>
      <td>http://www.youtube.com/watch?v=dlg1ZgwTsJE</td>
      <td>•2020. 4. 24.</td>
      <td>댓글 1,045개</td>
      <td>6.5천개</td>
      <td>83개</td>
    </tr>
    <tr>
      <th>8</th>
      <td>고막 녹여버리는 강민경의 Digital Lover / [슈퍼마켓콘서트]</td>
      <td>https://i.ytimg.com/vi/7WvZsD_NCpo/hqdefault.j...</td>
      <td>6.8만회</td>
      <td>1주 전</td>
      <td>http://www.youtube.com/watch?v=7WvZsD_NCpo</td>
      <td>•2020. 4. 23.</td>
      <td>댓글 96개</td>
      <td>955개</td>
      <td>8개</td>
    </tr>
    <tr>
      <th>9</th>
      <td>목소리도 잘생긴 GRAY의 Digital Lover / [슈퍼마켓콘서트]</td>
      <td>https://i.ytimg.com/vi/doUcYolMiVI/hqdefault.j...</td>
      <td>8.4만회</td>
      <td>2주 전</td>
      <td>http://www.youtube.com/watch?v=doUcYolMiVI</td>
      <td>•2020. 4. 22.</td>
      <td>댓글 239개</td>
      <td>1.9천개</td>
      <td>9개</td>
    </tr>
  </tbody>
</table>
</div>


