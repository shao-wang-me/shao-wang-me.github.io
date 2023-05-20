---
publish: true
en_title: NPV, Net Present Value
tags:
  - draft
  - flashcards
---


#draft 等额净现值计算公式化简

#flashcards 

NPV 计算公式 calculation
?
```text
r = interest rate
p = payment, p0 for the initial payment, p1, p2, p3, ... pn for afterwards
    p0 is called initial investment, p1...pn are called payments/coupon
    commonly p1 = p2 = ... = pn
n = number of terms

npv = p0 + p1/(1+r)^1 + p2/(1+r)^2 + ... + pn/(1+r)^n
```

$NPV = \sum_{t=0}^n p_t (1 + r)^{-t}$

```python
def npv(p0, r, n, p):  # equal payments
  result = p0  # p0 is negative, indicating outflow
  for i in range(1, n + 1):
    result += p / (1 + r) ** i
  return result

def npv(p0, r, ps):  # different payments
  result = p0
  for i, p in enumerate(ps):
    result += p / (1 + r) ** (i + 1)
  return result
```