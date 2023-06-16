

# backend server 실행방법 및 log보는 법 팀내 교육 자료 



## backend server 실행방법 

**theme_classification** 


~~~
conda activate twodigit_theme_t5
cd /work/twodigit_theme_t5
pm2 start server.sh --name theme_classification_server
~~~

**theme_generation**

~~~
conda activate twodigit_theme_generation
cd /work/twodigit_theme_generation
pm2 start server.sh --name theme_generation_server
~~~
**file_server**
~~~
conda activate file_server
cd /work/file_server
pm2 start file_server.py --interpreter python 
~~~



## pm2 로그 형식

#### pm2 서버 로그 형식

~~~
{id}|{name}           | {date_time}                  - {debug_level}  - {debug_message}
~~~

#### 예시

~~~
11  |temp_generation  | 2023-06-16 15:24:06,963	      - DEBUG         - [OPENAPI] clustering
~~~

```
11  |temp_generation  | 2023-06-16 15:24:07,320       - DEBUG         - [OPENAPI][200 OK] [
11  |temp_generation  |   {
11  |temp_generation  |     "theme": "배터리",...
```
~~~
10|theme_generation_server  | 2023-06-16 15:33:21,767 - DEBUG - [OPENAPI][400 BAD REQUEST] {
10|theme_generation_server  |   "errors": {
10|theme_generation_server  |     "body": "Missing required parameter in the JSON body"
10|theme_generation_server  |   },...
~~~

~~~
10|theme_generation_server  | 2023-06-16 15:32:06,177 - DEBUG - generation title: 툴젠, 유전자교정 CAR-T 세포치료제 호주 이어 중국에 기술이전
~~~


## engine별, api별 log


### theme_classification server api

#### 1. /t5_inference 
​ **1-1. json 형식에 맞고, 값이 다 있는 경우**

~~~
14|theme_classification_server  | 2023-06-16 16:42:05,897|140168767222848|DEBUG|## t5_inference
14|theme_classification_server  | 2023-06-16 16:42:05,898|140168767222848|DEBUG|t5_inference title : 툴젠, 유전자교정 CAR-T 세포치료제 호주 이어 중국에 기술이전
14|theme_classification_server  | 2023-06-16 16:42:05,898|140168767222848|DEBUG|t5_inference body : (서울=뉴스1) 성재준 바이오전문기자 = 유전자교정기업 툴젠(199800)은 11일 중국 순시홀딩스그룹에 자사 카티(CAR-T) 세포치료제 'CTH-004'에 대한 중화권(중국, 홍콩, 마카오 및 대만) 지역 권리를 이전했다고 밝혔다. CTH-004는 툴젠이 2021년에 호주 카세릭스에도 기술이전했던 신약 후보 물질이다.순시홀딩스그룹은 이번 계약을 통해 중화권에서 CTH-004에 대한 권리를 갖고 난소암을 포함한 고형암 대상 임상 개발과 사업화를 진행할 예정이다. 계약에 따라 자세한 계약 내용은 공개되지 않았다.CTH-004는 유전자교정 기술을 적용한 TAG-72 표적 카티 치료제이다. 호주 카세릭스는 곧 유전자교정 카티 치료제로는 처음으로 사람 대상 임상에 들어간다. 이번 기술이전으로 중국 내 임상시험도 시작될 예정이다.
14|theme_classification_server  | 2023-06-16 16:42:05,898|140168767222848|DEBUG|inference
14|theme_classification_server  | Truncation was not explicitly activated but `max_length` is provided a specific value, please use `truncation=True` to explicitly truncate examples to max length. Defaulting to 'longest_first' truncation strategy. If you encode pairs of sequences (GLUE-style) with the tokenizer you can select this strategy more precisely by providing a specific strategy to `truncation`.
14|theme_classification_server  | 2023-06-16 16:42:06,518|140168767222848|DEBUG|[OPENAPI][200 OK] [
14|theme_classification_server  |   {
14|theme_classification_server  |     "theme": "카티",
14|theme_classification_server  |     "stock": "툴젠",
14|theme_classification_server  |     "score": -0.365
14|theme_classification_server  |   }
14|theme_classification_server  | ]

~~~

​	**1-2. json 요청값을 1개/0개만 보냈을 경우**

~~~
|server  | 2023-06-16 14:45:33,674|140710257185856|DEBUG|## t5_inference
9|server  | 2023-06-16 14:45:33,675|140710257185856|DEBUG|[OPENAPI][400 BAD REQUEST] {
9|server  |   "errors": {
9|server  |     "body": "Missing required parameter in the JSON body"
9|server  |   },
9|server  |   "message": "Input payload validation failed"
9|server  | }
~~~

​	**1-3. json 요청값이 모두있으나, title+body 길이가 5미만인 경우**

~~~
9|server  | 2023-06-16 14:46:00,861|140710257185856|DEBUG|## t5_inference
9|server  | 2023-06-16 14:46:00,862|140710257185856|DEBUG|[OPENAPI][400 BAD REQUEST] {
9|server  |   "errors": {
9|server  |     "text": "text length is too short."
9|server  |   },
9|server  |   "message": "Input payload validation failed"
9|server  | }
~~~



#### 2. /tf_inference

​	**2-1. json 형식에 맞고, 값이 다 있는 경우**

~~~
14|theme_classification_server  | 2023-06-16 16:40:12,577|140168767222848|DEBUG|[OPENAPI] tf_inference
14|theme_classification_server  | 2023-06-16 16:40:12,577|140168767222848|DEBUG|tf_inference title : 툴젠, 유전자교정 CAR-T 세포치료제 호주 이어 중국에 기술이전
14|theme_classification_server  | 2023-06-16 16:40:12,578|140168767222848|DEBUG|tf_inference body : (서울=뉴스1) 성재준 바이오전문기자 = 유전자교정기업 툴젠(199800)은 11일 중국 순시홀딩스그룹에 자사 카티(CAR-T) 세포치료제 'CTH-004'에 대한 중화권(중국, 홍콩, 마카오 및 대만) 지역 권리를 이전했다고 밝혔다. CTH-004는 툴젠이 2021년에 호주 카세릭스에도 기술이전했던 신약 후보 물질이다.순시홀딩스그룹은 이번 계약을 통해 중화권에서 CTH-004에 대한 권리를 갖고 난소암을 포함한 고형암 대상 임상 개발과 사업화를 진행할 예정이다. 계약에 따라 자세한 계약 내용은 공개되지 않았다.CTH-004는 유전자교정 기술을 적용한 TAG-72 표적 카티 치료제이다. 호주 카세릭스는 곧 유전자교정 카티 치료제로는 처음으로 사람 대상 임상에 들어간다. 이번 기술이전으로 중국 내 임상시험도 시작될 예정이다.
14|theme_classification_server  | 2023-06-16 16:40:12,578|140168767222848|DEBUG|inference
14|theme_classification_server  | 2023-06-16 16:40:12,591|140168767222848|DEBUG|[OPENAPI][200 OK] [
14|theme_classification_server  |   {
14|theme_classification_server  |     "theme": "유전자",
14|theme_classification_server  |     "stock": "툴젠",
14|theme_classification_server  |     "score": -0.43400447427293054,
14|theme_classification_server  |     "theme_count": 4,
14|theme_classification_server  |     "stock_count": 3
14|theme_classification_server  |   }
14|theme_classification_server  | ]

~~~

​	**2-2. json 요청값을 1개/0개만 보냈을 경우**

​		동일

​	**2-3. json 요청값이 모두있으나, title+body 길이가 5미만인 경우**

​		동일

#### 3. /theme_inference

​	**3-1. json 형식에 맞고, 값이 다 있는 경우**

~~~
14|theme_classification_server  | 2023-06-16 16:42:49,946|140168767222848|DEBUG|inference
14|theme_classification_server  | 2023-06-16 16:42:50,100|140168767222848|DEBUG|inference
14|theme_classification_server  | 2023-06-16 16:42:50,103|140168767222848|DEBUG|[OPENAPI][200 OK] {
14|theme_classification_server  |   "result": [
14|theme_classification_server  |     {
14|theme_classification_server  |       "theme": "유전자",
14|theme_classification_server  |       "stock": "툴젠",
14|theme_classification_server  |       "score": -0.43400447427293054,
14|theme_classification_server  |       "from": "tf"
14|theme_classification_server  |     }
14|theme_classification_server  |   ],
14|theme_classification_server  |   "t5_result": [
14|theme_classification_server  |     {
14|theme_classification_server  |       "theme": "카티",
14|theme_classification_server  |       "stock": "툴젠",
14|theme_classification_server  |       "score": -0.365
14|theme_classification_server  |     }
14|theme_classification_server  |   ],
14|theme_classification_server  |   "tf_result": [
14|theme_classification_server  |     {
14|theme_classification_server  |       "theme": "유전자",
14|theme_classification_server  |       "stock": "툴젠",
14|theme_classification_server  |       "score": -0.43400447427293054,
14|theme_classification_server  |       "theme_count": 4,
14|theme_classification_server  |       "stock_count": 3
14|theme_classification_server  |     }
14|theme_classification_server  |   ]
14|theme_classification_server  | }

~~~

​	**3-2. json 요청값을 1개/0개만 보냈을 경우**

​		동일

​	**3-3. json 요청값이 모두있으나, title+body 길이가 5미만인 경우**

​		동일

#### 4. /upload

​	**4-1. 맞는 csv를 보냈을 경우**

~~~	
9|server  | 2023-06-16 15:14:54,288|140710257185856|DEBUG|[OPENAPI] upload
9|server  | 2023-06-16 15:14:54,327|140710257185856|DEBUG|load_theme_list
9|server  | 2023-06-16 15:14:54,328|140710257185856|DEBUG|load
9|server  | 2023-06-16 15:14:55,892|140710257185856|DEBUG|load...ok
9|server  | 2023-06-16 15:14:55,893|140710257185856|DEBUG|[OPENAPI][200 OK] {
9|server  |   "result": "success"
9|server  | }
~~~
​	**4-2. 맞지 않는 csv를 보냈을 경우**
~~~
9|server  | 2023-06-16 15:15:58,919|140710257185856|DEBUG|[OPENAPI] upload
9|server  | 2023-06-16 15:15:59,028|140710257185856|DEBUG|load_theme_list
9|server  | 2023-06-16 15:15:59,028|140710257185856|DEBUG|load
9|server  | 2023-06-16 15:15:59,047|140710257185856|DEBUG|[OPENAPI][400 BAD REQUEST] {
9|server  |   "error": "list index out of range"
9|server  | }
~~~



### theme_generation server api

#### 1. /clustering
​	**1-1. 맞는 csv를 보냈을 경우**

~~~
11|temp_generation  | 2023-06-16 15:24:06,963 - DEBUG - [OPENAPI] clustering
11|temp_generation  | 2023-06-16 15:24:07,320 - DEBUG - [OPENAPI][200 OK] [
11|temp_generation  |   {
11|temp_generation  |     "theme": "배터리",
11|temp_generation  |     "count": 27,
11|temp_generation  |     "companies": [
11|temp_generation  |       {
11|temp_generation  |         "stock": "LG에너지솔루션",
11|temp_generation  |         "count": 8,
11|temp_generation  |         "objids": [
11|temp_generation  |           "id0250003263005",
11|temp_generation  |           "id0220003788337",
11|temp_generation  |           "id0200003482740",
11|temp_generation  |           "id0050001590443",
11|temp_generation  |           "id0810003343050",
11|temp_generation  |           "id3740000325080",
11|temp_generation  |           "id2770005224869",
11|temp_generation  |           "id1190002688307"
11|temp_generation  |         ]
11|temp_generation  |       },
11|temp_generation  |       {
11|temp_generation  |         "stock": "HMM",
11|temp_generation  |         "count": 6,
11|temp_generation  |         "objids": [
11|temp_generation  |           "id0250003263005",
11|temp_generation  |           "id0220003788337",
11|temp_generation  |           "id0200003482740",
11|temp_generation  |           "id0050001590443",
11|temp_generation  |           "id0810003343050",
11|temp_generation  |           "id3740000325080"
11|temp_generation  |         ]
11|temp_generation  |       },...
11|temp_generation  |     "theme": "보안",
11|temp_generation  |     "count": 3,
11|temp_generation  |     "companies": [
11|temp_generation  |       {
11|temp_generation  |         "stock": "SK스퀘어",
11|temp_generation  |         "count": 2,
11|temp_generation  |         "objids": [
11|temp_generation  |           "id0200003482753",
11|temp_generation  |           "id0050001590432"
11|temp_generation  |         ]
11|temp_generation  |       }
11|temp_generation  |     ]
11|temp_generation  |   }
11|temp_generation  | ]

~~~

​	**1-2. 맞지 않는 csv를 보냈을 경우**

~~~
11|temp_generation  | 2023-06-16 15:25:53,385 - DEBUG - [OPENAPI] clustering
11|temp_generation  | 2023-06-16 15:25:53,496 - DEBUG - [OPENAPI][400 BAD REQUEST] {
11|temp_generation  |   "error": "'result'"
11|temp_generation  | }
~~~

#### 2. /generation

​	**2-1. json 형식에 맞고, 값이 다 있는 경우**

~~~
10|theme_generation_server  | 2023-06-16 15:32:06,177 - DEBUG - [OPENAPI] generation
10|theme_generation_server  | 2023-06-16 15:32:06,177 - DEBUG - generation title: 툴젠, 유전자교정 CAR-T 세포치료제 호주 이어 중국에 기술이전
10|theme_generation_server  | 2023-06-16 15:32:06,178 - DEBUG - generation body: (서울=뉴스1) 성재준 바이오전문기자 = 유전자교정기업 툴젠(199800)은 11일 중국 순시홀딩스그룹에 자사 카티(CAR-T) 세포치료제 'CTH-004'에 대한 중화권(중국, 홍콩, 마카오 및 대만) 지역 권리를 이전했다고 밝혔다. CTH-004는 툴젠이 2021년에 호주 카세릭스에도 기술이전했던 신약 후보 물질이다.순시홀딩스그룹은 이번 계약을 통해 중화권에서 CTH-004에 대한 권리를 갖고 난소암을 포함한 고형암 대상 임상 개발과 사업화를 진행할 예정이다. 계약에 따라 자세한 계약 내용은 공개되지 않았다.CTH-004는 유전자교정 기술을 적용한 TAG-72 표적 카티 치료제이다. 호주 카세릭스는 곧 유전자교정 카티 치료제로는 처음으로 사람 대상 임상에 들어간다. 이번 기술이전으로 중국 내 임상시험도 시작될 예정이다.
10|theme_generation_server  | 2023-06-16 15:32:08,303 - DEBUG - [OPENAPI][200 OK] [
10|theme_generation_server  |   {
10|theme_generation_server  |     "idx": 0,
10|theme_generation_server  |     "result": [
10|theme_generation_server  |       {
10|theme_generation_server  |         "theme": "세포치료제",
10|theme_generation_server  |         "stock": "툴젠",
10|theme_generation_server  |         "score": -0.002
10|theme_generation_server  |       }
10|theme_generation_server  |     ],
10|theme_generation_server  |     "sentence": "툴젠, 유전자교정 CAR-T 세포치료제 호주 이어 중국에 기술이전"
10|theme_generation_server  |   },
10|theme_generation_server  |   {
10|theme_generation_server  |     "idx": 1,
10|theme_generation_server  |     "result": [
10|theme_generation_server  |       {
10|theme_generation_server  |         "theme": "바이오",
10|theme_generation_server  |         "stock": "툴젠",
10|theme_generation_server  |         "score": -0.091
10|theme_generation_server  |       },
10|theme_generation_server  |       {
10|theme_generation_server  |         "theme": "세포치료제",
10|theme_generation_server  |         "stock": "카티(CAR-T)",
10|theme_generation_server  |         "score": -0.091
10|theme_generation_server  |       }
10|theme_generation_server  |     ],
10|theme_generation_server  |     "sentence": "(서울=뉴스1) 성재준 바이오전문기자 = 유전자교정기업 툴젠(199800)은 11일 중국 순시홀딩스그룹에 자사 카티(CAR-T) 세포치료제 'CTH-004'에 대한 중화권(중국, 홍콩, 마카오 및 대만) 지역 권리를 이전했다고 밝혔다."
10|theme_generation_server  |   },
10|theme_generation_server  |   {
10|theme_generation_server  |     "idx": 2,
10|theme_generation_server  |     "result": [
10|theme_generation_server  |       {
10|theme_generation_server  |         "theme": "신약 후보 물질",
10|theme_generation_server  |         "stock": "툴젠",
10|theme_generation_server  |         "score": -0.003
10|theme_generation_server  |       },
10|theme_generation_server  |       {
10|theme_generation_server  |         "theme": "신약 후보 물질",
10|theme_generation_server  |         "stock": "카세릭스",
10|theme_generation_server  |         "score": -0.003
10|theme_generation_server  |       }
10|theme_generation_server  |     ],
10|theme_generation_server  |     "sentence": "CTH-004는 툴젠이 2021년에 호주 카세릭스에도 기술이전했던 신약 후보 물질이다."
10|theme_generation_server  |   },...
10|theme_generation_server  |   {
10|theme_generation_server  |     "idx": 7,
10|theme_generation_server  |     "result": [
10|theme_generation_server  |       {
10|theme_generation_server  |         "theme": "없음",
10|theme_generation_server  |         "stock": "없음",
10|theme_generation_server  |         "score": -10
10|theme_generation_server  |       }
10|theme_generation_server  |     ],
10|theme_generation_server  |     "sentence": "이번 기술이전으로 중국 내 임상시험도 시작될 예정이다."
10|theme_generation_server  |   }
10|theme_generation_server  | ]
~~~

​	**2-2. json 요청값을 1개/0개만 보냈을 경우**

~~~
10|theme_generation_server  | 2023-06-16 15:33:21,765 - DEBUG - [OPENAPI] generation
10|theme_generation_server  | 2023-06-16 15:33:21,767 - DEBUG - [OPENAPI][400 BAD REQUEST] {
10|theme_generation_server  |   "errors": {
10|theme_generation_server  |     "body": "Missing required parameter in the JSON body"
10|theme_generation_server  |   },
10|theme_generation_server  |   "message": "Input payload validation failed"
10|theme_generation_server  | }
~~~

​	**2-3. json 요청값이 모두있으나, title+body 길이가 5미만인 경우**

```
10|theme_generation_server  | 2023-06-16 15:33:55,760 - DEBUG - [OPENAPI] generation
10|theme_generation_server  | 2023-06-16 15:33:55,761 - DEBUG - [OPENAPI][400 BAD REQUEST] {
10|theme_generation_server  |   "errors": {
10|theme_generation_server  |     "text": "text length is too short."
10|theme_generation_server  |   },
10|theme_generation_server  |   "message": "Input payload validation failed"
10|theme_generation_server  | }
```

#### 3. /inference

​	**3-1. json 형식에 맞고, 값이 다 있는 경우**

```
10|theme_generation_server  | 2023-06-16 15:34:13,022 - DEBUG - [OPENAPI] inference
10|theme_generation_server  | 2023-06-16 15:34:13,023 - DEBUG - inference title : 툴젠, 유전자교정 CAR-T 세포치료제 호주 이어 중국에 기술이전
10|theme_generation_server  | 2023-06-16 15:34:13,023 - DEBUG - inference body : (서울=뉴스1) 성재준 바이오전문기자 = 유전자교정기업 툴젠(199800)은 11일 중국 순시홀딩스그룹에 자사 카티(CAR-T) 세포치료제 'CTH-004'에 대한 중화권(중국, 홍콩, 마카오 및 대만) 지역 권리를 이전했다고 밝혔다. CTH-004는 툴젠이 2021년에 호주 카세릭스에도 기술이전했던 신약 후보 물질이다.순시홀딩스그룹은 이번 계약을 통해 중화권에서 CTH-004에 대한 권리를 갖고 난소암을 포함한 고형암 대상 임상 개발과 사업화를 진행할 예정이다. 계약에 따라 자세한 계약 내용은 공개되지 않았다.CTH-004는 유전자교정 기술을 적용한 TAG-72 표적 카티 치료제이다. 호주 카세릭스는 곧 유전자교정 카티 치료제로는 처음으로 사람 대상 임상에 들어간다. 이번 기술이전으로 중국 내 임상시험도 시작될 예정이다.
10|theme_generation_server  | 2023-06-16 15:34:16,004 - DEBUG - [OPENAPI][200 OK] [
10|theme_generation_server  |   {
10|theme_generation_server  |     "theme": "세포치료제",
10|theme_generation_server  |     "stock": "툴젠",
10|theme_generation_server  |     "score": -0.002,
10|theme_generation_server  |     "gen_score": -0.002,
10|theme_generation_server  |     "rel_score": -0.0,
10|theme_generation_server  |     "sentence": "툴젠, 유전자교정 CAR-T 세포치료제 호주 이어 중국에 기술이전"
10|theme_generation_server  |   },
10|theme_generation_server  |   {
10|theme_generation_server  |     "theme": "바이오",
10|theme_generation_server  |     "stock": "툴젠",
10|theme_generation_server  |     "score": -0.091,
10|theme_generation_server  |     "gen_score": -0.091,
10|theme_generation_server  |     "rel_score": -0.0,
10|theme_generation_server  |     "sentence": "(서울=뉴스1) 성재준 바이오전문기자 = 유전자교정기업 툴젠(199800)은 11일 중국 순시홀딩스그룹에 자사 카티(CAR-T) 세포치료제 'CTH-004'에 대한 중화권(중국, 홍콩, 마카오 및 대만) 지역 권리를 이전했다고 밝혔다."
10|theme_generation_server  |   },...
10|theme_generation_server  |   {
10|theme_generation_server  |     "theme": "유전자교정 카티 치료제",
10|theme_generation_server  |     "stock": "카세릭스",
10|theme_generation_server  |     "score": -0.006,
10|theme_generation_server  |     "gen_score": -0.006,
10|theme_generation_server  |     "rel_score": -0.0,
10|theme_generation_server  |     "sentence": "호주 카세릭스는 곧 유전자교정 카티 치료제로는 처음으로 사람 대상 임상에 들어간다."
10|theme_generation_server  |   }
10|theme_generation_server  | ]
```

​	**3-2. json 요청값을 1개/0개만 보냈을 경우**

​		동일

​	**3-3. json 요청값이 모두있으나, title+body 길이가 5미만인 경우**

​		동일

#### 4. /relevance

​	**4-1. json 형식에 맞고, 값이 다 있는 경우**

```
10|theme_generation_server  | 2023-06-16 15:35:44,092 - DEBUG - [OPENAPI] relevance
10|theme_generation_server  | 2023-06-16 15:35:44,093 - DEBUG - relevance input1 : 툴젠, 유전자교정 CAR-T 세포치료제 호주 이어 중국에 기술이전
10|theme_generation_server  | 2023-06-16 15:35:44,093 - DEBUG - relevance input2 : (서울=뉴스1) 성재준 바이오전문기자 = 유전자교정기업 툴젠(199800)은 11일 중국 순시홀딩스그룹에 자사 카티(CAR-T) 세포치료제 'CTH-004'에 대한 중화권(중국, 홍콩, 마카오 및 대만) 지역 권리를 이전했다고 밝혔다. CTH-004는 툴젠이 2021년에 호주 카세릭스에도 기술이전했던 신약 후보 물질이다.순시홀딩스그룹은 이번 계약을 통해 중화권에서 CTH-004에 대한 권리를 갖고 난소암을 포함한 고형암 대상 임상 개발과 사업화를 진행할 예정이다. 계약에 따라 자세한 계약 내용은 공개되지 않았다.CTH-004는 유전자교정 기술을 적용한 TAG-72 표적 카티 치료제이다. 호주 카세릭스는 곧 유전자교정 카티 치료제로는 처음으로 사람 대상 임상에 들어간다. 이번 기술이전으로 중국 내 임상시험도 시작될 예정이다.
10|theme_generation_server  | Token indices sequence length is longer than the specified maximum sequence length for this model (285 > 256). Running this sequence through the model will result in indexing errors
10|theme_generation_server  | 2023-06-16 15:35:44,281 - DEBUG - [OPENAPI][200 OK] {
10|theme_generation_server  |   "result": 1,
10|theme_generation_server  |   "result_text": "related",
10|theme_generation_server  |   "score": -0.0
10|theme_generation_server  | }
```

​	**4-2. json 요청값을 1개/0개만 보냈을 경우**

​		동일

​	**4-3. json 요청값이 모두있으나, title+body 길이가 5미만인 경우**

​		동일



