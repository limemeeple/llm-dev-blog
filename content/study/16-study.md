---
title: "16. HuggingFace"
date: 2026-09-01
draft: false
tags: ["huggingface", "", ""]
categories: ["STUDY"]
summary: "이어드림2026 서비스 개발 수업 정리"
weight: 16
---
### HuggingFace란?
Hugging Face는 머신러닝 모델과 데이터셋을 올리고 받아 쓰는 플랫폼이다.

'AI계의 GitHub'라는 비유가 있다.

AI 모델을 직접 만들려면 대량의 데이터, GPU 그리고 몇 주에서 몇 달의 학습 시간이 필요하다. 그런데 이런 자원과 시간은 개인 개발자가 감당할 수 없기 때문에 Hugging Face는 이미 학습된 모델을 제공한다.

구글, 메타, 마이크로소프트 그리고 많은 연구자들이 학습시킨 모델을 무료로 공개해뒀고 코드 몇 줄이면 개인 사용자의 컴퓨터에서 돌릴 수 있다.

#### custom model head 완성
```
"""
    사전학습모델은 본체(Backbone)와 헤드(Head)로 구분된다.
    Backbone - 문장을 이해하고 임베딩하는 부분을 담당
    Head - 받은 임베딩 내용을 우리가 원하는 최종 결과로 바꿔주는 부분
"""
import torch

# nn : 신경망 계층(Linear, ReLU 등)이 들어 있는 모듈
# no_grad : 기울기 계산을 끄는 도구. 아래에서 추론할 때 쓴다.
from torch import nn, no_grad

# PreTrainedModel   : 내 모델을 Hugging Face 규격에 맞추기 위한 부모 클래스
# AutoModel         : backbone(본체)을 불러오는 클래스
# AutoTokenizer     : 텍스트를 숫자로 바꾸는 도구
# AutoConfig        : 모델 설계도(config.json)를 불러오는 클래스
from transformers import PreTrainedModel, AutoModel, AutoTokenizer, AutoConfig


# 1단계. 커스텀모델 만들기
class CustomClassifier(PreTrainedModel):

    def __init__(self, config): #생성자(클래스가 객체화 될때 가장먼저 실행)
        super().__init__(config) # 부모 초기화를 위해 전달
        
        #(1) backbone 생성
        # config 가 뭐죠? config.json(모델설계도)
        # from_config : 설계도만 보고 빈 모델을 만든다.
        self.backbone = AutoModel.from_config(config)

        # backbone 이 출력하는 벡터 차원 알아내기
        # 숫자를 config에서 꺼내는 이유는 모델을 다른 것으로 바꿔도 문제 없이 돌릴 수 있기 때문
        hidden_size = config.hidden_size # 768 차원

        # (2) 커스텀헤드 만들기 - 받아온 벡터들을 원하는 결과로 추출
        # 영화 리뷰를 가지고 긍정/부정 등을 구분하는 것을 할 예정
        self.custom_head = nn.Sequential(                           # sequential : 안에 넣은 층들을 위에서 아래로 순서대로 통과 시킨다.
            nn.Linear(hidden_size,hidden_size//2), # 768 -> 384     # Linear : 입력 벡터에 가중치를 곱하고 더하는 층
            nn.ReLU(),                                              # 활성함수. 음수는 0으로, 양수는 그대로 비선형성을 넣어주는 역활 
            nn.Dropout(0.3),                                        # 30% 무작위 끄기(전교1등 재우기), 과적합을 막는 장치
            nn.Linear(hidden_size//2,2) #384 -> 2                 # 최종 출력 2개 = 부정/긍정 두 클래스
        )

        self.post_init() # 가중치 초기화

    # model() 하면 forawrd() 가 실행 된다.
    def forward(self,input_ids, attention_mask=None):
        
        # 1. backbone 에 입력을 넣어서 결과를 받는다.
        outputs = self.backbone(input_ids=input_ids, attention_mask=attention_mask)
        print(f'outputs shape : {outputs.last_hidden_state.shape}') #[문장수,토큰수,벡터수]
        
        # [CLS] - 시작토큰 (해당 문장에 대표되는 내용을 담고 있다.)
        # 슬라이싱 '[:,0,:] ' 의 의미
        # ':' -> 모든 문장
        # '0' -> 0번째 토큰 = [CLS]
        # ':' -> 벡터 768차원 전부
        # 결과 shape:[2,768] - 문장당 대표 벡터 하나씩
        cls_vec = outputs.last_hidden_state[:,0,:] 
        print(f'[CLS] vector : {cls_vec}')
        
        # 2. 커스텀헤드에 보내서 최종 결과값을 받아낸다.
        print(f'[cls] vector : {cls_vec}')
        
        # 3. 결과값 반환
        # 반환값은 logit이라 부르면 아직 확률이 아닌 날것의 점수다.
        return  self.custom_head(cls_vec)  # shape: (배치크기, num_labels)

# 2단계 : 토크나이저와 모델 준비
model_id = "distilbert-base-uncased"
# 토크나이저는 모델과 반드시 짝이 맞아야하기 때문에 같은 model_id를 쓴다.
tokenizer = AutoTokenizer.from_pretrained(model_id)

# 해당 모델의 설계도를 불러옴
config = AutoConfig.from_pretrained(model_id)
model = CustomClassifier(config) # 해당 설계도 전달

# 3 단계 : 동작 확인
sentences = [
    "I really loved this movie, it was fantastic!",
    "This was a waste of time, I hated it.",
]

# padding=True : 짧은 문장에 0을 채워 길이를 맞춘다
# truncation=True : max_length를 넘으면 잘라낸다.
# max_length=128 : 최대 토큰 수
# return_tensors="pt" : 결과를 PyTorch 텐서로 반환("tf"면 TensorFlow, "np"면 numpy)
# 반환값에 input_ids와 attention_mask가 들어 있다.
inputs = tokenizer(sentences,padding=True,truncation=True,max_length=128, return_tensors="pt")
# print(inputs)

# 검증모드로 변경
model.eval() # 학습 과정은 필요없이 추론 결과만 보고자 할 때

with no_grad():
    # 기울기 계산을 하지 않음. 학습이 아니라 추론이므로 필요 없음.
    logit = model(inputs['input_ids'],inputs['attention_mask'])
print(f'model  출력 : {logit}')

# dim=0 세로 방향을 따라 연산
# dim=1/-1 가로 방향을 따라 이동
probs = torch.softmax(logit, dim=-1)        # softmax : 점수들을 0~1 사이 값으로 바꾸고 합이 1이 되게 만든다.
print("\n확률로 변환 (softmax):\n", probs)

# 가장 확률이 높은 클래스를 예측 결과로 선택
predictions = torch.argmax(probs, dim=-1)   # argmax : 최댓값 자체가 아니라 최댓값의 인덱스(위치)를 반환한다.
print("\n예측 클래스 (0=부정, 1=긍정):\n", predictions)

save_path = "./my_custom_model"
model.save_pretrained(save_path)
tokenizer.save_pretrained(save_path)
```
#### Backbone과 Head로 나누는 이유
- Backbone은 누가 만들어도 비슷하게 쓸 수 있음.
- Head는 내 문제에 따라 달라짐
- 같은 Backbone에 다른 Head를 붙이면 전혀 다른 일을 하는 모델이 된다.
```
"분위기가 좋았고 음식이 맛있었다."
      ↓
   Backbone          ← 언어를 이해하는 부분. 학습에 수백만 달러가 든다
      ↓
   [768차원 벡터]     ← 문장의 의미가 숫자로 압축된 것
      ↓
   Head              ← 그 의미를 내 문제에 맞게 변환. 작고 가볍다
      ↓
   [부정 0.1, 긍정 0.9]
```
#### 임배딩이란?
- ***[CLS]vector 로 뽑아낸 768개의 숫자가 임베딩임.
- 단어나 문장의 의미를 좌표로 표현한 것이라고 보면 됨.
- 의미가 비슷한 문장은 좌표상 가까운 위치에 놓인다.
- 768개는 모델 설계자가 정한 값이고 정답이 아니다. 클수록 표현력이 좋지만 무거워진다.
```
"재밌었다"   → [0.8, -0.2, 0.5, ...]
"즐거웠다"   → [0.7, -0.1, 0.6, ...]   ← 가까움
"지루했다"   → [-0.6, 0.3, -0.4, ...]  ← 멀음
```

#### [CLS] 토큰
BERT 계열 모델은 입력 앞에 '[CLS]'라는 특수 토큰을 자동으로 붙임.
```
입력:  "I loved it"
실제:  [CLS] i loved it [SEP]
```
이 토큰은 원래 아무 의미가 없는 빈칸인데 학습 과정에서 '문장 전체를 요약하는 자리'로 훈련.

'outputs.last_hidden_state[:, 0, :]'에서 '0'이 이 토큰의 위치다.

#### Shape 읽는 습관
AI 코드에서 가장 많이 만는 에러가 shape 불일치라고 한다. 

```
입력 문장 2개
  ↓ tokenizer
input_ids           [2, 14]         문장 2개 × 토큰 14개
  ↓ backbone
last_hidden_state   [2, 14, 768]    토큰마다 벡터 768개
  ↓ [:, 0, :]
cls_vec             [2, 768]        문장마다 대표 벡터 1개
  ↓ Linear(768→384)
                    [2, 384]
  ↓ Linear(384→2)
logit               [2, 2]          문장마다 점수 2개
```

'print(x.shape)'를 중간중간 찍어보는건 이 분야에서 아주 일반적인 디버깅 방식이다.
----------------------
#### 데이터셋 다루기
```
# uv : pip보다 훨씬 빠른 파이썬 패키지 설치 도구
#uv pip install datasets
from os import truncate

# datasets : Hugging Face의 데이터셋 전용 라이브러리.
#            transformers와 별개 패지라 따로 설치 해야 함.
from datasets import load_dataset
from transformers import AutoTokenizer

# 데이터를 어떤 모델이 읽을 형태로 바꿀지 결정하는 도구.
# 나중에 학습시킬 모델과 반드시 짝이 맞아야 한다.
tokenizer = AutoTokenizer.from_pretrained('bert-base-uncased')

def token_func(ds):
    # print(ds)
    
    # truncation=True : 128토큰을 넘으면 뒤를 잘라낸다. 
    return tokenizer(ds['text'],truncation=True,max_length=128)

if __name__ == '__main__': # 이름이 메인 이라면 실행 해라
    # 병렬처리를 할때는 이 내용 자체를 메인 스레드가 실행하도록 설정해줘야 한다.
    # 1. HF 에서 데이터셋 불러오기
    dataset = load_dataset('stanfordnlp/imdb', split='train')
    # print(dataset)

    # 2. 데이터 전처리
    # map : 모든 샘플에 함수를 적용해 '새 데이터셋'을 만든다. 원본은 그대로 두고 결과를 반환.
    encoded_ds = dataset.map(
        token_func, # 해야할 일
        batched=True, # 특정 단위로 작업을 몰아서 처리
        num_proc=4, # 사용할 스레드 수
        remove_columns=['text'], # 불필요한 컬럼 삭제
    )
    print('전처리 완료 : ',encoded_ds)

    # filter : 특정 조건의 샘플만 가져오도록
    # item => item.label == 1
    pos_ds = encoded_ds.filter(

        # 반환값이 True인 샘플만 남긴다.
        lambda item : item['label'] == 1,
        num_proc=4
    )
    print(f'filtering : {pos_ds}')

    # map 을 가지고 토큰수를 length 로 추가 한다.
    # item=> {'length' : len(item['input_ids'])}
    pos_ds = pos_ds.map(
        lambda item: {'length' : len(item['input_ids'])},
        num_proc=4
    )
    print(f'pos_ds : {pos_ds}')

    # lengh 를 기준으로 sort
    # 특정 컬럼 기준 정렬. reverse=True면 내림차순이다.
    sorted_ds = pos_ds.sort("length",reverse=True)

    # select() : 특정갯수 n 개만 가져온다.
    top10_df = sorted_ds.select([0,1,2,3,4,5,6,7,8,9]) # range(10)

    # enumerate : 순번과 값을 함께 꺼내는 파이썬 내장 함수.
    for i,item in enumerate(top10_df):
        print(f'[{i}] : 토큰길이 : {item['length']} / Label:{item['label']}')
```
#### datasets 라이브러리는 왜 따로 있나.
- pandas로 CSV를 읽으면 되지 않을까? 생각할 수 있으나 AI 데이터는 규모가 다름. 그 많은 데이터셋을 메모리에 다 올릴 수 없다.
- datasets는 Apache Arrow라는 형식을 쓴다. 핵심은 디스크에 둔채로 필요한 부분만 읽는것이다.
- 메모리 매핑(memory-mapping)이라고 부르는 방식이다.

#### 캐싱 - 두 번째 실행이 빠른 이유
- map의 결과는 디스크에 자동 저장된다. 같은 함수에 같은 데이터일 경우 다시 계산하지 않고 캐시에서 읽는다.
- 주의할 점은 함수 내용을 바꾸면 캐시도 갱신된다. 함수를 해시해서 비교하기 때문.
- 반대로 함수는 그대로지만 결과가 이상하면 캐시가 오랜된 것일 수 있기 때문에 'load_from_cache_file=False'를 주거나 캐시 폴더를 비우면 된다.

#### batched = True가 중요한 이유
- 토크나이저는 Rust로 구현되어 있고, 여러 문장을 한 번에 처리할 때 내부적으로 병렬화 된다.
| |처리방식|속도|
|---|---|---|
|batched=False|함수를 2.5만 번 호출|느림|
|batched=True|함수를 25번 호출(1000개씩)|수십 배 빠름|


대신 함수가 받는 인자 모양이 달라진다.
```
# batched = False
def f(item):
    return tokenizer(item['text'])  # text가 문자열 하나

# batched = True
def f(batch):
    return tokenizer(batch['text']) # text가 문자열 리스트
```
----------
#### 데이터 업로드
```
from datasets import load_dataset

# split='train[:200]' : train 의 0~199 까지만 가져와라
# 슬라이스 문법을 문자열 안에 쓰는 방식이다.
#   'train[:200]'       앞에서 200개
#   'train[200:400]'    201~400번째
#   'train[:10%]'       앞에서 10%
#   'train+test'        두 split을 합쳐서
dataset = load_dataset('cornell-movie-review-data/rotten_tomatoes',split='train[:200]')

# label, text
# 1. label = 0,1 -> negative, positive
"""
def add_label_text(item):
    if item['label'] == 1:
        item['label_text'] = 'positive'
    else:
        item['label_text'] = 'negative'
    # return item['label_text'] = 'positive' if item['label'] == 1 else "negative"
    return item
"""

ds = dataset.map(
    lambda item: {'label_text': 'positive' if item['label'] == 1 else "negative"}
)

# 2. text -> review  컬럼명 변경
# 컬럼 이름만 바꾸고 내용은 그대로 둔다.
ds = ds.rename_column('text','review')

# 3. 10자 미만의 리뷰는 거른다.
# (item) => len(item['review']) >= 10
# 여기서 len()은 '글자 수'를 센다. '토큰 수'가 아니다.
ds = ds.filter(lambda item: len(item['review']) >= 10)

print(f'columns : {ds.column_names}')
print(f'data : {ds[0]}')

# HF 에 업로드
# hf auth login --force
REPO_ID = 'jihookuku/upload_test_ds'

# 데이터를 parquest 형식으로 변환해 업로드한다.
# 저장소가 없으면 자동으로 만들어 준다.
# split="train" : 이 데이터가 train 분할이라고 표시해둔다.
                  나중에 test를 따로 올리면 한 저장소에 두 split이 들어간다.
# 기본은 공개(public) 저장소디. private=True를 주면 비공개가 된다.
ds.push_to_hub(REPO_ID,split="train")
print(f'업로드 완료 : https://huggingface.co/datasets/{REPO_ID}')

# DOWNLOAD
# dataset = load_dataset(REPO_ID,split='train')
# print(dataset)
```
#### Hub는 모델만 올리는 곳이 아니다.
- Hugging Face에는 세 종류의 저장소가 있습니다.
|종류|URL|올리는 것|
|---|---|---|
|모델|/모델이름|가중치, config|
|데이터셋|/datasets/이름|데이터 파일|
|Spaces|/spaces/이름|데모 앱 코드|


전부 git 기반이고 버전 관리가 된다.

#### Parquet 형식
- 'push_to_hub'는 데이터를 CSV나 JSON이 아니라 Parquet으로 변환해 올린다.
- 열 단위로 저장하는 형식이라 압축률이 좋고, 특정 컬럼만 읽을 때 빠르다. 데이터 분야에서 널리 쓰이는 표준이다.

--------
#### 검증방법
```
# uv pip install evaluate scikit-learn
# evaluate      :   Hugging Face의 평가지표 전용 라이브러리
# scikit-learn  :   실제 계산은 이 라이브러리에서 한다. 그래서 둘다 설치해야 accuracy가 돌아간다.
import evaluate
from markdown_it.rules_block import reference

# 1. 평가지표 로딩
# 문자열 이름으로 지표를 불러온다.
acc = evaluate.load('accuracy')

# 2. 예측값과 정답을 주고 결과 계산
# 이름 그대로 predictions는 모델이 내놓은 답, references는 정답지다.
# 두 리스트의 길이가 반드시 같아야 한다.
result = acc.compute(
    predictions=[0,1,1,0], # 예측 값들
    references = [0,1,0,0] # 정답
)
print(result)
```
#### 평가는 학습과 별개의 단계다.
- 모델을 만들면 얼마나 잘하는지 숫자로 확인해야 한다. 그 숫자를 지표(metric)라고 한다.

#### accuracy는 가장 단순한 지표다.
```
accuracy = 맞힌 개수 / 전체 개수
```
- 직관적으로 확인할 수 있으나 데이터가 한쪽으로 치우쳐져 있을 경우 무의미하다.
- accuracy외 다른 지표들도 있다.
|지표|묻는 질문|
|---|---|
|accuracy|전체 중 몇개를 맞혔나|
|precision|'맞다'고 한것 중 실제로 맞는 비율|
|recall|실제 정답 중 몇개를 찾아냈나|
|f1|precision과 recall의 조화 평균|

#### 여러 지표를 한번에 쓰는법
```
clf_metrics = evaluate.combine(['accuracy', 'f1', 'precision', 'recall'])

result = clf_metrics.compute(
    predictions = [0, 1, 1, 0],
    references = [0, 1, 0, 0]
)
```
'combine'으로 묶으면 한번에 계산된다.