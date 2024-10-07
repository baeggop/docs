## Order API 문서

### 기본 정보
- **Base URL**: `/order`

### 목차
- [GET `/:storeId`](#get-storeid) - 특정 가게의 미결제 주문 조회
- [GET `/:tableId`](#get-tableid) - 특정 테이블의 미결제 주문 조회
- [POST `/:tableId`](#post-tableid) - 테이블에 새로운 주문 추가

---

### GET `/:storeId`
- **설명**: 특정 가게에서 미결제 상태인 모든 주문을 조회합니다.

- **Request**:
  - **Path Parameters**:
    - `storeId`: 가게 ID (string, 필수)
  
- **Response**:
  - **Status**: 200 OK
  - **Body**:
    ```json
    [
      {
        "id": "string",
        "isPaid": "N",
        "table": {
          "id": "string",
          "name": "string"
        },
        "orderDetails": [
          {
            "id": "string",
            "menu": {
              "id": "string",
              "name": "string"
            }
          }
        ]
      }
    ]
    ```
  - **Error Responses**:
    - **404 Not Found**:
      ```json
      {
        "error": "No active orders found for this store"
      }
      ```

---

### GET `/:tableId`
- **설명**: 특정 테이블의 미결제 주문을 조회합니다.

- **Request**:
  - **Path Parameters**:
    - `tableId`: 테이블 ID (string, 필수)
  
- **Response**:
  - **Status**: 200 OK
  - **Body**:
    ```json
    {
      "id": "string",
      "isPaid": "N",
      "orderDetails": [
        {
          "id": "string",
          "menu": {
            "id": "string",
            "name": "string"
          }
        }
      ]
    }
    ```
  - **Error Responses**:
    - **404 Not Found**:
      ```json
      {
        "error": "No active order found for this table"
      }
      ```

---

### POST `/:tableId`
- **설명**: 특정 테이블에 주문을 추가합니다. 이 API는 테이블에 주문을 생성하거나, 이미 존재하는 미결제 주문의 세부사항을 업데이트합니다.

- **Request**:
  - **Path Parameters**:
    - `tableId`: 테이블 ID (string, 필수)
  - **Body**:
    ```json
    [
      {
        "menuId": "string",
        "quantity": 1
      }
    ]
    ```

- **Response**:
  - **Status**: 201 Created
  - **Body**:
    ```json
    {
      "message": "Order saved successfully"
    }
    ```
  - **Error Responses**:
    - **404 Not Found**:
      ```json
      {
        "error": "Table not found"
      }
      ```
    - **400 Bad Request**:
      ```json
      {
        "error": "Menu ID and quantity are required"
      }
      ```
    - **500 Internal Server Error**:
      ```json
      {
        "error": "Failed to save order"
      }
      ```

---
