## Payment API 문서

### 기본 정보
- **Base URL**: `/payment`

### 목차
- [PUT `/:orderId`](#put-orderid) - 주문 결제 상태 업데이트

---

### PUT `/:orderId`
- **설명**: 특정 주문의 결제 상태를 "Y"로 업데이트합니다.

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
        "id": "string",
        "isPaid": "Y"
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

---
