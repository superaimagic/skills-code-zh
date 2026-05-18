# 好测试和坏测试

## 好测试

**集成式**：通过真实接口测试，不是 mock 内部部分。

```typescript
// 好的：测试可观察行为
test("user can checkout with valid cart", async () => {
  const cart = createCart();
  cart.add(product);
  const result = await checkout(cart, paymentMethod);
  expect(result.status).toBe("confirmed");
});
```

特征：

- 测试用户/调用者关心的行为
- 只使用公共 API
- 在内部重构后存活
- 描述 WHAT，不是 HOW
- 每个测试一个逻辑断言

## 坏测试

**实现细节测试**：与内部结构耦合。

```typescript
// 坏的：测试实现细节
test("checkout calls paymentService.process", async () => {
  const mockPayment = jest.mock(paymentService);
  await checkout(cart, payment);
  expect(mockPayment.process).toHaveBeenCalledWith(cart.total);
});
```

红旗：

- Mock 内部协作者
- 测试私有方法
- 断言调用次数/顺序
- 重构时行为没变但测试崩了
- 测试名描述 HOW 不是 WHAT
- 通过外部方式验证而不是通过接口

```typescript
// 坏的：绕过接口验证
test("createUser saves to database", async () => {
  await createUser({ name: "Alice" });
  const row = await db.query("SELECT * FROM users WHERE name = ?", ["Alice"]);
  expect(row).toBeDefined();
});

// 好的：通过接口验证
test("createUser makes user retrievable", async () => {
  const user = await createUser({ name: "Alice" });
  const retrieved = await getUser(user.id);
  expect(retrieved.name).toBe("Alice");
});
```
