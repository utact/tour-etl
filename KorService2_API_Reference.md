# 한국관광공사 국문 관광정보 서비스 API (KorService2)

> **버전**: v4.4 (2026-02-10)  
> **Base URL**: `https://apis.data.go.kr/B551011/KorService2`  
> **응답 형식**: XML (기본) / JSON (`&_type=json` 추가)  
> **데이터 갱신**: 일 1회  
> **개발계정 트래픽**: 일 1,000건

---

## 공통 필수 파라미터

모든 오퍼레이션에 공통으로 필요한 파라미터입니다.

| 파라미터 | 설명 | 예시 |
|---|---|---|
| `serviceKey` | 공공데이터포털 발급 인증키 (URL 인코딩) | - |
| `MobileOS` | OS 구분 | `IOS` / `AND` / `WEB` / `ETC` |
| `MobileApp` | 서비스명(앱명) | `AppTest` |
| `numOfRows` | 한 페이지 결과 수 (옵션) | `10` |
| `pageNo` | 페이지 번호 (옵션) | `1` |
| `_type` | 응답 형식 (옵션, 기본 XML) | `json` |

---

## 오퍼레이션 목록

| # | 오퍼레이션명 | 메서드 | 설명 |
|---|---|---|---|
| 1 | `areaBasedList2` | 지역기반 관광정보 조회 | 법정동 코드 기반 목록 조회 |
| 2 | `locationBasedList2` | 위치기반 관광정보 조회 | GPS 좌표 + 반경 기반 목록 조회 |
| 3 | `searchKeyword2` | 키워드 검색 조회 | 키워드로 전체/타입별 목록 조회 |
| 4 | `searchFestival2` | 행사정보 조회 | 날짜 기반 행사/공연/축제 목록 |
| 5 | `searchStay2` | 숙박정보 조회 | 숙박 타입 목록 조회 |
| 6 | `detailCommon2` | 공통정보 조회 | contentId로 기본정보/개요/좌표 등 |
| 7 | `detailIntro2` | 소개정보 조회 | contentId + contentTypeId로 타입별 상세 |
| 8 | `detailInfo2` | 반복정보 조회 | 타입별 추가정보(객실, 코스, 반복항목) |
| 9 | `detailImage2` | 이미지정보 조회 | 이미지 URL 목록 |
| 10 | `areaBasedSyncList2` | 동기화 목록 조회 | 수정일/표출여부 기반 동기화용 목록 |
| 11 | `detailPetTour2` | 반려동물 동반여행 정보 | 반려동물 동반 조건·시설 정보 |
| 12 | `ldongCode2` | 법정동 코드 조회 | 시도/시군구 법정동 코드 조회 |
| 13 | `lclsSystmCode2` | 분류체계 코드 조회 | 대/중/소분류 코드 조회 |

---

## ContentTypeId 코드표

| 관광타입 | contentTypeId |
|---|---|
| 관광지 | `12` |
| 문화시설 | `14` |
| 행사/공연/축제 | `15` |
| 여행코스 | `25` |
| 레포츠 | `28` |
| 숙박 | `32` |
| 쇼핑 | `38` |
| 음식점 | `39` |

---

## 정렬 구분 (`arrange`)

| 값 | 설명 |
|---|---|
| `A` | 제목순 |
| `C` | 수정일순 (최신순) |
| `D` | 생성일순 |
| `E` | 거리순 (위치기반 전용) |
| `O` | 제목순 (대표이미지 있는 것만) |
| `Q` | 수정일순 (대표이미지 있는 것만) |
| `R` | 생성일순 (대표이미지 있는 것만) |
| `S` | 거리순 (대표이미지 있는 것만, 위치기반 전용) |

---

## 1. 지역기반 관광정보 조회 `areaBasedList2`

```
GET /areaBasedList2
```

**주요 요청 파라미터**

| 파라미터 | 필수 | 설명 |
|---|---|---|
| `contentTypeId` | 옵션 | 관광타입 ID |
| `arrange` | 옵션 | 정렬 구분 |
| `lDongRegnCd` | 옵션 | 법정동 시도 코드 |
| `lDongSignguCd` | 옵션 | 법정동 시군구 코드 (lDongRegnCd 필수) |
| `lclsSystm1` | 옵션 | 분류체계 대분류 |
| `lclsSystm2` | 옵션 | 분류체계 중분류 (lclsSystm1 필수) |
| `lclsSystm3` | 옵션 | 분류체계 소분류 (1,2 필수) |
| `modifiedtime` | 옵션 | 수정일 필터 (YYYYMMDD) |

**응답 주요 항목**

`contentid`, `contenttypeid`, `title`, `addr1`, `addr2`, `zipcode`, `tel`, `firstimage`, `firstimage2`, `mapx`, `mapy`, `createdtime`, `modifiedtime`, `lDongRegnCd`, `lDongSignguCd`, `lclsSystm1`, `lclsSystm2`, `lclsSystm3`, `cpyrhtDivCd`

**예시**
```
GET /areaBasedList2?serviceKey=인증키&MobileOS=ETC&MobileApp=AppTest&_type=json
  &arrange=C&contentTypeId=12&lDongRegnCd=26&lDongSignguCd=380
  &lclsSystm1=NA&lclsSystm2=NA04&lclsSystm3=NA040500
```

---

## 2. 위치기반 관광정보 조회 `locationBasedList2`

```
GET /locationBasedList2
```

**주요 요청 파라미터**

| 파라미터 | 필수 | 설명 |
|---|---|---|
| `mapX` | **필수** | GPS 경도 (WGS84) |
| `mapY` | **필수** | GPS 위도 (WGS84) |
| `radius` | **필수** | 반경(m), 최대 20,000 |
| `contentTypeId` | 옵션 | 관광타입 ID |
| `arrange` | 옵션 | 정렬 구분 (E=거리순 지원) |
| `lDongRegnCd` | 옵션 | 법정동 시도 코드 |
| `lDongSignguCd` | 옵션 | 법정동 시군구 코드 |
| `lclsSystm1~3` | 옵션 | 분류체계 |

**응답 추가 항목**: `dist` (중심 좌표로부터 거리, 단위 m)

**예시**
```
GET /locationBasedList2?serviceKey=인증키&MobileOS=ETC&MobileApp=AppTest&_type=json
  &mapX=126.98375&mapY=37.563446&radius=1000&contentTypeId=39
```

---

## 3. 키워드 검색 조회 `searchKeyword2`

```
GET /searchKeyword2
```

**주요 요청 파라미터**

| 파라미터 | 필수 | 설명 |
|---|---|---|
| `keyword` | **필수** | 검색 키워드 (국문 URL 인코딩 필요) |
| `lDongRegnCd` | 옵션 | 법정동 시도 코드 |
| `lDongSignguCd` | 옵션 | 법정동 시군구 코드 |
| `lclsSystm1~3` | 옵션 | 분류체계 |

**예시**
```
GET /searchKeyword2?serviceKey=인증키&MobileOS=ETC&MobileApp=AppTest&_type=json
  &keyword=시장&lDongRegnCd=50&lDongSignguCd=130
```

---

## 4. 행사정보 조회 `searchFestival2`

```
GET /searchFestival2
```

contentTypeId=15 (행사/공연/축제) 전용.

**주요 요청 파라미터**

| 파라미터 | 필수 | 설명 |
|---|---|---|
| `eventStartDate` | **필수** | 행사 시작일 (YYYYMMDD) |
| `eventEndDate` | 옵션 | 행사 종료일 (YYYYMMDD) |
| `lDongRegnCd` | 옵션 | 법정동 시도 코드 |
| `lclsSystm1~3` | 옵션 | 분류체계 |

**응답 추가 항목**: `eventstartdate`, `eventenddate`, `progresstype`, `festivaltype`

---

## 5. 숙박정보 조회 `searchStay2`

```
GET /searchStay2
```

contentTypeId=32 (숙박) 전용.

**주요 요청 파라미터**: `lDongRegnCd`, `lDongSignguCd`, `lclsSystm1~3`, `arrange`, `modifiedtime`

---

## 6. 공통정보 조회 `detailCommon2`

```
GET /detailCommon2
```

**요청 파라미터**

| 파라미터 | 필수 | 설명 |
|---|---|---|
| `contentId` | **필수** | 콘텐츠 ID |

**응답 항목**: `contentid`, `contenttypeid`, `title`, `createdtime`, `modifiedtime`, `tel`, `telname`, `homepage`, `firstimage`, `firstimage2`, `addr1`, `addr2`, `zipcode`, `mapx`, `mapy`, `overview`, `lDongRegnCd`, `lDongSignguCd`, `lclsSystm1~3`, `cpyrhtDivCd`

---

## 7. 소개정보 조회 `detailIntro2`

```
GET /detailIntro2
```

**요청 파라미터**

| 파라미터 | 필수 | 설명 |
|---|---|---|
| `contentId` | **필수** | 콘텐츠 ID |
| `contentTypeId` | **필수** | 관광타입 ID |

**타입별 응답 항목 (일부)**

| contentTypeId | 주요 항목 |
|---|---|
| 12 관광지 | `usetime`, `restdate`, `parking`, `chkbabycarriage`, `chkpet`, `chkcreditcard`, `infocenter`, `opendate` |
| 14 문화시설 | `usetimeculture`, `restdateculture`, `parkingculture`, `usefee`, `discountinfo` |
| 15 행사/공연/축제 | `eventstartdate`, `eventenddate`, `eventplace`, `playtime`, `usetimefestival`, `festivalgrade` |
| 25 여행코스 | `distance`, `taketime`, `schedule`, `theme` |
| 28 레포츠 | `usetimeleports`, `restdateleports`, `usefeeleports`, `openperiod` |
| 32 숙박 | `checkintime`, `checkouttime`, `chkcooking`, `roomcount`, `reservationlodging`, `refundregulation` |
| 38 쇼핑 | `opentime`, `restdateshopping`, `saleitem`, `parkingshopping` |
| 39 음식점 | `opentimefood`, `restdatefood`, `firstmenu`, `treatmenu`, `smoking`, `lcnsno` |

---

## 8. 반복정보 조회 `detailInfo2`

```
GET /detailInfo2
```

**요청 파라미터**: `contentId` (필수), `contentTypeId` (필수)

- **숙박(32)**: 객실별 상세정보 (`roomtitle`, `roomsize1/2`, `roombasecount`, `roommaxcount`, 비수기/성수기 요금, 시설 여부 등)
- **여행코스(25)**: 코스 구성정보 (`subname`, `subdetailoverview`, `subdetailimg`)
- **기타 타입**: `infoname` + `infotext` 반복 구조 (fldgubun으로 유형 구분)

**fldgubun 유형 (관광지 기준)**

| 유형 | 포함 항목 |
|---|---|
| 1 | 입장료, 관람료, 시설이용료, 주차요금 |
| 2 | 화장실, 이용가능시설, 장애인편의시설 |
| 3 | 한국어/외국어안내서비스, 예약안내, 대관안내 |

---

## 9. 이미지정보 조회 `detailImage2`

```
GET /detailImage2
```

**요청 파라미터**

| 파라미터 | 필수 | 설명 |
|---|---|---|
| `contentId` | **필수** | 콘텐츠 ID |
| `imageYN` | 옵션 | `Y`=콘텐츠 이미지, `N`=음식메뉴 이미지(음식점 전용) |

**응답 항목**: `originimgurl` (500×333), `smallimageurl` (160×100), `imgname`, `serialnum`, `cpyrhtDivCd`

---

## 10. 동기화 목록 조회 `areaBasedSyncList2`

```
GET /areaBasedSyncList2
```

**주요 요청 파라미터**

| 파라미터 | 필수 | 설명 |
|---|---|---|
| `showflag` | 옵션 | `1`=표출, `0`=비표출 |
| `modifiedtime` | 옵션 | 수정일 (YYYYMMDD 또는 YYYYMM 또는 YYYY) |
| `oldContentid` | 옵션 | 이전 콘텐츠ID (DB 키 변경 추적용) |
| `contentTypeId` | 옵션 | 관광타입 ID |
| `lDongRegnCd` | 옵션 | 법정동 시도 코드 |
| `lclsSystm1~3` | 옵션 | 분류체계 |

**응답 추가 항목**: `showflag` (표출여부)

---

## 11. 반려동물 동반여행 정보 `detailPetTour2`

```
GET /detailPetTour2
```

**요청 파라미터**: `contentid` (옵션, 미기입 시 전체)

**응답 항목**: `acmpyPsblCpam`(동반가능동물), `acmpyNeedMtr`(필요사항), `acmpyTypeCd`(동반유형), `etcAcmpyInfo`, `relaAcdntRiskMtr`, `relaPosesFclty`, `relaFrnshPrdlst`, `relaPurcPrdlst`, `relaRntlPrdlst`

---

## 12. 법정동 코드 조회 `ldongCode2`

```
GET /ldongCode2
```

**요청 파라미터**

| 파라미터 | 필수 | 설명 |
|---|---|---|
| `lDongRegnCd` | 옵션 | 시도코드 (미입력 시 전체 시도 목록) |
| `lDongListYn` | 옵션 | `N`=코드조회(기본), `Y`=전체목록조회 |

**응답**
- `lDongListYn=N`: `code`, `name` (시도 또는 시군구)
- `lDongListYn=Y`: `lDongRegnCd`, `lDongRegnNm`, `lDongSignguCd`, `lDongSignguNm`

---

## 13. 분류체계 코드 조회 `lclsSystmCode2`

```
GET /lclsSystmCode2
```

**요청 파라미터**

| 파라미터 | 필수 | 설명 |
|---|---|---|
| `lclsSystm1` | 옵션 | 대분류 코드 |
| `lclsSystm2` | 옵션 | 중분류 코드 (lclsSystm1 필수) |
| `lclsSystm3` | 옵션 | 소분류 코드 (1,2 필수) |
| `lclsSystmListYn` | 옵션 | `N`=코드조회(기본), `Y`=전체목록조회 |

**응답**
- `Y`일 때: `lclsSystm1Cd/Nm`, `lclsSystm2Cd/Nm`, `lclsSystm3Cd/Nm`
- `N`일 때: `code`, `name`

---

## 에러 코드

| 코드 | 메시지 | 설명 |
|---|---|---|
| 00 | NORMAL_CODE | 정상 |
| 01 | APPLICATION_ERROR | 어플리케이션 에러 |
| 02 | DB_ERROR | DB 에러 |
| 03 | NODATA_ERROR | 데이터 없음 |
| 10 | INVALID_REQUEST_PARAMETER_ERROR | 잘못된 요청 파라미터 |
| 11 | NO_MANDATORY_REQUEST_PARAMETERS_ERROR | 필수 파라미터 누락 |
| 20 | SERVICE_ACCESS_DENIED_ERROR | 서비스 접근 거부 |
| 22 | LIMITED_NUMBER_OF_SERVICE_REQUESTS_EXCEEDS_ERROR | 요청 한도 초과 |
| 30 | SERVICE_KEY_IS_NOT_REGISTERED_ERROR | 미등록 서비스키 |
| 31 | DEADLINE_HAS_EXPIRED_ERROR | 활용기간 만료 |
| 99 | UNKNOWN_ERROR | 기타 에러 |

---

## 저작권 유형 (`cpyrhtDivCd`)

| 값 | 설명 |
|---|---|
| `Type1` | 제1유형 - 출처표시(권장) |
| `Type3` | 제3유형 - 출처표시 + 변경금지 |
