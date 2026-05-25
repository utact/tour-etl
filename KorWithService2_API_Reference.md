# 한국관광공사 무장애 여행 서비스 API (KorWithService2)

> **버전**: v4.3 (2025-05-12)  
> **Base URL**: `https://apis.data.go.kr/B551011/KorWithService2`  
> **응답 형식**: XML (기본) / JSON (`&_type=json` 추가)  
> **데이터 갱신**: 일 1회  
> **개발계정 트래픽**: 일 1,000건  
> **서비스 대상**: 장애인, 어르신, 영유아 동반 여행객

---

## KorService2와의 차이점

| 구분 | KorService2 (국문) | KorWithService2 (무장애) |
|---|---|---|
| 여행코스(25) | 지원 | **미지원** |
| 무장애 상세정보 | 없음 | **detailWithTour2** 추가 |
| 지역코드 방식 | 법정동 코드만 | 구 지역코드 + 법정동 코드 병행 |
| 동기화 오퍼레이션 | areaBasedSyncList2 | areaBasedSyncList2 (동일명) |

> ⚠️ 구 지역코드(`areaCode`)와 서비스 분류코드(`cat1/2/3`)는 **2025년 12월 말까지만** 지원. 이후 법정동 코드(`lDongRegnCd`) 및 신분류체계(`lclsSystm1~3`)로 대체 예정.

---

## 공통 필수 파라미터

모든 오퍼레이션에 공통으로 필요한 파라미터입니다.

| 파라미터 | 설명 | 예시 |
|---|---|---|
| `serviceKey` | 공공데이터포털 발급 인증키 | - |
| `MobileOS` | OS 구분 | `IOS` / `AND` / `WEB` / `ETC` |
| `MobileApp` | 서비스명(앱명) | `AppTest` |
| `numOfRows` | 한 페이지 결과 수 (옵션) | `10` |
| `pageNo` | 페이지 번호 (옵션) | `1` |
| `_type` | 응답 형식 (옵션) | `json` |

---

## ContentTypeId 코드표 (무장애 서비스 지원 타입)

| 관광타입 | contentTypeId |
|---|---|
| 관광지 | `12` |
| 문화시설 | `14` |
| 행사/공연/축제 | `15` |
| 레포츠 | `28` |
| 숙박 | `32` |
| 쇼핑 | `38` |
| 음식점 | `39` |

> ⚠️ 여행코스(25)는 무장애 서비스에서 제공하지 않습니다.

---

## 오퍼레이션 목록

| # | 오퍼레이션명 | 설명 |
|---|---|---|
| 1 | `areaCode2` | 구 지역코드 조회 (2025년 말 종료) |
| 2 | `categoryCode2` | 구 서비스 분류코드 조회 (2025년 말 종료) |
| 3 | `areaBasedList2` | 지역기반 관광정보 조회 |
| 4 | `locationBasedList2` | 위치기반 관광정보 조회 |
| 5 | `searchKeyword2` | 키워드 검색 조회 |
| 6 | `detailCommon2` | 공통정보 조회 |
| 7 | `detailIntro2` | 소개정보 조회 |
| 8 | `detailInfo2` | 반복정보 조회 |
| 9 | `detailImage2` | 이미지정보 조회 |
| 10 | `detailWithTour2` | **무장애여행 상세정보 조회** (전용) |
| 11 | `areaBasedSyncList2` | 무장애 여행정보 동기화 목록 조회 |
| 12 | `ldongCode2` | 법정동 코드 조회 |
| 13 | `lclsSystmCode2` | 분류체계 코드 조회 |

---

## 목록 조회 오퍼레이션 (3~5, 11번 공통)

### 3. 지역기반 관광정보 조회 `areaBasedList2`

```
GET /areaBasedList2
```

**요청 파라미터**

| 파라미터 | 필수 | 설명 |
|---|---|---|
| `contentTypeId` | 옵션 | 관광타입 ID |
| `arrange` | 옵션 | 정렬 (A/C/D, O/Q/R) |
| `areaCode` | 옵션 | 구 지역코드 (2025년 말 종료 예정) |
| `sigunguCode` | 옵션 | 구 시군구코드 (areaCode 필수) |
| `cat1/2/3` | 옵션 | 구 서비스 분류코드 (2025년 말 종료 예정) |
| `lDongRegnCd` | 옵션 | 법정동 시도 코드 |
| `lDongSignguCd` | 옵션 | 법정동 시군구 코드 |
| `lclsSystm1~3` | 옵션 | 신 분류체계 코드 |
| `modifiedtime` | 옵션 | 수정일 필터 (YYYYMMDD) |

**응답 항목**: `contentid`, `contenttypeid`, `title`, `addr1/2`, `zipcode`, `tel`, `firstimage/2`, `mapx`, `mapy`, `createdtime`, `modifiedtime`, `areacode`, `sigungucode`, `cat1/2/3`, `lDongRegnCd`, `lDongSignguCd`, `lclsSystm1~3`, `cpyrhtDivCd`

### 4. 위치기반 관광정보 조회 `locationBasedList2`

```
GET /locationBasedList2
```

**추가 필수 파라미터**: `mapX` (경도), `mapY` (위도), `radius` (반경 m, 최대 20,000)  
**정렬 `arrange`**: `E`=거리순, `S`=거리순(이미지 있는 것) 추가 지원  
**응답 추가 항목**: `dist` (거리, 단위 m)

### 5. 키워드 검색 조회 `searchKeyword2`

```
GET /searchKeyword2
```

**추가 필수 파라미터**: `keyword` (검색어, 국문 URL 인코딩 필요)

### 11. 무장애 동기화 목록 조회 `areaBasedSyncList2`

```
GET /areaBasedSyncList2
```

| 파라미터 | 필수 | 설명 |
|---|---|---|
| `showflag` | 옵션 | `1`=표출, `0`=비표출 |
| `modifiedtime` | 옵션 | 수정일 (YYYYMM 또는 YYYYMMDD) |
| `contentTypeId` | 옵션 | 관광타입 ID |
| `areaCode` | 옵션 | 구 지역코드 |
| `lDongRegnCd` | 옵션 | 법정동 시도 코드 |
| `lclsSystm1~3` | 옵션 | 신 분류체계 코드 |

**응답 추가 항목**: `showflag`

---

## 6. 공통정보 조회 `detailCommon2`

```
GET /detailCommon2
```

**필수**: `contentId`

**응답**: `title`, `overview`, `homepage`, `tel`, `addr1/2`, `mapx/y`, `firstimage/2`, `areacode`, `sigungucode`, `cat1/2/3`, `lDongRegnCd`, `lDongSignguCd`, `lclsSystm1~3`

---

## 7. 소개정보 조회 `detailIntro2`

```
GET /detailIntro2
```

**필수**: `contentId`, `contentTypeId`

**타입별 주요 응답 항목**

| contentTypeId | 주요 항목 |
|---|---|
| 12 관광지 | `usetime`, `restdate`, `parking`, `chkbabycarriage`, `chkpet`, `chkcreditcard` |
| 14 문화시설 | `usetimeculture`, `usefee`, `parkingculture`, `discountinfo` |
| 15 행사/공연/축제 | `eventstartdate`, `eventenddate`, `usetimefestival`, `festivalgrade`, `festivaltype`, `progresstype` |
| 28 레포츠 | `usetimeleports`, `usefeeleports`, `openperiod`, `reservation` |
| 32 숙박 | `checkintime`, `checkouttime`, `roomcount`, `refundregulation` |
| 38 쇼핑 | `opentime`, `saleitem`, `parkingshopping` |
| 39 음식점 | `opentimefood`, `firstmenu`, `treatmenu`, `lcnsno` |

---

## 8. 반복정보 조회 `detailInfo2`

```
GET /detailInfo2
```

**필수**: `contentId`, `contentTypeId`

- **숙박(32)**: 객실 상세 (`roomtitle`, `roomsize1/2`, 기준/최대인원, 비수기/성수기 요금, 편의시설 Y/N 등)
- **기타**: `infoname` + `infotext` 반복 (fldgubun으로 유형 구분)

> ⚠️ 여행코스 반복정보는 KorWithService2에서 제공하지 않습니다. KorService2 사용 필요.

---

## 9. 이미지정보 조회 `detailImage2`

```
GET /detailImage2
```

**필수**: `contentId`  
**옵션**: `imageYN` (`Y`=콘텐츠 이미지, `N`=음식메뉴 이미지)

**응답**: `originimgurl` (500×333), `smallimageurl` (160×100), `imgname`, `serialnum`, `cpyrhtDivCd`

---

## 10. 무장애여행 상세정보 조회 `detailWithTour2` ★

```
GET /detailWithTour2
```

**필수**: `contentId`

**응답 항목**

### 지체장애

| 항목 | 설명 |
|---|---|
| `parking` | 장애인 주차장 여부 |
| `publictransport` | 대중교통 접근 정보 |
| `route` | 접근로 (경사로 등) |
| `ticketoffice` | 매표소 접근 |
| `promotion` | 홍보물 |
| `wheelchair` | 휠체어 대여 가능 여부 |
| `exit` | 출입통로 (주출입구 휠체어 접근) |
| `elevator` | 엘리베이터 |
| `restroom` | 장애인 화장실 |
| `auditorium` | 관람석 |
| `room` | 객실 |
| `handicapetc` | 기타 상세 |

### 시각장애

| 항목 | 설명 |
|---|---|
| `braileblock` | 점자블록 |
| `helpdog` | 보조견 동반 |
| `guidehuman` | 안내요원 |
| `audioguide` | 오디오 가이드 |
| `bigprint` | 큰 활자 홍보물 |
| `brailepromotion` | 점자 홍보물/표지판 |
| `guidesystem` | 유도 안내 설비 |
| `blindhandicapetc` | 기타 상세 |

### 청각장애

| 항목 | 설명 |
|---|---|
| `signguide` | 수화 안내 |
| `videoguide` | 자막 비디오/영상 자막 |
| `hearingroom` | 객실 |
| `hearinghandicapetc` | 기타 상세 |

### 영유아 가족

| 항목 | 설명 |
|---|---|
| `stroller` | 유모차 대여/사용 |
| `lactationroom` | 수유실 |
| `babysparechair` | 유아용 보조의자 |
| `infantsfamilyetc` | 기타 상세 |

**예시**
```
GET /detailWithTour2?serviceKey=인증키&MobileOS=ETC&MobileApp=AppTest
  &contentId=988449&_type=json
```

---

## 12. 법정동 코드 조회 `ldongCode2`

```
GET /ldongCode2
```

| 파라미터 | 필수 | 설명 |
|---|---|---|
| `lDongRegnCd` | 옵션 | 시도코드 (미입력 시 전체 시도 목록) |
| `lDongListYn` | 옵션 | `N`=코드조회, `Y`=전체목록조회 |

---

## 13. 분류체계 코드 조회 `lclsSystmCode2`

```
GET /lclsSystmCode2
```

| 파라미터 | 필수 | 설명 |
|---|---|---|
| `lclsSystm1` | 옵션 | 대분류 코드 |
| `lclsSystm2` | 옵션 | 중분류 코드 |
| `lclsSystm3` | 옵션 | 소분류 코드 |
| `lclsSystmListYn` | 옵션 | `N`=코드조회, `Y`=전체목록조회 |

> ℹ️ 분류체계 코드 상세 정의(대/중/소분류 전체 목록 및 contentTypeId 매핑)는 **KorService2 전용 정의서** `LclsSystm_Classification_Codes.md` 를 참고하세요.

---

## (Deprecated) 구 지역코드 조회 `areaCode2`

```
GET /areaCode2
```

> ⚠️ **2025년 12월 말 서비스 종료 예정.** `ldongCode2`로 대체.

`areaCode` 파라미터 미입력 시 전체 시도 목록, 입력 시 해당 시도의 시군구 목록 반환.

---

## (Deprecated) 서비스 분류코드 조회 `categoryCode2`

```
GET /categoryCode2
```

> ⚠️ **2025년 12월 말 서비스 종료 예정.** `lclsSystmCode2`로 대체.

`contentTypeId`, `cat1`, `cat2`, `cat3` 조합으로 대/중/소분류 코드 조회.

---

## 에러 코드

| 코드 | 메시지 | 설명 |
|---|---|---|
| 00 | NORMAL_CODE | 정상 |
| 03 | NODATA_ERROR | 데이터 없음 |
| 10 | INVALID_REQUEST_PARAMETER_ERROR | 잘못된 파라미터 |
| 11 | NO_MANDATORY_REQUEST_PARAMETERS_ERROR | 필수 파라미터 누락 |
| 22 | LIMITED_NUMBER_OF_SERVICE_REQUESTS_EXCEEDS_ERROR | 요청 한도 초과 |
| 30 | SERVICE_KEY_IS_NOT_REGISTERED_ERROR | 미등록 서비스키 |
| 31 | DEADLINE_HAS_EXPIRED_ERROR | 활용기간 만료 |
| 99 | UNKNOWN_ERROR | 기타 에러 |
