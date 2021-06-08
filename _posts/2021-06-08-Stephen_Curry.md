---
title: "NBA API를 활용한 스테판 커리 슛 데이터 추출 및 대시보드 시각화"
tags: [Blog,Python,Tableau]
style:
color:
description: TBWX 마지막 과제를 위한 데이터 수집 및 대시보드 시각화
---

### NBA API를 활용하여 스테판 커리 20/21 시즌 슛 정보 가져오기

참고:https://dataliteracyhq.slack.com/archives/C020MDFK918/p1622979891003500

##### NBA api 중 shotchartdetail을 불러옵니다


```python
from nba_api.stats.endpoints import shotchartdetail
import json
import pandas as pd
```

#### FGA는 슛 시도, team_id = 0으로 불러올시 플레이어의 전체 정보를 불러올 수 있습니다. 
#### 커리의 player id는 201939입니다. season은 2020-21로 설정 후 정규 시즌만 불러옵니다


```python
response = shotchartdetail.ShotChartDetail(
    context_measure_simple='FGA',
    team_id=0,
    player_id=201939,
    season_nullable='2020-21',
    season_type_all_star='Regular Season'
)

content = json.loads(response.get_json())
```

#### 불러온 정보를 데이터 프레임화 합니다


```python
results = content['resultSets'][0]
headers = results['headers']
rows = results['rowSet']
df = pd.DataFrame(rows)
df.columns = headers
```

#### TBWX 커뮤니티의 마지막 과제(수료를 위해 과제 마무리가 필요하여 이 데이터를 활용하였습니다)


```python
df.to_csv("D:/TBWX/개인과제2차/stephen_curry.csv", index=False)
```

아래 만든 대시보드 링크 남깁니다. 위의 참고 링크를 많이 참조하였습니다.

태블로 퍼블릭 링크: https://public.tableau.com/app/profile/je.ho.yeon/viz/TBWX2-2021/StephenCurryShootsAnalysis
