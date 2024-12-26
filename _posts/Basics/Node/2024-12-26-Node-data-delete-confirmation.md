---
title: "Node.js로 데이터 삭제 시 사용자 확인 및 결과 반환 구현"
excerpt: "Node.js와 MongoDB를 활용하여 데이터 삭제 시 사용자 확인 팝업과 삭제된 데이터를 반환하는 효율적인 구현 방법을 알아봅니다."
categories:
  - Node

tags:
  - [JavaScript, Node.js, MongoDB, Backend, 데이터 관리]
permalink: /Node/data-delete-confirmation/
toc: true
toc_sticky: true
date: 2024-12-26
last_modified_at: 2024-12-26
---

데이터 삭제 기능을 구현할 때, 사용자 확인을 받고 삭제된 항목에 대한 정보를 반환하면 사용자 경험(UX)을 크게 향상시킬 수 있습니다. 이번 글에서는 Node.js와 MongoDB를 활용하여 데이터 삭제 시 사용자 확인 팝업과 삭제된 데이터를 반환하는 방법을 살펴보겠습니다.

---

## 구현 목표
1. **사용자 확인**: 데이터를 삭제하기 전, 사용자에게 확인 팝업을 띄워 의사를 묻습니다.
2. **관련 데이터 삭제**: 메인 데이터 삭제 시 연관된 데이터도 함께 삭제합니다.
3. **삭제 결과 반환**: 삭제 완료 후, 삭제된 항목과 관련 데이터를 사용자에게 반환합니다.

---

## 1. 사용자 확인 팝업 구현
프론트엔드에서 삭제 버튼 클릭 시 사용자 확인 팝업을 띄웁니다. 사용자가 확인을 누르면 서버에 삭제 요청을 보냅니다.

### HTML + JavaScript 예제
```html
<!-- 삭제 버튼 -->
<button onclick="confirmDelete('farmId123')">Delete Farm</button>

<script>
    async function confirmDelete(farmId) {
        // 사용자 확인
        const confirmation = confirm(
            "진짜 삭제하시겠습니까?\n삭제할 경우 관련된 내용도 함께 삭제됩니다."
        );
        if (confirmation) {
            // 삭제 요청 전송
            const response = await fetch(`/farms/${farmId}`, {
                method: "DELETE",
            });

            if (response.ok) {
                const result = await response.json();
                // 삭제 완료 후 결과 출력
                alert(`삭제 완료!\n삭제된 농장: ${result.deletedFarm.name}\n관련 삭제된 항목: ${result.deletedProducts.join(", ")}`);
                window.location.reload(); // 페이지 새로고침
            } else {
                alert("삭제 중 오류가 발생했습니다.");
            }
        }
    }
</script>
```

---

## 2. 백엔드에서 삭제 처리 및 결과 반환
삭제 작업이 이루어진 후, 삭제된 메인 데이터와 관련 데이터를 반환하도록 백엔드를 구현합니다.

### MongoDB 미들웨어 설정 (`farm.js`)
MongoDB 미들웨어를 사용하여 관련 데이터를 삭제하고 결과를 저장합니다.

```javascript
farmSchema.post('findOneAndDelete', async function (farm) {
    if (farm.products.length) {
        const res = await Product.deleteMany({ _id: { $in: farm.products } });
        farm.deletedProducts = res.deletedCount ? farm.products : [];
    }
});
```

### 삭제 라우트 구현 (`index.js`)
삭제 작업 후 삭제된 데이터를 클라이언트로 반환합니다.

```javascript
// Delete farm
app.delete('/farms/:id', async (req, res) => {
    const { id } = req.params;
    const farm = await Farm.findByIdAndDelete(id);

    if (farm) {
        res.json({
            message: "삭제 완료",
            deletedFarm: farm,
            deletedProducts: farm.deletedProducts || []
        });
    } else {
        res.status(404).json({ message: "농장을 찾을 수 없습니다." });
    }
});
```

---

## 3. 트랜잭션을 활용한 삭제 작업
`farm`과 관련된 `products`를 삭제하는 작업이 원자적으로 처리되도록 트랜잭션을 사용하는 방법도 고려할 수 있습니다.

### 트랜잭션 예제
```javascript
app.delete('/farms/:id', async (req, res) => {
    const { id } = req.params;
    const session = await mongoose.startSession();
    session.startTransaction();
    try {
        const farm = await Farm.findByIdAndDelete(id).session(session);
        if (farm.products.length) {
            await Product.deleteMany({ _id: { $in: farm.products } }).session(session);
        }
        await session.commitTransaction();
        res.json({
            message: "삭제 완료",
            deletedFarm: farm,
            deletedProducts: farm.products
        });
    } catch (error) {
        await session.abortTransaction();
        res.status(500).json({ message: "삭제 중 오류 발생", error });
    } finally {
        session.endSession();
    }
});
```

---

## 결론
위 방법을 활용하면 사용자 확인 팝업과 삭제된 데이터 정보를 효율적으로 관리할 수 있습니다.

### 주요 포인트:
1. **프론트엔드**: 삭제 요청 전 사용자 확인 팝업 구현.
2. **백엔드**: 삭제 작업 완료 후 삭제된 데이터 반환.
3. **트랜잭션**: 삭제 작업의 원자성을 보장하여 데이터 무결성 유지.

위 코드를 바탕으로 사용자 경험을 향상시키는 삭제 기능을 구현해 보세요!

