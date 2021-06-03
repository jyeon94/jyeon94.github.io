---
title: "유튜브 API를 활용한 경돼님 채널 정보 수집"
tags: [Blog,]
style:
color:
description: 스터디에서 활용할 정보 유튜브 API를 활용하여 수집
---

[참고1](https://yobro.tistory.com/193?category=793224) 

[참고2](https://untitledtblog.tistory.com/169)


```python
from googleapiclient.discovery import build
from googleapiclient.errors import HttpError
from oauth2client.tools import argparser
import pandas as pd
```

#### 팀원분이 해당 동영상의 url을 전달주셔서 url에서 videoId만 추출하여 사용하였습니다.


```python
video_id =  pd.read_csv("D:/경돼님_api/경돼영상url.csv",engine='python')
```

#### lambda를 이용해서 각 url을 =로 나누고 그중 2번째값이 videoId를 리스트화합니다


```python
video_full_id = video_id['url'].apply(lambda x:x.split("=")[1]).tolist()
```

#### 키값을 입력합니다


```python
developer_key = "API키"
```


```python
youtube_service_name = "youtube"
youtube_api_version = "v3"
```

#### youtube_api 정보를 만듭니다


```python
youtube_api = build(youtube_service_name,youtube_api_version,developerKey=developer_key)
```

#### 각 뽑아올 항목을 리스트로 만드는 작업입니다.


```python
# snippet에서 뽑아올 수 있는 정보들
title = []
description = []
tags = []
publishedAt = []

# content detail에서 뽑아올 수 있는 정보
duration = []

# statistics에서 뽑아올 수 있는 정보들
view_count = []
likes = []
dislikes = []
comment_count = []
```


```python
df = pd.DataFrame([video_full_id,title,publishedAt,description,tags,duration,view_count,likes,dislikes,comment_count]).T
```


```python
df.columns = ["video_id","title","published_at","description","tags","duration","view_count","likes","dislikes","comment_count"]
```


```python
df = df.reset_index(drop=True)
```


```python
df.to_csv("D:/경돼님_api/경돼님_채널정보.csv",encoding='utf-8- sig',index=False)
```

#### 각 정보를 해당 정보에 맞는 list에 추가합니다
#### 혹시 없을 수 있는 항목이 있기 때문에 try, except를 사용하여 그런 값들은 공백을 넣어줍니다


```python
for i in range(len(video_full_id)):
    request = youtube_api.videos().list(
    part = "snippet,contentDetails,statistics",
    id = video_full_id[i]
    )
    response = request.execute()
    
    publishedAt.append(response['items'][0]['snippet']['publishedAt'])
    title.append(response['items'][0]['snippet']['title'])
    try:
        description.append(response['items'][0]['snippet']['description'])
    except KeyError:
        description.append("")
    try:
        tags.append(''.join(response['items'][0]['snippet']['tags']))
    except KeyError:
        tags.append("")
    duration.append(response['items'][0]['contentDetails']['duration'])
    view_count.append(response['items'][0]['statistics']['viewCount'])
    try:
        likes.append(response['items'][0]['statistics']['likeCount'])
    except KeyError:
        likes.append("")
    try:
        dislikes.append(response['items'][0]['statistics']['dislikeCount'])
    except KeyError:
        dislikes.append("")
    try:
        comment_count.append(response['items'][0]['statistics']['commentCount'])
    except KeyError:
        comment_count.append("")
```


```python
comments_lst = []
```


```python
video_id_lst2 = []
```


```python
# comments_lst_new라는 리스트를 만든 이유는 댓글들을 동영상 마다 구분하기 위함입니다.
for i,j in enumerate(video_full_id):
    video_id_lst2.append(video_full_id[i])
    comments_lst_new = []
    try:
        comments = youtube_api.commentThreads().list(
            part='snippet',
            videoId = video_full_id[i],
            maxResults = 100
        ).execute()
    except HttpError: # 댓글이 막혀있는 동영상의 경우 에러가 발생하기 때문에 이럴시 빈 리스트를 append 해줍니다
        comments_lst.append([])
    while comments:  # 한번에 100개밖에 크롤링이 안되기 때문에 nextPageToken을 이용하여 전부 수집합니다
        for item in comments['items']:
            comment = item['snippet']['topLevelComment']['snippet']
            comments_lst_new.append(comment['textOriginal'])
        if 'nextPageToken' in comments:
            comments = youtube_api.commentThreads().list(part='snippet',videoId = video_full_id[i],
                       pageToken = comments['nextPageToken'],maxResults = 100).execute()                              
        else:
            comments_lst.append(comments_lst_new)
            break
```

    gBx2UrI4JqE
    n2jAepfvF6c
    _thZKDd0NGo
    UURowG79j8c
    0Cn9Jk0BrdE
    QXpJ7WOF4Zo
    w2eBt4FhFVQ
    66z8rKLa-3Q
    JeEU0qeb-0w
    RodnFgR1vmg
    Hb_OsRmhpuA
    5U0Mqr8Twto
    gVAVBuKfya8
    2c_b7d62xCo
    v8NUx1NaIV4
    GoqO4SqYQQM
    

    WARNING:googleapiclient.http:Encountered 403 Forbidden with reason "commentsDisabled"
    

    eFeE-FZe4_M
    eFeE-FZe4_M
    QK9pqWVBfFE
    fx46_GOo_ms
    gkmfepaHejM
    vNZ2er_xyYQ
    fwDjvswIso8
    ZviULrv6bMQ
    _q0hwb68qnQ
    qj9YZrVF46k
    1mk-mrXFmkQ
    RN64_7xoHEw
    KvLyGsiISkI
    v1e_MQN_VGU
    95HaaiPUdXw
    3pm11veI65Y
    LjclPyVzuCI
    1DUopwS7IBQ
    M50wdJKRXKA
    LBPVDuR7w1I
    4o1TY2lVT-k
    8yd5wvZCcWg
    gLm-FShskxg
    bTs1bi9sAqY
    YKDoZVczaBo
    rqN4an-zJaI
    x2rccJbHUN8
    EZqOAvQunDE
    pMTvAAh2a1o
    6bt_Lrk7wW8
    pyDxhZjkDJk
    zxcOc19HRJQ
    9P6MFIUzkAg
    wKW4-NpEY5A
    6zrc8hCy-DQ
    5L1Z08XWFhM
    9HT6e1Eihig
    X6KVUxXrUKg
    ClUqq-UW7JU
    Z1H4RTOfGCg
    4VuDz2IcSnA
    tipClX1QaJE
    CgdO6la8HeA
    4Cxsh0jXeFE
    2ftlNARznU0
    UjtoEBOQTx8
    2ac3Mez-fO0
    _lwNPEaziFs
    j522Ebv84M4
    9gBdZgIE9zQ
    LjviCuyxglo
    mdVkL1bgifQ
    iidJdvELeHc
    6KDBphHqsCs
    j_CoPgJMCSg
    NndQiSdRJrs
    F1EWeKtRj2E
    tzSuHiR3dVo
    gkIIQpWZVJk
    HeUvabaN_c4
    yKr3pdcKgMU
    KQJYG9iYT0I
    hqt6acjG6O0
    ETpxyrA2qVo
    5LKirlp5asE
    kV_eZuKBME0
    gZAaUPLmlzQ
    fXB93zEzyFg
    nJl3nJxhAgc
    Wu8zZA0piIY
    f3nz_Rg8HTM
    XKEWfPF1hdw
    9fXnDPfzHWk
    dL1ONEqMblY
    f2RkKn4wcF8
    hDdw4Gl2hz0
    SkOz6SJRLPE
    NanZKNq9NHw
    xLQ7OtqTqZg
    Frlgq0CjY6w
    7BzkUumns7c
    PdwuLcv2NZ0
    xlsYv-N19kg
    wol-RZndOVU
    jx-HMlvEQts
    -gVphiL8_mM
    Z-teavQnQME
    1glXjNpIHGo
    _R0BZN1rF-k
    goycT32NApA
    fh7FCirlr0w
    99Yx2yu77O0
    HdxfWR2UYBk
    T6EQRt6oKig
    iSA_cSOtvf0
    35coQcHVHyA
    _HEv_N7LA_4
    U-rnTQ6-sWg
    CubC9ybgMUc
    G23uXAyj9ig
    XeE2XUTmoSo
    XyyhqbzHuq8
    I6_0MOznh0s
    vOCeY1m3TTo
    CLqNFQdf2fs
    QmA0hQ85KKU
    J4fsx2Omt2U
    OhopykKfX60
    aUnLbOnyljc
    SABbGD0L8dE
    XYAuEPKvibk
    lGu_2uZpDp0
    Xgf2zXjtPKA
    YQ9G0qkNsJY
    zDAkqIKgX1w
    SI3KATarOgY
    DLC7SsxsuCc
    K70EnITy_DA
    -D3Y7jHa9vw
    4ZjWQssiuFM
    bEiu_vxbHaE
    zzWyhiwwr0M
    r_YOmFjHspk
    igcVeKv9aYQ
    TLw99tUVRlY
    Hhor_vaZLFk
    KU_-cqmAJrk
    Jh-1E7PXpcU
    QkmP4x3gncg
    b2w6Os2HTb8
    IibshV2FopY
    --_wzCITL2w
    kzKNe3--0rE
    PwVlqglkjDk
    uPFcZz8l248
    T_kgfz383dk
    AJoP-2Lr4_s
    f6T2H0gaSf8
    5GeqO-9EzwQ
    dj4Ew_8ogXE
    SRtxQ_gTMNI
    hamxj-KiVlk
    49LnckQDyy4
    DYtDdo74MFM
    4ZOSYb0UttM
    5KaO3PcUSOI
    T8MIpPmI7H8
    M6tPJSWDmMc
    brZdTUTk6o4
    wF67vb4NMwM
    f3y57IP5fjI
    oK3LVnNuddM
    DLyFstQj-gI
    m8DaGvkANow
    pWoTGIhxqco
    88Hd6VlEIEY
    kUwzitZYQpo
    98IZmNNcst0
    K1mWIwMvN8w
    MRH0y6RNmQw
    5TFZL4Yy-9U
    0PKDHWqr07Y
    tR6qeOvLywY
    CWvHO50PWwM
    wC6eDfXhN5w
    nIas88mjwAY
    OGoSZ59US-g
    C0SbCL_ay64
    SKBCVBcC_Kg
    HAyRYzw77zg
    jL0jhDMfT9I
    62ef6WsAYeA
    BBekDF7-JAg
    41zDcXV2ZZk
    uT4pdwydZw4
    tvRhOrmLxd8
    ov71mLiLHhU
    mAEShHI83jA
    KC_JovJvOz8
    GI0Fo7v4F3o
    p1T31yiA6JA
    l2Sos8Id3v4
    3BEvz9bCmTg
    3AkM23Yo5q4
    _681HlwO-tg
    6DoGPeDKFOM
    pjxw8VHjeNI
    SYP6mpZk-dg
    DJ6wn_BPyr8
    Ksm2czTtrOI
    96IcXA0_oqI
    R6olyXsHlgo
    OG1kaNe7D2I
    TicijJJU8j8
    dBCpE72G7o8
    FQjSH1VJQts
    6Ii7m3wnO5g
    dnfWrg4WqDw
    8F4yBUs93M0
    4KrYUD4MEzA
    gKwC-J44ZkY
    -2Apb8Zbisk
    z9lc7KgiZ8c
    OVXM0-0UJXU
    q8fHgYS8z_k
    EV1JpUvf2jw
    LKtNAc2zxMw
    Oauxf_RMgss
    YKwqHYnfxU4
    G8uCuBq55cc
    lPooWmMlkG0
    xSoY-ut3uGY
    LLYxvYPwrHY
    DsyHA63clSk
    nYPzR-QiNFY
    o8-zVq8rqPQ
    FwidrlMvZAY
    iHY5D7ac6aM
    J-SXDhe4sdc
    ouTtNYzDrh8
    m2K5b3uKCTg
    Ts0tnInmUlc
    hON7vVkFMDc
    vdnnTsEt7hw
    ilSqQDD9Ilo
    iQ2dMtRmYcA
    cIsRavyr_3E
    xz1KtLE1zUo
    WbvPFPGJTE4
    TUGcgZoHFP0
    n_hDHmirWls
    72Ivek9rwC4
    JaFj7mGtOzE
    EHpbiqTl7Fo
    Jj9CqBYpr1M
    b9UiLxNRf0c
    tJL3tMXLvz8
    l3Ap-u0vMZ8
    sUP1vhqEKYs
    yh1stpTBEWs
    6vdqykI0wF4
    wAD_xJQtFfU
    nahpOCFC1iM
    RFgGTxToc8M
    SOCYPbxnPNA
    gxBrHEiFwQk
    a7bs_SnJx7U
    c1iJweiYAys
    Jzd0YOIzJaE
    yBDgcPRQuN8
    JAWw3nhQz9s
    RUs_whjlyVM
    Wb-XmjTq1-I
    Liv6p_EcCTI
    3DFYeVGnnmM
    N9TbLr_E8Bg
    o_CLNIBdnD0
    x0hucPyynyM
    b6EC_dPKPw4
    X2cRUs2NjjU
    SlTZ-1Kj4pY
    fJzGysJ7daQ
    kAuK4ovbN7Q
    U_t_G_SISnQ
    FkR4r0MC4ic
    jxd5c6iq4a4
    ba-3poViAas
    1QY29wUHVWE
    v9yn1YVvIZ8
    YgovrcWAYeU
    TF3Gqftossk
    QPg81GkmX-M
    8aPHtEl1dFk
    eTj7NtgpRr4
    pJ7t_pzPy_w
    9_fQ40RFmmo
    lqKNs3L3ta4
    lCoE9IF5bSc
    r1Ig7RddCO8
    COMoVzJS_9s
    nGoF9IOqAFs
    QMd4w-AaXhQ
    I86kXAcXk14
    P4NKMVZu54A
    r3yrADqaocs
    N6R_q5XgQ5U
    GDMlDy7txWs
    sOQKbw7wSZk
    -ogTzFM_dso
    fRte_UTTKW4
    4AOOG4Y5ak0
    sjedNCyUtcI
    JJE8rioWZok
    iiAtxlHsa2U
    yKyCy_NA-Rs
    tpg4DGuqreE
    Hu7Kr-W4LGM
    Cs8tK5uxfN0
    4cZyaJCV6MA
    EJRn-H9eMLY
    TFR62WBXLeI
    I2L98JkSgEI
    mb1FDxbCT_4
    9hCxaM_bW_8
    mQkTXE2lWdw
    6YJ8ZxD-LeU
    zwjofCgVt4g
    rSFKB-5Fg10
    TUO3vnM_NDg
    3kDOMSdWNy8
    L870btp4K74
    BTXBDH-wQlA
    znudVOU18nU
    jKyV_2iJO-s
    r04ECly4Vok
    tqeZtQ4LiRg
    jGCBQR_T2b0
    CQByjgj_6MY
    02q6BlqVS6Q
    GtCr6rULXIo
    6xOuh5O3J9c
    zB6cK-N1P78
    zVW4yA4jl-k
    HkAClOmyhYM
    onRge-CxoRE
    OwYGV18N7bo
    3E-c2eXTNzo
    flZS9WON8FU
    lBmGExibHEA
    3GaoFmlOHgY
    UAFEB1YEpYg
    LKnpuEmMU2M
    KzM-cysBMTQ
    9IweKuLaF2I
    ouMQfZGShdU
    XYgRMxdzpAo
    IMTTl3wVjvE
    5s3SVID53J4
    dJtXfE5pLr8
    OWIL-nnTWqI
    B4VDL2vn5t4
    wsNpFVeYIrs
    58syYVCz-VM
    WgzFE-e5-vI
    PKca2oEtXEg
    jX99yxsIePY
    zXST_-ORl5g
    y-mO4AB5c2A
    WqIcX9RDwbY
    NGxhg9MEcpE
    FNfGpUXlyms
    KUuMHcZvzdE
    XKXSxNQkHGE
    d5y3YU3AMJk
    0oHrmc5CQY8
    mjhSw_WajnM
    EMbz1bHHPHk
    8DBT7rjtbzQ
    yvYeATPcHrg
    BG5JBVFPJeU
    lOXpxECxKjw
    9BaGIY-1KDY
    m3CytgP-3tQ
    nTdgrCsjpg8
    TIAMCrAdYWA
    1KanLUUf4aQ
    N8qZCCAbNqo
    D9V7XkAOgvU
    otsxvWjYuRo
    mMrRwREEYDE
    DrXbuzYHj3M
    BwNUn09JXeI
    tlFOga0CNfk
    incZiTyevxU
    Q1hnijlwAQc
    l-PeCDHA56Y
    Jbsbph_xb2Y
    I8KLb_7ZIhA
    pzYkVaaA-QQ
    ZeAzTMIpFd4
    7kKApmC4gfk
    fdCNS5hquzw
    SJPh7wNIFLY
    Y1NjsmW63HA
    Y2TdT3G3P6g
    L7B0YAj2FQE
    TzfZdz1FvSQ
    oCf9o39xQ7c
    bCR7F9xQ-CM
    HgwiLAw86is
    UeurIROt3fs
    IRXzK7fVl2c
    Ee48Edqvhn0
    xVVxp4xAGsY
    kStWPmHvmJ4
    cof20rW2C1k
    57NAodmifFI
    6UmplFlBRCk
    TyN11ASMX2U
    iA2SS8NwJwo
    HttG0VGNN-I
    4yooxQDvy7A
    yV-4PzrdYn8
    HNA-9PtgcNk
    9DSludGZxs0
    Isu29b48T6Y
    sFFTq2SG5EI
    qQRDA3dT7Cw
    xsS8RG-OaBQ
    V4JgwexNtCI
    PeGph0AdG34
    vYVmaIBbDCU
    u7FnB5LVnIg
    cyrk7_Lde4s
    1L-M1SJYI9o
    BnBlx3p6af4
    smSGDxb2Ox0
    l1knv9cn8M4
    F8g-UMR0ucY
    QPyDm079zSk
    eYyN1RUCZp8
    e_-t9_fNJgo
    ANev8N8wbKQ
    wpA8srR6D5E
    Kx3FNwSXU3Y
    dCRrBiILd-w
    g2xyuqN1sqQ
    1LPe89lbGZ0
    cRahF_SJqHY
    i83lA_CBPlo
    7aV9_FJa0SU
    SOfFFThjnVI
    sZGZOuhaehs
    3PQSFXK6njI
    IOgj2y5-WVA
    pWbe7VhLBz8
    YgYQDoWAnpY
    fzVERcSFIsE
    nfuR5TQOzQk
    JZv7BL12K8g
    3U8RWTyHZNY
    qnUeVL9u3hc
    2BCoI4eE4FY
    5RJwvGyP1Vg
    YMxojctbrBQ
    tp-3jyz7ft8
    5V5Air0OfCY
    I-mBqfNVcJ8
    lwUMWTCZhcg
    Y6bVW-UM9mA
    JIbx6K0tH9o
    2-Q2lvf8ZkY
    -aV0xG4xcek
    KuYmVhUn6MI
    N3dMekwUrAQ
    D9X5-4XhdXg
    26KJFynfOLQ
    _D45kex4S7U
    Ve1Ltk1yavk
    M8BdFHT2jDk
    173VQMiRJUY
    RMp4L9kQ9gQ
    XSWanuaQjwY
    gH9afuibMCM
    wO7LIVqvb18
    -V5AXl9V3EU
    ik0-YdlYkBI
    G6bnx4MyJlE
    5tjzHK2HACM
    yfmJVAaaDMU
    XYs2r9sQ_JU
    ZLdwAcsZuWo
    gNn-3L9dvWc
    hmAh7NQ6miQ
    wZXvIVesAFo
    WHhH1iDwCbY
    S_RwNwpu4tE
    gYrqs16GH0U
    bsGoHz4UbFY
    MJ7Plhx8SaA
    -9gfmpOTonw
    INTiByA10zk
    sudEXteWW5Y
    K47iVe2QVoU
    AWoRTWTVPuk
    y4OnTcPbp1U
    jVZgCNSZNx4
    oAa6kVf9NAo
    Kt7JSWGnRjY
    c3QGr18Hl94
    1QQlRQM3xXQ
    uWgr1SXAY-o
    XdUvCdXwHJU
    P_cCW7g2dlw
    LFpd3Fbw_H4
    Y_VjEiCvcVU
    tEl1V9MLvTg
    SVv9xXe-FEg
    5suEb06mLAQ
    IgqeIUcHEoM
    _W039-xMMKA
    WafmsGPbyEY
    rVGlTyj3f3k
    ff4W_Qf_C4U
    zO_n5sMirug
    rCTaqsndNEs
    fs-69ggYCjE
    Y-tt9bA62VQ
    amuOFghXFfw
    _8QJlF3nyDk
    L9RIwivnsFA
    ovlUWGXibdc
    mToeibzI4fQ
    PA8gmOEaH14
    S2fknMJwKvU
    zjAeFJAyaSU
    xN4jbIvupPA
    m1cKNhk8eM0
    KyaNtkdsBAo
    YlZirzuvaqc
    wybAAynw-60
    qdMNl0ZQSHU
    Ln571yOY50s
    ki2aQhjqeOU
    ab6mon01N1w
    yL6aB2VicjU
    b6uAQQqS7po
    9Z7Uep8vQu0
    pHnU7JjVrGM
    bnUaOq4Jrjs
    7hACV_ZvD3U
    XBGQvvvBw10
    _rgRrU8YftY
    RcSEgaStQec
    pMAOh6kHE44
    scUaVxwK1QA
    cAuoVoiMFCE
    F9yytZbAZQw
    9FiaaBptSdA
    uVfZd1uYaQ4
    yrntjKXx-5w
    k-nqTN2G8pw
    u1DsYFmlKv4
    U0SsMoccufw
    ty8Ii3RU594
    k-wgqR409f0
    piRy2TEleLM
    4XtQx1ABRsQ
    CdDI1fpxNwY
    4Qmh1d7xNSY
    uN-zBbtas6s
    gOghQLoU1NE
    LaZvEcp6qW8
    AepKJuE3NM4
    GsKdvLz10S0
    FXkVqdtc_sk
    QVzAKc7z2F0
    jP8KPNUY_Ss
    e_MyBVT9hl0
    CbCI4j_xFk0
    nFTSxafaS4A
    R9a9VestzPE
    2IJpkkq-x0w
    IJDJ-h-fCTY
    dqatQ-Di8ss
    Vwj-rXAwOsY
    itasN0L7SQ8
    pqPFNAwYGpM
    7_49XRSrESw
    _j92aIWyNhM
    ijLM_tL8DwM
    5NHeeXtkOVM
    jtVIFho9ueA
    QgdUJvdHm5s
    0RGkOaPJVHo
    jb8A0AUdVBc
    lUSrM130bO0
    NS5zLjWa66c
    lcDrAq8eREI
    iaFRwgs5nqE
    aapHRcyk6aI
    I8YWmoamSdQ
    HaUsGYk285E
    


```python
del comments_lst[17]
```


```python
comment_df = pd.DataFrame([video_full_id,comments_lst]).T
```


```python
comment_df.columns = ["video_id","comments"]
```


```python
comment_df.to_csv("D:/경돼님_api/경돼님_댓글.csv")
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
      <th>video_id</th>
      <th>comments</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>gBx2UrI4JqE</td>
      <td>[형님영상보고 5X5X5 루틴하고있는데\n5,5,5가 효과가 더있나요 아니면 많이씩...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>n2jAepfvF6c</td>
      <td>[손바닥 전체적으로 미시나요?\n자신만의 포인트좀 공유부탁드려요.\n 상당히 안정적...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>_thZKDd0NGo</td>
      <td>['진짜 운동하는 직장인', 쌀돼 경돼 갓돼, 요새는 유도 안하세요??, 진짜 강하...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>UURowG79j8c</td>
      <td>[진짜 멋있다 ㅎㅎ, 도대체이형은 몸이 왜 변하지않는거지, 경돼 가슴이 진짜 이쁘다...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0Cn9Jk0BrdE</td>
      <td>[비아냥이라니요. 지금도\n잘 하시고 있는데.. 누구 탓할\n필요도 없습니다. 본인...</td>
    </tr>
    <tr>
      <th>5</th>
      <td>QXpJ7WOF4Zo</td>
      <td>[와 몸 미쳤어요, 무한동력 왜 연구함\n경돼의 무한슈레딩이 있는데 ㅋㅋ, 경돼님 ...</td>
    </tr>
    <tr>
      <th>6</th>
      <td>w2eBt4FhFVQ</td>
      <td>[와 몸 또 만들었네요.. 대박~ 멋있어요, 경돼님 어떻게 운동을 2번이나... 회...</td>
    </tr>
    <tr>
      <th>7</th>
      <td>66z8rKLa-3Q</td>
      <td>[이런 기술은 얼마나 익혀야\n제대로 나올까요? 유단자시니까\n잘 하시는것 같아요....</td>
    </tr>
    <tr>
      <th>8</th>
      <td>JeEU0qeb-0w</td>
      <td>[저 니랩??은 혹시 어디껀지 알 수 있을까요?, 운동 좋아하는 취준생인데 보면서 ...</td>
    </tr>
    <tr>
      <th>9</th>
      <td>RodnFgR1vmg</td>
      <td>[아저씨 다됐네...., 돌아와서 기쁩니다💪💪💪💪, 경돼형님 벌써 4년가까이 구독중...</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Hb_OsRmhpuA</td>
      <td>[100키로로 근비대갯수라... 후덜덜, 그래도 여전히 몬스터이구만, 강력하다, 역...</td>
    </tr>
    <tr>
      <th>11</th>
      <td>5U0Mqr8Twto</td>
      <td>[경돼 찡 다이슼히! 항상 경돼띠 뒤에서 묵묵히 응원하는중!, 키 몸무게가 어찌 되...</td>
    </tr>
    <tr>
      <th>12</th>
      <td>gVAVBuKfya8</td>
      <td>[와 3년전에 진짜 많이 봤었는데 오랜만에 보니까 동창만난 찡한느낌..ㅠ\n행님 영...</td>
    </tr>
    <tr>
      <th>13</th>
      <td>2c_b7d62xCo</td>
      <td>[안녕하세요 유도 질문있는데요  2단을 목표로 주 4회이상  꾸준히  3년정도 다니...</td>
    </tr>
    <tr>
      <th>14</th>
      <td>v8NUx1NaIV4</td>
      <td>[와아 댓글들 정말 감사합니다...!\n앞으로 느리더라도 꾸준히 영상 업로드 할게요...</td>
    </tr>
    <tr>
      <th>15</th>
      <td>GoqO4SqYQQM</td>
      <td>[생각나서 왔습니다\n빨리 와요 \n경돼 대학교 유도 볼때까 언제적이더라 시간 잘 ...</td>
    </tr>
    <tr>
      <th>16</th>
      <td>eFeE-FZe4_M</td>
      <td>[]</td>
    </tr>
    <tr>
      <th>17</th>
      <td>QK9pqWVBfFE</td>
      <td>[동영상 다시 풀린건가요?! 제일 처음 접한 운동유투버가 경돼님이라 그런지 유난히 ...</td>
    </tr>
    <tr>
      <th>18</th>
      <td>fx46_GOo_ms</td>
      <td>[2020 썸머슈레딩은 순항 중?\n\n과연 성공할 수 있을까?!!\n\n대한득근만...</td>
    </tr>
    <tr>
      <th>19</th>
      <td>gkmfepaHejM</td>
      <td>[경돼님 올만에 영상으로 뵙네용! 반갑습니다!! 올해도 어김없이 썸머슈레딩 하시는 ...</td>
    </tr>
    <tr>
      <th>20</th>
      <td>vNZ2er_xyYQ</td>
      <td>[경돼띠 💪, 인스타부터 오래봤습니다 몸이점점좋아지시네요, 몸 엄청 이뻐지셨어요!!...</td>
    </tr>
    <tr>
      <th>21</th>
      <td>fwDjvswIso8</td>
      <td>[같이 운동해보고싶네요 ㅋㅋ 다이어트 응원합니다, 간만에 팬티짤 보니까 옛날 비포애...</td>
    </tr>
    <tr>
      <th>22</th>
      <td>ZviULrv6bMQ</td>
      <td>[아 진짜 썸띵댕져 배경 투명도 좀 올려달라구요 빨리 급해요, 경돼행님~~~~ 인바...</td>
    </tr>
    <tr>
      <th>23</th>
      <td>_q0hwb68qnQ</td>
      <td>[네모난 가빠 부럽다, 예전부터 느꼈던 거지만 경돼 키에 비해 어깨 골격이 되게 좋...</td>
    </tr>
    <tr>
      <th>24</th>
      <td>qj9YZrVF46k</td>
      <td>[와아아아, 형 이거 브금 좀 알려줘 ㅠㅠ, 어? 이거되는거였어? 용기+동기 얻고갑...</td>
    </tr>
    <tr>
      <th>25</th>
      <td>1mk-mrXFmkQ</td>
      <td>[8:39 벤치프레스 140kg 보조 없이.. .실화?!!!!!\n\n마이프로틴 대...</td>
    </tr>
    <tr>
      <th>26</th>
      <td>RN64_7xoHEw</td>
      <td>[간만에  봐도 좋네요., 알고리즘이 추천을 이젠...안해주길래 와봤는데\nstil...</td>
    </tr>
    <tr>
      <th>27</th>
      <td>KvLyGsiISkI</td>
      <td>[구독자 좀 늘었으면 좋겠네요 나름 괜찮은 유튜번데;;;, 경돼형님은 역시 경이로그...</td>
    </tr>
    <tr>
      <th>28</th>
      <td>v1e_MQN_VGU</td>
      <td>[아니 경돼형 영상을 지금봤네~ 왜 아무도말을안해줘!!!!!\n형도 주식영상 한번 ...</td>
    </tr>
    <tr>
      <th>29</th>
      <td>95HaaiPUdXw</td>
      <td>[안녕하세요 경돼입니다!\n마이프로틴 할인 진행 중입니다 아래 링크로 할인 받고 구...</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>568</th>
      <td>AepKJuE3NM4</td>
      <td>[덕분에 많이 화이팅 하고 있습니다~!!!\n부지런한 모습 보기 좋습니다~ㅎ, 크리...</td>
    </tr>
    <tr>
      <th>569</th>
      <td>GsKdvLz10S0</td>
      <td>[고립 대박이네요!자세굿, 신발 이름이 뭐예요??, 캬...  새우탕 냄새가 여기까...</td>
    </tr>
    <tr>
      <th>570</th>
      <td>FXkVqdtc_sk</td>
      <td>[키는 얼만가요? 경돼님^^, 누가 운동후에 인바디 재던가요..\n오전공복에 센터가...</td>
    </tr>
    <tr>
      <th>571</th>
      <td>QVzAKc7z2F0</td>
      <td>[대박이 주인왔다구 좋아하는 모습 귀엽네요~ㅋㅋ\n(던킨은 사랑이죠~ㅎㅎㅎ)\n헬스...</td>
    </tr>
    <tr>
      <th>572</th>
      <td>jP8KPNUY_Ss</td>
      <td>[형제여!..., 와몸이 가면갈수록 좋아지시네요 혹시 체지방 몇퍼에 근육량 체중 키...</td>
    </tr>
    <tr>
      <th>573</th>
      <td>e_MyBVT9hl0</td>
      <td>[바벨로우 영상 보고 싶은데... 없나요?, 어벤저스 총출동하겠네요ㅎ 잘보고 갑니다...</td>
    </tr>
    <tr>
      <th>574</th>
      <td>CbCI4j_xFk0</td>
      <td>[돼지감자가 체지방분해에 도움을준다네요ㅋㅋ정주행중에발견한 경돼님 몸매유지의 비밀, ...</td>
    </tr>
    <tr>
      <th>575</th>
      <td>nFTSxafaS4A</td>
      <td>[처음동영상부터 정주행하고있어요 좋은정보 고맙습니다 경돼님 내용도 되게 재밌는거같아...</td>
    </tr>
    <tr>
      <th>576</th>
      <td>R9a9VestzPE</td>
      <td>[경돼님! 5x5에서 증량하고 증량한후에도 갯수 세트수 내려서 5x5 성공할때까지 ...</td>
    </tr>
    <tr>
      <th>577</th>
      <td>2IJpkkq-x0w</td>
      <td>[와 자세 좋다 올릴때 팔꿈치 앞으로 하고 올리고 내릴땐 어떻게 내려와야하나요? 허...</td>
    </tr>
    <tr>
      <th>578</th>
      <td>IJDJ-h-fCTY</td>
      <td>[굳히기나 조르기는 일부로 안하시는건가요?, 경돼님  유도하면서 웨이트는 언제 하세...</td>
    </tr>
    <tr>
      <th>579</th>
      <td>dqatQ-Di8ss</td>
      <td>[맨날 소고기만 드셔 ㅋㅋ   부럽.   메치기 본, 굳히기 영상도 올려주세요.  ...</td>
    </tr>
    <tr>
      <th>580</th>
      <td>Vwj-rXAwOsY</td>
      <td>[오늘은 여기 까지 보고 내일 봐야지~!!!, 너무세게 놓아서 몸이흔들림ㅋㅋ, 데드...</td>
    </tr>
    <tr>
      <th>581</th>
      <td>itasN0L7SQ8</td>
      <td>[이야 이때 머리 너무 멋진데??, New haircut looks great~, ...</td>
    </tr>
    <tr>
      <th>582</th>
      <td>pqPFNAwYGpM</td>
      <td>[아닠ㅋㅋㅋㅋ팬티색 나랑 왜 같은거냐고 ㅋㅋㅋㅋ, 팬티색깔이 맘에 드네요, Grea...</td>
    </tr>
    <tr>
      <th>583</th>
      <td>7_49XRSrESw</td>
      <td>[와 운동도 잘하고 공부도 잘하고 이 남자 뭐지..???, 바벨을 저렇께 떨굴 수 ...</td>
    </tr>
    <tr>
      <th>584</th>
      <td>_j92aIWyNhM</td>
      <td>[쥐엔장 형!! 아가씨 앞에선 무조건 들었어야지(나도 실수로 광대뼈 부술뻔 했는데....</td>
    </tr>
    <tr>
      <th>585</th>
      <td>ijLM_tL8DwM</td>
      <td>[음~~~사람냄새나는 브이로그 너무 좋아, 경돼님 해병대이신가요, Solid for...</td>
    </tr>
    <tr>
      <th>586</th>
      <td>5NHeeXtkOVM</td>
      <td>[와 쎄다...., 언제나 맛있는 것만 드시는 것 같아요~ㅋㅋ\n하다 보면 잘 안될...</td>
    </tr>
    <tr>
      <th>587</th>
      <td>jtVIFho9ueA</td>
      <td>[축제 안즐기는 거야?? 역시 운동밖에 모르는 아싸경돼띠~~, 상세한 설명 도움되고...</td>
    </tr>
    <tr>
      <th>588</th>
      <td>QgdUJvdHm5s</td>
      <td>[와 경희대 헬스장 시설 미쳤다, 경돼님 영상보면서 처음으로 클린한 음식 보네요~ㅋ...</td>
    </tr>
    <tr>
      <th>589</th>
      <td>0RGkOaPJVHo</td>
      <td>[아니 마지막에 상대는 불곰이자나..., 유도 경기 재밌네요~ㅎㅎ\n(마지막 체급은...</td>
    </tr>
    <tr>
      <th>590</th>
      <td>jb8A0AUdVBc</td>
      <td>[벗윙크 생기는데 가동범위 훈련하고 풀스퀏 할게요, 스쿼트할때 하고 데드할때 굳이 ...</td>
    </tr>
    <tr>
      <th>591</th>
      <td>lUSrM130bO0</td>
      <td>[댓글창 무엇?ㅋㅋㅋ경우형 월클이네, The Kyung hee campus is r...</td>
    </tr>
    <tr>
      <th>592</th>
      <td>NS5zLjWa66c</td>
      <td>[Aㅏ 난 데드나 스쿼트 하고 레그컬 했는데 반대로 해야겠네요, 6:38 매의 눈,...</td>
    </tr>
    <tr>
      <th>593</th>
      <td>lcDrAq8eREI</td>
      <td>[등 운동 진짜 많이 하네, 갓돼님 영상 정주행중입니다 ㅋㅋ, 영상 재밌네요ㅎㅎ 처...</td>
    </tr>
    <tr>
      <th>594</th>
      <td>iaFRwgs5nqE</td>
      <td>[처음 인사할때 빤쓰 차림로 찍었네 ㅋㅋㅋㅋㅋ, 와우.. 저는 Tom의 제자입니다 ...</td>
    </tr>
    <tr>
      <th>595</th>
      <td>aapHRcyk6aI</td>
      <td>[저 젊다...지금은 아저씨 뻘인데ㅋㅋ, 경돼 1화부터 다시 정주행중!!, 헐 옛날...</td>
    </tr>
    <tr>
      <th>596</th>
      <td>I8YWmoamSdQ</td>
      <td>[지린다....., 자세 제대로 찍어주지도않고 뭐지, 풋풋하네요ㅋㅋ, ㅋㅋㅋ풋풋, ...</td>
    </tr>
    <tr>
      <th>597</th>
      <td>HaUsGYk285E</td>
      <td>[전설의 시작 ㄷㄷ 정주행 시작합니다, 2019년에 보는사람?!?!?!, 새마음으로...</td>
    </tr>
  </tbody>
</table>
<p>598 rows × 2 columns</p>
</div>


