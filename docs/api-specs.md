# API 명세서

---
### **상품 API**

#### 1. 상품 목록 조회
`GET` `/api/v1/products` (시스템에 등록된 모든 상품 목록을 조회합니다)

**Headers**
| name | type | data type | description |
| --- | --- | --- | --- |
| (none) | | | |

**Parameters**
| (none) |

**Responses**
| http code | content-type | response |
| --- | --- | --- |
| 200 | application/json | `[{ "id": 1, "name": "상품1", "price": 1000, "stock": 10 }, ...]` |
| 500 | application/json | `{"code": "E001", "message": "서버 내부 오류"}` |

**Example cURL**
```bash
curl -X GET http://localhost:8080/api/v1/products
```

#### 2. 인기 상품 조회
`GET` `/api/v1/products/popular` (최근 N일간 가장 많이 판매된 상품 목록을 조회합니다)

**Headers**
| name | type | data type | description |
| --- | --- | --- | --- |
| (none) | | | |

**Parameters**
| name | type | data type | description |
| --- | --- | --- | --- |
| days | query | integer | 조회 기간(일). 기본값: 7 |
| limit | query | integer | 상위 N개. 기본값: 5 |

**Responses**
| http code | content-type | response |
| --- | --- | --- |
| 200 | application/json | `[{ "id": 10, "name": "인기상품A", "price": 5000, "stock": 50 }, ...]` |
| 500 | application/json | `{"code": "E001", "message": "서버 내부 오류"}` |

**Example cURL**
```bash
curl -X GET "http://localhost:8080/api/v1/products/popular?days=3&limit=5"
```

---
### **포인트 API**

#### 1. 내 포인트 조회
`GET` `/api/v1/points/me` (현재 사용자의 포인트 잔액을 조회합니다)

**Headers**
| name | type | data type | description |
| --- | --- | --- | --- |
| X-USER-ID | required | long | 사용자 ID |

**Parameters**
| (none) |

**Responses**
| http code | content-type | response |
| --- | --- | --- |
| 200 | application/json | `{"userId": 1, "points": 25000}` |
| 401 | application/json | `{"code": "A001", "message": "사용자 ID(X-USER-ID)가 필요합니다."}` |
| 404 | application/json | `{"code": "A002", "message": "사용자를 찾을 수 없습니다."}` |

**Example cURL**
```bash
curl -X GET -H "X-USER-ID: 1" http://localhost:8080/api/v1/points/me
```

#### 2. 포인트 충전
`POST` `/api/v1/points/charge` (포인트를 충전합니다)

**Headers**
| name | type | data type | description |
| --- | --- | --- | --- |
| X-USER-ID | required | long | 사용자 ID |

**Parameters (Request Body)**
```json
{
  "amount": 10000
}
```

**Responses**
| http code | content-type | response |
| --- | --- | --- |
| 200 | application/json | `{"userId": 1, "points": 35000}` |
| 400 | application/json | `{"code": "P001", "message": "충전 금액은 0보다 커야 합니다."}` |
| 401 | application/json | `{"code": "A001", "message": "사용자 ID(X-USER-ID)가 필요합니다."}` |

**Example cURL**
```bash
curl -X POST -H "Content-Type: application/json" -H "X-USER-ID: 1" \
-d '{"amount": 10000}' http://localhost:8080/api/v1/points/charge
```

---
### **주문 API**

#### 1. 주문 및 결제
`POST` `/api/v1/orders` (상품을 주문하고 포인트로 결제합니다)

**Headers**
| name | type | data type | description |
| --- | --- | --- | --- |
| X-USER-ID | required | long | 사용자 ID |

**Parameters (Request Body)**
```json
{
  "items": [
    { "productId": 1, "quantity": 2 },
    { "productId": 5, "quantity": 1 }
  ]
}
```

**Responses**
| http code | content-type | response |
| --- | --- | --- |
| 201 | application/json | `{"orderId": 101, "status": "ORDERED", "totalPrice": 5000}` |
| 400 | application/json | `{"code": "O001", "message": "재고가 부족한 상품이 있습니다."}` <br> `{"code": "O002", "message": "포인트 잔액이 부족합니다."}` |
| 401 | application/json | `{"code": "A001", "message": "사용자 ID(X-USER-ID)가 필요합니다."}` |

**Example cURL**
```bash
curl -X POST -H "Content-Type: application/json" -H "X-USER-ID: 1" \
-d '{"items": [{ "productId": 1, "quantity": 2 }]}' http://localhost:8080/api/v1/orders
```

---
### **쿠폰 API**

#### 1. 선착순 쿠폰 발급
`POST` `/api/v1/coupons/{couponId}/issue` (선착순으로 쿠폰을 발급받습니다)

**Headers**
| name | type | data type | description |
| --- | --- | --- | --- |
| X-USER-ID | required | long | 사용자 ID |

**Parameters**
| name | type | data type | description |
| --- | --- | --- | --- |
| couponId | path | long | 쿠폰 ID |

**Responses**
| http code | content-type | response |
| --- | --- | --- |
| 200 | application/json | `{"userId": 1, "couponId": 1, "issuedAt": "2025-11-10T10:00:00"}` |
| 400 | application/json | `{"code": "C001", "message": "이미 발급받은 쿠폰입니다."}` |
| 401 | application/json | `{"code": "A001", "message": "사용자 ID(X-USER-ID)가 필요합니다."}` |
| 409 | application/json | `{"code": "C002", "message": "쿠폰이 모두 소진되었습니다."}` |

**Example cURL**
```bash
curl -X POST -H "X-USER-ID: 1" http://localhost:8080/api/v1/coupons/1/issue
```

```