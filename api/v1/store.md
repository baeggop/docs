## Store API 문서

### 기본 정보
- **Base URL**: `/store`

### 목차
- [GET `/getAll`](#get-getall) - 모든 가게 조회
- [GET `/search`](#get-search) - 가게 검색
- [GET `/:storeId/tables`](#get-storeid-tables) - 특정 가게의 테이블 정보 조회
- [GET `/:storeId/menus`](#get-storeid-menus) - 특정 가게의 메뉴 조회
- [GET `/:storeId`](#get-storeid) - 특정 가게의 상세 정보 조회
- [POST `/:storeId/tables`](#post-storeid-tables) - 테이블 정보 수정 및 등록
- [POST `/:storeId/menu-categories`](#post-storeid-menu-categories) - 메뉴 카테고리 추가
- [POST `/:menuCateId/menus`](#post-menucateid-menus) - 메뉴 추가
- [POST `/:menuId/menu-options`](#post-menuid-menu-options) - 메뉴 옵션 추가

---

### GET `/getAll`
- **설명**: 모든 가게 정보를 조회합니다.
  
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
        "start": "hh:mm",
        "end": "hh:mm",
        "break": "hh:mm",
        "tel": "string",
        "info": "string",
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

---

### GET `/search` 
- **설명**: 가게 이름 또는 주소로 검색합니다.

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
        "start": "hh:mm",
        "end": "hh:mm",
        "break": "hh:mm",
        "tel": "string",
        "info": "string",
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

---

### GET `/:storeId/tables`
- **설명**: 특정 가게의 테이블 정보 및 관련 미결제 주문 조회

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

---

### GET `/:storeId/orders`
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
    - **500 Internal Server Error**:
      ```json
      {
        "error": "Failed to retrieve orders for this store"
      }
      ```

---

### GET `/:tableId/orders`
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
    - **500 Internal Server Error**:
      ```json
      {
        "error": "Failed to retrieve the current order"
      }
      ```

---

### GET `/:storeId/menus`
- **설명**: 특정 가게의 메뉴 및 메뉴 옵션을 조회합니다.

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

---

### GET `/:storeId`
- **설명**: 특정 가게의 세부 정보 및 주문 정보를 조회합니다.

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

---

### POST `/:storeId/tables`
- **설명**: 특정 가게의 테이블 정보를 수정 또는 등록합니다.

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

---

### POST `/:storeId/menu-categories`
- **설명**: 특정 가게에 새로운 메뉴 카테고리를 추가합니다.

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

---

### POST `/:menuCateId/menus`
- **설명**: 특정 메뉴 카테고리에 새로운 메뉴를 추가합니다.

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

---

### POST `/:menuId/menu-options`
- **설명**: 특정 메뉴에 새로운 옵션을 추가합니다.

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