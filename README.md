# char_rnn_ari
한글 Character RNN 구현

# 파일 설명
## Hangulpy/Hangulpy.py
https://github.com/rhobot/Hangulpy에서folk한 한글 관련 분해/조합 코드
오토마타, 문장 분리 코드 추가

## char_rnn_test.ipynb
테스트용 코드 (내부에 Hangulpy.py 로딩처리 추가)

## conv2utf8.py
수집한 텍스트 파일 인코딩이 제각각인 관계로 utf8로 통합 변환하는코드.
10KB 로딩 후 인코딩 추측, 컨버팅 해서 저장 하는 코드
data/origin/*.txt를 모두 처리 후 data/conv/*.txt로 저장
인코딩 추측을 위해서 chardet 라이브러리에 의존
인코딩 추측을 실패한 경우 EUC_KR로 처리 후 확장자에 ".unknown" 추가
추측 확률이 0.8 이하일 경우 확장자에 ".notsure"추가 

## deparse.py
형태소 조합 테스트를 위해서 배열에 있는 형태소 묶음을 조합해서 출력하는 코드
Hangulpy의존

## parse.py
input.txt 파일을 형태소로 분리해주는 코드 /data/conv/*.txt 모두 읽어서 분리후 출력
Hangulpy의존

## train.py
data_dir에 지정된(han1~han5중 1) 폴더를 읽어서 트레이닝 처리

# 작업 순서 

## 1. 타이핑된 책 텍스트 수동으로 모음
data/origin/*.txt 로 저장 ( 2처리 후 삭제 했음)

## 2. 인코딩 타입 utf8로 통일
python conv2utf8.py
data/conv/*.txt 로 컨버팅됨. ( 3처리 후 conv.zip으로 압축 처리했음)

## 3. 형태소 분리
python parse.py > data/index.txt
data/conv/*.txt를 모두 읽어서 형태소 분리, 한글+라틴 외에 삭제 후 출력하는 코드
stdout으로 출력하는 형태로 파일 저장을 위해서
linux shell의 output redirection을 써서 data/index.txt으로 저장.

## 4. 파일 나누기
split -l 2500000 index.txt index.
단일 파일로는 너무 커서 처리가 힘들어서 약 500메가 단위로 자름
한개의 파일로 진행하면 파일이 너무커서 tensor변환이 실패하는 관계로(8G램 맥북 기준)
han1, han2, han3, han4, han5로 분리 해서 저장.

## 5. 트레이닝
python train.py
data_dir 폴더에 지정된 폴더 내용을 읽어서 트레이닝
han1~han5 중 사용하는 한개 폴더 외에 주석 처리 상태

* input이 변경되면 단어장을 새로 만드는 형태여서 연결해서 트레이닝을 위해선 수정 필요.

## 6. 출력 내용 복원 (* 노트북의 test사용 시는 조합 된 형태로 출력 됨)
deparse.py 수정
=> data 변수에 출력된 문자 배열 저장 후 실행


# 출력 셈플 (96000 iter)

## 셈플 1
오늘은 프를 풀어 버렸다.
왕뜩의 호인이라고 내냐.
경피 그의 일문인니는 머리칼이 씨로 간줄에 힘의 부정하려는 것이 뻔했다. 일에 우리의 식구 교중사닥 섬아 먹을까지도 우승받지 않았는데 얼굴은 비롳 알아야 한다는 소리도  지긍고 재배시키다 고맙슨
을 돌 테니. 그것도 누구아였지만 신선한 목도였다.
 "좋구성유 나메일 손에 나서 강의 대산 년이구가 더 와불에 소리를 없이 날아오를  당 모르는 사람도 
사이를 이야기할 거야.' 그런데 그는
거니까 벌써 만드는 지모의 일기 구불도 호한해. 그게서도 그 거레, 이제는 갈라지게 되었기에나 사람인 집들도 깨어날 수 없는 지
고까지지 못하게도 그것이 기완이나 놀라는 교장락

이 사용하고 그렇게 어두홍록한 것만이 그건 나 빠그무하솨의 결과는 고 세상을 삼
지 못한 재회의 자유도의 추하의 신분 영태된다고 상교했습니다. 그런데 보고스덩이

숨이 사나워진 머리의 검은 잊고 있었다. 로보이의 체주수라면 구양봉의 생각의  모든 뒤 윽지해지게 깡팔이 알매져 두려왔지만 그걸 자기였다. 그는
자리에서 뜨덕으무 퀴어스 능
설과 그 대신 홀관이 짓까지 좀 허라엔이 더욱 두지 않고 가르지 않고 실장 위
중산권에 별괴인은 병들의  길껑을 하는 
인상으로 틀림없는 또나는 건 싫어 기억하면 장수였다. 나로 울려온 공곤이 손가나출하며
영화의 제자의 도생하고 있을까?
명풍보다 그 여자의 중현제가 되게 되어 있으나 회사들이 고생하로 빛살하지 않은 병땅이 생산하는 것이 아닙니다. 왜놈

## 셈플 2

오늘은 유리카의 몸이 없었다. 그녀의 친간에 따라의 행동 나는 물었다.
명훈은 각 아랫복에서는 없고 풍대 송수익을 털어서 부드러웠다. 그것도 그의 앞이 다 판격으로 녹응했
다.
황궁을 본 미치기도 하는 시작이 천괜하지 않나요" 라고 난장에
그의 결궁임 이상시()에서
약앞에서 사라버리는 것들의 목을 말한다. 그러 고두이건 명수를 가리킬 수 있었다. 고끈하는 지
못의 손목의 결호,두 한가지 안에 결코 죄용형할지도  충격을 하고 자연 열성을 느껴지올지 적전에 촉각과도
같은 주침여 집을 눈임하고 있었다. 주정의 충법이었을 때, 시고밖에 없을 뿐이었다. 냉래 금프라는 보턱에 신의
놈된다면 아주 모르겠나? 
 이야기는 농칩의 성문간에는 네가 아직도 언로집 가신 모양이든 마음의 말이 바로 이담삼 번쩍 옆에 가득이 다 받아놓고리
는 경리좀 펴날렸다. 그리고 그녀의 생물장을 만해주져요.
눈이 보그려도 꼽쳐라 고함과 혼자 이제 아무도 한 사람이었다.
커티아드는 피수처럼 놈들이 안을 잃고 만들었다. 그러면

노인은 잡히고 재갈처럼 세상의 관심을 삶을 시작하여 박육중의 내쪽도 미적지고 무엇일까요? 확생질에의 촌재심장격
에는 나아가 군 가슴 군사들은 발전까지, 사위는 용병시테에서 그대덕의 식도 사귀는 꺼번에, 너는 소매연된다면 독량
의 해질을 지니면 어디로 다니는가 본 딱  가운다. 이미 죄
처해줬다. 정말 여섯 결과가 일으키였어요.노기는 내가 만가든 자의 앞광경을 '이기 켜!]
물에 정말이라
