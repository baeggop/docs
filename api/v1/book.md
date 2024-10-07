## Book API 문서

### 기본 정보
- **Base URL**: `/book`

### 목차
- [POST `/:storeId`](#post-storeid) - 예약 생성 및 업데이트

---

### POST `/:storeId`
- **설명**: 특정 가게에 예약을 생성하거나 기존 예약을 업데이트합니다.

- **Request**:
  - **Path Parameters**:
    - `storeId`: 가게 ID (string, 필수)
  - **Body**:
    ```json
    {
      "id": "string",           // 예약 ID (기존 예약일 경우 제공)
      "userEmail": "string",    // 사용자 이메일
      "peopleCnt": int,         // 예약 인원수
      "bookDate": "string",     // 예약 날짜 (형식: YYYYMMDD)
      "bookTime": "string"      // 예약 시간 (형식: HHMM)
    }
    ```

- **Response**:
  - **Status**: 200 OK
  - **Body**:
    ```json
    {
      "message": "Booking saved successfully"
    }
    ```
  - **Error Responses**:
    - **500 Internal Server Error**:
      ```json
      {
        "error": "Failed to save Booking"
      }
      ```

---
