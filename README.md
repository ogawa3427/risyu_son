# risyu-son(第一回)
CASるIT班で行うrisyu-son(Ogawa作のrisyu_apiを使ったハッカソン)のためのリポジトリです  
APIの使い方やサンプルコードを載せていきます

## APIの使い方
GETリクエストを使います

### 試しに叩いてみる
```bash
curl -X 'GET' 'https://kurisyushien.org/api&mode=hackathon'
```

こんな感じでURLのクエリを使って操作していきます
### 基本モード
リアルタイム情報(ということにしているもの)を取得します
#### req  
`https://kurisyushien.org/api&mode=hackathon`(wordは任意)  
#### res 
```json
{
  "message": "thanks for participating in the hackathon",
  "operation type": "realtime"
  "status": "success"
  "data": [
    [
        "timestamp1",
        "時間割番号1",
        "適正人数1",
        "総登録者1",
        "優先指定1",
        "第一希望1",
        "第二希望1",
        "第三希望1",
        "第四希望1",
        "第五希望1"
    ],
    [
        "timestamp2",
        "時間割番号2",
        "適正人数2",
        "総登録者2",
        "優先指定2",
        "第一希望2",
        "第二希望2",
        "第三希望2",
        "第四希望2",
        "第五希望2"
    ],
    [
        "timestampn",
        "時間割番号n",
        "適正人数n",
        "総登録者n",
        "優先指定n",
        "第一希望n",
        "第二希望n",
        "第三希望n",
        "第四希望n",
        "第五希望n"
    ]
  ]
}
```

### 時刻指定モード
yyyyMMddHHmmss形式の**12文字のキー**で時刻を指定して情報を取得します  
202404010715から202404032357までおおむね1分間隔で取得できます
#### req  
`https://kurisyushien.org/api&mode=search&word=202404010715`  
#### res 
```json
{
  "message": "found by yyyyMMddHHmm",
  "operation type": "search",
  "status": "success",
  "data": [
    [
        "timestamp1",
        "時間割番号1",
        "適正人数1",
        "総登録者1",
        "優先指定1",
        "第一希望1",
        "第二希望1",
        "第三希望1",
        "第四希望1",
        "第五希望1"
    ],
    [
        "timestamp2",
        "時間割番号2",
        "適正人数2",
        "総登録者2",
        "優先指定2",
        "第一希望2",
        "第二希望2",
        "第三希望2",
        "第四希望2",
        "第五希望2"
    ],
    [
        "timestampn",
        "時間割番号n",
        "適正人数n",
        "総登録者n",
        "優先指定n",
        "第一希望n",
        "第二希望n",
        "第三希望n",
        "第四希望n",
        "第五希望n"
    ]
  ]
}
```

### 時間割番号指定モード
時間割番号を指定して情報を取得します  
一意に決まらなかった場合は決まらなかった場合は該当する時間割番号をすべて返します
#### req(一意に定まる場合)
`https://kurisyushien.org/api&mode=search&word=74E01.101`  
#### res(一意に定まる場合)
```json
{
  "message": "found by class code",
  "operation type": "search",
  "status": "success",
  "data": [
    [
        "timestamp1",
        "時間割番号1",
        "適正人数1",
        "総登録者1",
        "優先指定1",
        "第一希望1",
        "第二希望1",
        "第三希望1",
        "第四希望1",
        "第五希望1"
    ],
    [
        "timestamp2",
        "時間割番号2",
        "適正人数2",
        "総登録者2",
        "優先指定2",
        "第一希望2",
        "第二希望2",
        "第三希望2",
        "第四希望2",
        "第五希望2"
    ],
    [
        "timestampn",
        "時間割番号n",
        "適正人数n",
        "総登録者n",
        "優先指定n",
        "第一希望n",
        "第二希望n",
        "第三希望n",
        "第四希望n",
        "第五希望n"
    ]
  ]
}
```

#### req(一意に定まらない場合)
`https://kurisyushien.org/api&mode=search&word=74E01`  
#### res(一意に定まらない場合)
```json
{
    "message": "multiple classes found, please specify one",
    "operation type": "search",
    "status": "redirect",
    "data": [
        "74E01.101",
        "74E01.102",
        "74E01.103",
        "74E01.104"
    ]
}
```

### 科目詳細モード
時間割番号から科目名や担当教員などの詳細情報を取得します  
これは該当部分のある項目をすべて返します  
短すぎる時間割番号だと該当する項目が多いので返答できない場合があります  
#### req
`https://kurisyushien.org/api&mode=exchange&word=74E01`
#### res
```json
{
    "message": "found by class code",
    "operation type": "exchange",
    "status": "success",
    "data": [
        [
            "74E01.101",
            "ＧＳ科目",
            "グローバル時代の国際協力（英語クラス）",
            "火1",
            "渡辺　敦子",
            "全学生（共通教育科目に係る卒業要件未充足の2年以上優先）"
        ],
        [
            "74E01.102",
            "ＧＳ科目",
            "グローバル時代の国際協力",
            "火3",
            "渡辺　敦子",
            "全学生（共通教育科目に係る卒業要件未充足の2年以上優先）"
        ],
        [
            "74E01.103",
            "ＧＳ科目",
            "グローバル時代の国際協力",
            "木2",
            "渡辺　敦子",
            "全学生（共通教育科目に係る卒業要件未充足の2年以上優先）"
        ],
        [
            "74E01.104",
            "ＧＳ科目",
            "グローバル時代の国際協力",
            "木4",
            "渡辺　敦子",
            "全学生（共通教育科目に係る卒業要件未充足の2年以上優先）"
        ]
    ]
}
```

### 全項目取得モード
情報のある科目の一覧を取得します
#### req
`https://kurisyushien.org/api&mode=all`
#### res
```json
{
    "message": "all classes",
    "operation type": "all",
    "status": "success",
    "data": [
        [ "時間割番号1", "科目名1" ],
        [ "時間割番号2", "科目名2" ],
        [ "時間割番号3", "科目名3" ],
        [ "時間割番号n", "科目名n" ]
    ]
}
```

## サンプルコード(好きにコピペしていいよ)
### JavaScriptからAPIを叩くサンプルコード

[https://ogawa3427.github.io/risyu_son/fetch.html](https://ogawa3427.github.io/risyu_son/fetch.html)

[https://github.com/ogawa3427/risyu_son/fetch.html](https://github.com/ogawa3427/risyu_son/fetch.html)


### DOM操作のサンプルコード

[https://ogawa3427.github.io/risyu_son/DOM.html](https://ogawa3427.github.io/risyu_son/DOM.html)

[https://github.com/ogawa3427/risyu_son/DOM.html](https://github.com/ogawa3427/risyu_son/DOM.html)

### cookieをいい感じに扱うサンプルコード

[https://ogawa3427.github.io/risyu_son/cookie.html](https://ogawa3427.github.io/risyu_son/cookie.html)

[https://github.com/ogawa3427/risyu_son/cookie.html](https://github.com/ogawa3427/risyu_son/cookie.html)

