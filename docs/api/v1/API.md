# 배꼽시계 API 명세서

<details>
<summary> Store API 명세서 </summary>

## 기본 정보
- **Base URL**: `/store`
- **Authentication**: Optional (Depending on your implementation)

## 목차
- [1. GET `/getAll`](#1-get-getall) - 모든 가게 조회
- [2. GET `/search`](#2-get-search) - 가게 검색
- [3. GET `/:storeId/tables`](#3-get-storeidtables) - 특정 가게의 테이블 정보 조회
- [4. GET `/:storeId/orders`](#4-get-storeidorders) - 특정 가게의 미결제 주문 조회
- [5. GET `/:tableId/orders`](#5-get-tableidorders) - 특정 테이블의 미결제 주문 조회
- [6. GET `/:storeId/menus`](#6-get-storeidmenus) - 특정 가게의 메뉴 조회
- [7. GET `/:storeId`](#7-get-storeid) - 특정 가게의 상세 정보 조회
- [8. POST `/:storeId/tables`](#8-post-storeidtables) - 테이블 정보 수정 및 등록
- [9. POST `/:storeId/menu-categories`](#9-post-storeidmenu-categories) - 메뉴 카테고리 추가
- [10. POST `/:menuCateId/menus`](#10-post-menucateidmenus) - 메뉴 추가
- [11. POST `/:menuId/menu-options`](#11-post-menuidmenu-options) - 메뉴 옵션 추가

---

<details>
<summary>1. GET `/getAll`</summary>

### **설명**: 모든 가게 정보를 조회합니다.

- **Request**: 없음
- **Response**:
  - **Status**: 200 OK
  - **Body**:
    ```json
    [
      {
        "id": "string",
        "name": "string",
        "address": "string",
        "lat": "string",
        "long": "string",
        "tel": "string",
        "createdAt": "date-time",
        "updatedAt": "date-time"
      }
    ]
    ```
  - **Error Responses**:
    - **500 Internal Server Error**:
      ```json
      {
        "error": "Internal server error"
      }
      ```

</details>

---

<details>
<summary>2. GET `/search`</summary>

### **설명**: 가게 이름 또는 주소로 검색합니다.

- **Request**:
  - **Query Parameters**:
    - `query`: 검색어 (string, 필수)
  
- **Response**:
  - **Status**: 200 OK
  - **Body**:
    ```json
    [
      {
        "id": "string",
        "name": "string",
        "address": "string",
        "lat": "string",
        "long": "string",
        "tel": "string",
        "createdAt": "date-time",
        "updatedAt": "date-time"
      }
    ]
    ```
  - **Error Responses**:
    - **400 Bad Request**:
      ```json
      {
        "error": "Invalid query parameter"
      }
      ```
    - **500 Internal Server Error**:
      ```json
      {
        "error": "Failed to search stores"
      }
      ```

</details>

---

<details>
<summary>3. GET `/:storeId/tables`</summary>

### **설명**: 특정 가게의 테이블 정보 및 관련 미결제 주문 조회

- **Request**:
  - **Path Parameters**:
    - `storeId`: 가게 ID (string, 필수)
  
- **Response**:
  - **Status**: 200 OK
  - **Body**:
    ```json
    {
      "id": "string",
      "spaces": [
        {
          "id": "string",
          "name": "string",
          "tables": [
            {
              "id": "string",
              "name": "string",
              "orders": [
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
              ]
            }
          ]
        }
      ]
    }
    ```
  - **Error Responses**:
    - **404 Not Found**:
      ```json
      {
        "error": "Store not found"
      }
      ```
    - **500 Internal Server Error**:
      ```json
      {
        "error": "Internal server error"
      }
      ```

</details>

---

<details>
<summary>4. GET `/:storeId/orders`</summary>

### **설명**: 특정 가게에서 미결제 상태인 모든 주문을 조회합니다.

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
    - **500 Internal Server Error**:
      ```json
      {
        "error": "Failed to retrieve orders for this store"
      }
      ```

</details>

---

<details>
<summary>5. GET `/:tableId/orders`</summary>

### **설명**: 특정 테이블의 미결제 주문을 조회합니다.

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
    - **500 Internal Server Error**:
      ```json
      {
        "error": "Failed to retrieve the current order"
      }
      ```

</details>

---

<details>
<summary>6. GET `/:storeId/menus`</summary>

### **설명**: 특정 가게의 메뉴 및 메뉴 옵션을 조회합니다.

- **Request**:
  - **Path Parameters**:
    - `storeId`: 가게 ID (string, 필수)

- **Response**:
  - **Status**: 200 OK
  - **Body**:
    ```json
    {
      "id": "string",
      "menuCates": [
        {
          "id": "string",
          "name": "string",
          "menus": [
            {
              "id": "string",
              "name": "string",
              "price": 0,
              "menuOptions": [
                {
                  "id": "string",
                  "name": "string",
                  "price": 0,
                  "isRequired": "Y"
                }
              ]
            }
          ]
        }
      ]
    }
    ```
  - **Error Responses**:
    - **404 Not Found**:
      ```json
      {
        "error": "Store not found"
      }
      ```
    - **500 Internal Server Error**:
      ```json
      {
        "error": "Internal server error"
      }
      ```

</details>

---

<details>
<summary>7. GET `/:storeId`</summary>

### **설명**: 특정 가게의 세부 정보 및 주문 정보를 조회합니다.

- **Request**:
  - **Path Parameters**:
    - `storeId`: 가게 ID (string, 필수)

- **Response**:
  - **Status**: 200 OK
  - **Body**:
    ```json
    {
      "id": "string",
      "spaces": [
        {
          "id": "string",
          "name": "string",
          "tables": [
            {
              "id": "string",
              "name": "string",
              "orders": [
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
              ]
            }
          ]
        }
      ],
      "menuCates": [
        {
          "id": "string",
          "name": "string",
          "menus": [
            {
              "id": "string",
              "name": "string",
              "price": 0,
              "menuOptions": [
                {
                  "id": "string",
                  "name": "string",
                  "price": 0,
                  "isRequired": "Y"
                }
              ]
            }
          ]
        }
      ]
    }
    ```
  - **Error Responses**:
    - **404 Not Found**:
      ```json
      {
        "error": "Store not found"
      }
      ```
    - **500 Internal Server Error**:
      ```json
      {
        "error": "Internal server error"
      }
      ```

</details>

---

<details>
<summary>8. POST `/:storeId/tables`</summary>

### **설명**: 특정 가게의 테이블 정보를 수정 또는 등록합니다.

- **Request**:
  - **Path Parameters**:
    - `storeId`: 가게 ID (string, 필수)
  - **Body**:
    ```json
    [
      {
        "id": "string",
        "left": 0,
        "top": 0,
        "width": 100,
        "height": 100,
        "name": "string",
        "color": "string",
        "spaceId": "string"
      }
    ]
    ```

- **Response**:
  - **Status**: 200 OK
  - **Body**:
    ```json
    {
      "message": "Tables saved successfully"
    }
    ```
  - **Error Responses**:
    - **500 Internal Server Error**:
      ```json
      {
        "error": "Failed to save tables"
      }
      ```

</details>

---

<details>
<summary>9. POST `/:storeId/menu-categories`</summary>

### **설명**: 특정 가게에 새로운 메뉴 카테고리를 추가합니다.

- **Request**:
  - **Path Parameters**:
    - `storeId`: 가게 ID (string, 필수)
  - **Body**:
    ```json
    {
      "name": "string"
    }
    ```

- **Response**:
  - **Status**: 201 Created
  - **Body**:
    ```json
    {
      "id": "string",
      "name": "string"
    }
    ```
  - **Error Responses**:
    - **500 Internal Server Error**:
      ```json
      {
        "error": "Failed to create menu category"
      }
      ```

</details>

---

<details>
<summary>10. POST `/:menuCateId/menus`</summary>

### **설명**: 특정 메뉴 카테고리에 새로운 메뉴를 추가합니다.

- **Request**:
  - **Path Parameters**:
    - `menuCateId`: 메뉴 카테고리 ID (string, 필수)
  - **Body**:
    ```json
    {
      "name": "string",
      "price": 0
    }
    ```

- **Response**:
  - **Status**: 201 Created
  - **Body**:
    ```json
    {
      "id": "string",
      "name": "string",
      "price": 0
    }
    ```
  - **Error Responses**:
    - **500 Internal Server Error**:
      ```json
      {
        "error": "Failed to create menu"
      }
      ```

</details>

---

<details>
<summary>11. POST `/:menuId/menu-options`</summary>

### **설명**: 특정 메뉴에 새로운 옵션을 추가합니다.

- **Request**:
  - **Path Parameters**:
    - `menuId`: 메뉴 ID (string, 필수)
  - **Body**:
    ```json
    {
      "name": "string",
      "price": 0,
      "isRequired": "N"
    }
    ```

- **Response**:
  - **Status**: 201 Created
  - **Body**:
    ```json
    {
      "id": "string",
      "name": "string",
      "price": 0,
      "isRequired": "N",
      "menuId": "string"
    }
    ```
  - **Error Responses**:
    - **500 Internal Server Error**:
      ```json
      {
        "error": "Failed to create menu option"
      }
      ```

</details>
</details>

---

<details>
<summary> Table Order API 명세서 </summary>

## 기본 정보
- **Base URL**: `/order`
- **Authentication**: Optional (Depending on your implementation)

## 목차
- [1. GET `/:tableId`](#1-get-tableid) - 특정 테이블의 미결제 주문 정보 조회
- [2. POST `/:tableId`](#2-post-tableid) - 특정 테이블에 주문 추가

---

<details>
<summary>1. GET `/:tableId`</summary>

### **설명**: 특정 테이블의 미결제 주문 정보를 조회합니다.

- **Request**:
  - **Path Parameters**:
    - `tableId`: 테이블 ID (string, 필수)
  
- **Response**:
  - **Status**: 200 OK
  - **Body**:
    ```json
    [
      {
        "id": "string",                 // 주문 ID
        "isPaid": "N",                  // 결제 여부 ('Y' 또는 'N')
        "orderDetails": [
          {
            "id": "string",             // 주문 상세 ID
            "quantity": 1,              // 주문 수량
            "menu": {
              "id": "string",           // 메뉴 ID
              "name": "string"          // 메뉴 이름
            },
            "menuOption": {
              "id": "string",           // 메뉴 옵션 ID
              "name": "string"          // 메뉴 옵션 이름
            },
            "menuOptionItem": {
              "id": "string",           // 메뉴 옵션 항목 ID
              "name": "string"          // 메뉴 옵션 항목 이름
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
        "error": "Table not found"
      }
      ```
    - **500 Internal Server Error**:
      ```json
      {
        "error": "Failed to retrieve unpaid orders for the table"
      }
      ```

</details>

---

<details>
<summary>2. POST `/:tableId`</summary>

### **설명**: 특정 테이블에 주문을 추가합니다. 이 API는 테이블에 주문을 생성하거나, 이미 존재하는 미결제 주문의 세부사항을 업데이트합니다.

- **Request**:
  - **Path Parameters**:
    - `tableId`: 테이블 ID (string, 필수)
  - **Body**: 주문 상세 정보 (Array of `OrderDetailInput`)
    ```json
    [
      {
        "menuId": "string",          // 메뉴 ID
        "quantity": 1                // 주문 수량
      }
    ]
    ```

- **Response**:
  - **Status**: 200 OK
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
    - **400 Bad Request**: 유효하지 않은 요청일 경우
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

</details>
</details>

---

<details>
<summary>Order Payment API 명세서</summary>



## 기본 정보
- **Base URL**: `/payment`
- **Authentication**: Optional (Depending on your implementation)

## 목차
- [1. PUT `/:orderId`](#1-put-orderid) - 주문 결제 업데이트

---

<details>
<summary>1. PUT <code>/:orderId</code></summary>

### **설명**: 특정 주문의 결제 상태를 "Y"로 업데이트합니다.

- **Request**:
  - **Path Parameters**:
    - `orderId`: 주문 ID (string, 필수)
  
- **Response**:
  - **Status**: 200 OK
  - **Body**:
    ```json
    {
      "message": "Order payment updated successfully",
      "order": {
        "id": "string",           // 주문 ID
        "isPaid": "Y"             // 결제 여부 (결제 완료: 'Y')
      }
    }
    ```
  - **Error Responses**:
    - **404 Not Found**:
      ```json
      {
        "error": "Order not found"
      }
      ```
    - **500 Internal Server Error**:
      ```json
      {
        "error": "Failed to update order payment status"
      }
      ```

</details>

</details>

---

<details>
<summary> Booking API 명세서 </summary>

## 기본 정보
- **Base URL**: `/book`
- **Authentication**: Optional (Depending on your implementation)

## 목차
- [1. POST `/:storeId`](#1-post-storeid) - 예약 생성 및 업데이트

---

<details>
<summary>1. POST <code>/:storeId</code></summary>

### **설명**: 특정 가게에 예약을 생성하거나 기존 예약을 업데이트합니다.

- **Request**:
  - **Path Parameters**:
    - `storeId`: 가게 ID (string, 필수)
  - **Body**: 예약 정보
    ```json
    {
      "id": "string",           // 예약 ID (기존 예약일 경우 제공)
      "userEmail": "string",    // 사용자 이메일
      "peopleCnt": 4,           // 예약 인원수
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

</details>

</details>