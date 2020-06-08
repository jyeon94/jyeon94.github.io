### 딩고 프리스타일 간단한 분석

데이터는 전 유튜브 크롤링한 데이터를 기반으로 했습니다.

*참고: <https://shinminyong.tistory.com/10?category=835486>


```python
# 데이터 로드
import pandas as pd
dingo = pd.read_csv("dingo.csv")
```


```python
dingo
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
      <td>세상에 공개 되면 안 되는 미방분 모음! / [영리한 문제아들] 비하인드 : 미방분...</td>
      <td>https://i.ytimg.com/vi/hDRlWn78e6A/hqdefault.j...</td>
      <td>8.7만회</td>
      <td>16시간 전</td>
      <td>http://www.youtube.com/watch?v=hDRlWn78e6A</td>
      <td>2020.6.7</td>
      <td>댓글 451개</td>
      <td>2.3천개</td>
      <td>24개</td>
    </tr>
    <tr>
      <th>1</th>
      <td>이 집 제육볶음 잘 하네... I [어이~ 오씨~] 오담률 (김농밀) I TEASER</td>
      <td>https://i.ytimg.com/vi/-biNhiZx3SU/hqdefault.j...</td>
      <td>18만회</td>
      <td>3일 전</td>
      <td>http://www.youtube.com/watch?v=-biNhiZx3SU</td>
      <td>2020.6.5</td>
      <td>댓글 864개</td>
      <td>2.6천개</td>
      <td>51개</td>
    </tr>
    <tr>
      <th>2</th>
      <td>나이 많은 사람은 힙합을 싫어한다고?! 트로트랩을 들은 어르신들의 반응은 과연......</td>
      <td>https://i.ytimg.com/vi/FgZjW_QMTW8/hqdefault.j...</td>
      <td>20만회</td>
      <td>4일 전</td>
      <td>http://www.youtube.com/watch?v=FgZjW_QMTW8</td>
      <td>2020.6.4</td>
      <td>댓글 1,590개</td>
      <td>4.3천개</td>
      <td>63개</td>
    </tr>
    <tr>
      <th>3</th>
      <td>[4K] MBC 힙합걸Z (브린(Bryn), 하선호(Sandy), 이영지)의 킬링벌...</td>
      <td>https://i.ytimg.com/vi/UDex1bH2vbM/hqdefault.j...</td>
      <td>122만회</td>
      <td>1주 전</td>
      <td>http://www.youtube.com/watch?v=UDex1bH2vbM</td>
      <td>2020.5.29</td>
      <td>댓글 6,339개</td>
      <td>2.8만개</td>
      <td>710개</td>
    </tr>
    <tr>
      <th>4</th>
      <td>남초 회사 근무중... 어떤 오빠가 제일 문제일까? | [영리한 문제아들 EP.4]...</td>
      <td>https://i.ytimg.com/vi/JLjQMj9uidM/hqdefault.j...</td>
      <td>38만회</td>
      <td>1주 전</td>
      <td>http://www.youtube.com/watch?v=JLjQMj9uidM</td>
      <td>2020.5.28</td>
      <td>댓글 1,971개</td>
      <td>6.6천개</td>
      <td>125개</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Rohann (이로한)의 첫 정규앨범 NEVERLAND✨✨ | [집에 갈래?] 변하...</td>
      <td>https://i.ytimg.com/vi/hjzKy20G6Lg/hqdefault.j...</td>
      <td>11만회</td>
      <td>1주 전</td>
      <td>http://www.youtube.com/watch?v=hjzKy20G6Lg</td>
      <td>2020.5.27</td>
      <td>댓글 391개</td>
      <td>2.4천개</td>
      <td>28개</td>
    </tr>
    <tr>
      <th>6</th>
      <td>수퍼비, 트월킹이다 질문받는다 | [영리한 문제아들] 비하인드 : 댓글 읽기</td>
      <td>https://i.ytimg.com/vi/r_5gsfcH-Wg/hqdefault.j...</td>
      <td>25만회</td>
      <td>1주 전</td>
      <td>http://www.youtube.com/watch?v=r_5gsfcH-Wg</td>
      <td>2020.5.26</td>
      <td>댓글 1,960개</td>
      <td>5.4천개</td>
      <td>78개</td>
    </tr>
    <tr>
      <th>7</th>
      <td>(전) 던밀스 (역)🎉🎉  드디어 싹이 보이는 딩고 VMC 콜라보...? / [던밀...</td>
      <td>https://i.ytimg.com/vi/TxevvmA4uho/hqdefault.j...</td>
      <td>32만회</td>
      <td>2주 전</td>
      <td>http://www.youtube.com/watch?v=TxevvmA4uho</td>
      <td>2020.5.25</td>
      <td>댓글 1,262개</td>
      <td>5.3천개</td>
      <td>67개</td>
    </tr>
    <tr>
      <th>8</th>
      <td>트로트랩 (Melody Ver.) | [Official Lyric Video] 수퍼...</td>
      <td>https://i.ytimg.com/vi/NE8rtMcB15A/hqdefault.j...</td>
      <td>46만회</td>
      <td>2주 전</td>
      <td>http://www.youtube.com/watch?v=NE8rtMcB15A</td>
      <td>2020.5.23</td>
      <td>댓글 2,970개</td>
      <td>9.1천개</td>
      <td>105개</td>
    </tr>
    <tr>
      <th>9</th>
      <td>내일 발매될 스월비 정규앨범 미리듣기! I [앨범 스포일러] 스월비 (Swervy)...</td>
      <td>https://i.ytimg.com/vi/vbKRXir7114/hqdefault.j...</td>
      <td>12만회</td>
      <td>2주 전</td>
      <td>http://www.youtube.com/watch?v=vbKRXir7114</td>
      <td>2020.5.22</td>
      <td>댓글 734개</td>
      <td>2.4천개</td>
      <td>76개</td>
    </tr>
    <tr>
      <th>10</th>
      <td>우리 트웰브 콘서트 갈 건데 트월킹 너도 올래? / [영리한 문제아들] EP.3 비...</td>
      <td>https://i.ytimg.com/vi/FQWlTIyBa24/hqdefault.j...</td>
      <td>12만회</td>
      <td>2주 전</td>
      <td>http://www.youtube.com/watch?v=FQWlTIyBa24</td>
      <td>2020.5.22</td>
      <td>댓글 1,021개</td>
      <td>2.1천개</td>
      <td>28개</td>
    </tr>
    <tr>
      <th>11</th>
      <td>영앤리치 ______를 속여라! | [영리한 문제아들 EP.3] 트웰브는 아무도 모...</td>
      <td>https://i.ytimg.com/vi/A7qyHf5Ljlk/hqdefault.j...</td>
      <td>30만회</td>
      <td>2주 전</td>
      <td>http://www.youtube.com/watch?v=A7qyHf5Ljlk</td>
      <td>2020.5.21</td>
      <td>댓글 2,256개</td>
      <td>6천개</td>
      <td>77개</td>
    </tr>
    <tr>
      <th>12</th>
      <td>※그레이의 첫 번째 자작곡 유출※ / 브이터뷰 [V-Terview]</td>
      <td>https://i.ytimg.com/vi/__HnCH0RD1U/hqdefault.j...</td>
      <td>18만회</td>
      <td>2주 전</td>
      <td>http://www.youtube.com/watch?v=__HnCH0RD1U</td>
      <td>2020.5.20</td>
      <td>댓글 188개</td>
      <td>1.3천개</td>
      <td>27개</td>
    </tr>
    <tr>
      <th>13</th>
      <td>[4K] 언에듀케이티드 키드의 킬링벌스를 라이브로! | 돈벌러가야대, Uneduca...</td>
      <td>https://i.ytimg.com/vi/CxuJQ32ih80/hqdefault.j...</td>
      <td>66만회</td>
      <td>2주 전</td>
      <td>http://www.youtube.com/watch?v=CxuJQ32ih80</td>
      <td>2020.5.19</td>
      <td>댓글 5,262개</td>
      <td>1.2만개</td>
      <td>1.3천개</td>
    </tr>
    <tr>
      <th>14</th>
      <td>비주얼 보이그룹 LEGIT GOONS, 눈물의 데뷔 무대 (w 넉살 전 매니저) I...</td>
      <td>https://i.ytimg.com/vi/SsT_850H4Ac/hqdefault.j...</td>
      <td>10만회</td>
      <td>3주 전</td>
      <td>http://www.youtube.com/watch?v=SsT_850H4Ac</td>
      <td>2020.5.15</td>
      <td>댓글 483개</td>
      <td>2.3천개</td>
      <td>40개</td>
    </tr>
    <tr>
      <th>15</th>
      <td>[MV] GRAY - Moon Blue | [DF FILM] DF X GRAY</td>
      <td>https://i.ytimg.com/vi/o9E9zsd4IRQ/hqdefault.j...</td>
      <td>62만회</td>
      <td>3주 전</td>
      <td>http://www.youtube.com/watch?v=o9E9zsd4IRQ</td>
      <td>2020.5.14</td>
      <td>댓글 1,044개</td>
      <td>2.2만개</td>
      <td>68개</td>
    </tr>
  </tbody>
</table>
</div>



#### 전처리


```python
# start_date의 날짜만 남겨놓기 위해 전처리
dingo['start_date'] = dingo['start_date'].replace("최초 공개","",regex=True).replace("•","",regex=True).replace(":","",regex=True).replace("\.$","",regex=True).str.replace(" ","")
```


```python
#view의 만회를 실질적인 숫자로 변경
df['view'] = dingo['view'].replace({'만회':'*10000'},regex=True).map(pd.eval).astype(int)
```




    0       87000
    1      180000
    2      200000
    3     1220000
    4      380000
    5      110000
    6      250000
    7      320000
    8      460000
    9      120000
    10     120000
    11     300000
    12     180000
    13     660000
    14     100000
    15     620000
    Name: view, dtype: int32




```python
#comment컬럼도 숫자만 처리
dingo['comment']=dingo['comment'].str.replace(",","").str.replace(r'(\w+\s)',"").str.replace("개","")
```


```python
#likes컬럼 천개,만개 숫자로 변경
```
