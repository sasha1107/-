# 미세먼지 데이터 분석

## 데이터셋
- Airkorea 전국 대기 환경 자료
- 기간 : 2001년 ~ 2021년
- 출처 : [한국환경공단 Airkorea](https://www.airkorea.or.kr/web/last_amb_hour_data?pMENU_NO=123).

## 분석 과정
```
가설: 서울 지역의 미세먼지 수치가 전국에서 가장 높을 것이다.
```
### 1. 측정 지역(도/시) 별 월평균 데이터 프레임으로 가공
- 결측치(-999)를 null 값으로 변환 (월별 평균 계산을 위해)
- 측정일시를 year/month/day로 나누어 개별 column으로 정리
- ‘주소’ 열을 슬라이싱하여 ‘시도명’과 ‘시군구명’으로 분리
- 측정소별(측정소코드 기준) 월별 평균으로 취합
- null로 변환했던 결측치를 측정소별 월별 평균으로 치환
- 특별,광역시를 따로 분류하여 ‘시군구명’ column 제거 후 병합
- CVS 파일로 내보내기 ([📄/월평균_데이터_분석.ipynb](https://github.com/sasha1107/airkorea-analysis/blob/main/%EC%9B%94%ED%8F%89%EA%B7%A0_%EB%8D%B0%EC%9D%B4%ED%84%B0_%EB%B6%84%EC%84%9D.ipynb).)
<br><img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/4a8b0377-1ea4-4185-af8a-6e7f692805d6/%E1%84%80%E1%85%B3%E1%84%85%E1%85%B5%E1%86%B7.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220928T172137Z&X-Amz-Expires=86400&X-Amz-Signature=09a2dd8348dcea8a18154c25d1883361e962fccbeeada331eafb075d02a89893&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22%25E1%2584%2580%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25B7.png%22&x-id=GetObject" width="25%" alt=""></img><br/>

<br>

### 2. 측정 지역(도/시) 별 월평균을 연평균 데이터로 취합
- 해당 년도에 해당하는 데이터를 모두 읽어온다.
- 지역별 평균값을 구해 CVS로 다시 내보낸다. ([📄/연평균_데이터_분석.ipynb](https://github.com/sasha1107/airkorea-analysis/blob/main/%EC%97%B0%ED%8F%89%EA%B7%A0_%EB%8D%B0%EC%9D%B4%ED%84%B0_%EB%B6%84%EC%84%9D.ipynb).)
<br><img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/89c17c02-2206-46e9-81e8-4aeafb8fdf2c/%E3%85%87%E3%85%87.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220928T172323Z&X-Amz-Expires=86400&X-Amz-Signature=dcb36e13e17e2b6445f8e76ea44051cc1e345d7344df43b54fd5402cbd3af023&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22%25E3%2585%2587%25E3%2585%2587.png%22&x-id=GetObject" width="20%" alt=""></img><br/>

<br>

### 3. 연평균 데이터 시각화
- 전국 지역의 연평균 데이터를 내림차순으로 정리
- ggplot으로 시각화를 시도했지만 차트 내 한글깨짐 현상을 해결하지 못해 pyplot으로 시각화
<br><img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/608f043a-dfbc-455c-8d9b-fc824d163607/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220928T172910Z&X-Amz-Expires=86400&X-Amz-Signature=d20e38dc1871449fd31d889ecd1525750449a5a77ab481f4a2d89fef2f400027&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject" width="100%" alt=""></img><br/>

- 지역수가 너무 많아 가독성이 나쁘므로 값이 큰 순서로 10개 지역만 추출하여 다시 시각화
<br><img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/aa76f4b0-39ab-4746-89a8-8fe2fcc10647/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220928T173221Z&X-Amz-Expires=86400&X-Amz-Signature=2eae411b39930ce80c7db28a60287f02a4c01b28193ad017a693d2ee8164ed0d&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject" width="45%" alt=""></img> <img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/3e1b038a-2dab-4a8c-9c15-8ad410e50a6e/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220928T173504Z&X-Amz-Expires=86400&X-Amz-Signature=dee8e96f7c20145466cdde9e2200612c4da344d06c343e7a9b234b3858a131b2&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject" width="45%" alt=""></img><br/>


<strong>-> 2000년대 초반에는 경기도 남서쪽 지역이 미세먼지 수치가 높게 나타났다.</strong><br>
<strong>-> 서울도 10위권 안에 있기는 하지만 가장 높진 않았다.</strong><br>

<br><img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/2d82b903-0cd2-416f-b615-a91248b2f9d4/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220928T173758Z&X-Amz-Expires=86400&X-Amz-Signature=0c4a6fc41025f6e62e0cce55f9f742aa2ba4e914abf8bba44a40b29b8877f856&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject" width="45%" alt=""></img> <img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/90cf521e-f5d1-4b9f-8a1e-87bf1c73b878/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220928T174001Z&X-Amz-Expires=86400&X-Amz-Signature=1926ebbc258c70a2ac47b5064bf8a944e46bf6eda74da8723dc7d76641dd6613&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject" width="45%" alt=""></img><br/>
<img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/92c3b2c9-e278-4bd8-9686-792154767cb0/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220928T174043Z&X-Amz-Expires=86400&X-Amz-Signature=db1434a9c1ca98ee1bcbc895e35d0f34d19175426984c57ce6fdef75666479ac&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject" width="45%" alt=""></img><br/>

<strong>-> 근 3년간 미세먼지 수치 분석 결과</strong><br>
<strong>-> 충청남도 당진시의 미세먼지 수치가 눈에 띄게 높게 나타났다.</strong><br>
<strong>-> 오히려 서울은 10위권 밖에 있는 것으로 보인다.</strong><br>



# 결론
```
미세먼지 절감 대책으로 교통 정책(ex. 자동차 운행 줄이기)을 실행했던 것으로 미루어 보아
가장 많은 교통량이 있는 서울이 미세먼지가 가장 많을 것으로 추론해보았으나 그렇지 않았다.
당진시의 높은 수치에 대해 알아본 결과, 이는 대규모 철강 공장들로 인한 결과로 보인다. 일부
공장에서는 날림먼지(오염먼지)를 대기 중에 배출한 것이 적발되어 벌금을 물기도 했다. 
미세먼지로 인해 당진시를 떠나는 거주민들도 많아 대규모 공장에 대한 규제 강화 등 인근 지역
의 미세먼지 절감 대책에 대한 논의가 빠르게 이루어져야 한다. 
```
