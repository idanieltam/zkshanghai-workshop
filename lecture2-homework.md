# 第2课 课后作业
## Exercise 1 - Num2Bits

-   Parameters: `nBits`
-   Input signal(s): `in`
-   Output signal(s): `b[nBits]`

The output signals should be an array of bits of length `nBits` equivalent to the binary representation of `in`. `b[0]` is the least significant bit. ( `b[0]` 是最低有效位 )

```circom
template Num2Bits(n) {
    signal input in;
    signal output out[n];
    var accum = 0;  

    for (var i = 0; i<n; i++) {
        accu += (2**i) * out[i];
    }

    in === accum;  
    
    for (var i = 0; i < n; i++) {
    0 === out[i] * (out[i] - 1);
    }
}
```

## Exercise 2 - IsZero

-   Parameters: none
-   Input signal(s): `in`
-   Output signal(s): `out`

Specification: If `in` is zero, `out` should be `1`. If `in` is nonzero, `out` should be `0`. This one is a little tricky!

```circom
template IsZero() {
    signal input in;
    signal output out;

    signal inv;

    inv <-- in!=0 ? 1/in : 0;

    out <== -in*inv +1;
    in*out === 0;
}
```

## Exercise 3 - IsEqual
-   Input signal(s): `in[2]`
-   Output signal(s): `out`

Specification: If `in[0]` is equal to `in[1]`, `out` should be `1`. Otherwise, `out` should be `0`.

``` rust
template IsEqual() {
    signal input in[2];
    signal output out;

    component isz = IsZero();

    in[1] - in[0] ==> isz.in;

    isz.out ==> out;
}
```

## Exercise 4 - LessThan

-   Input signal(s): `in[2]`. Assume that it is known ahead of time that these are at most 2252−12252−1.
-   Output signal(s): `out` 

Specification: If `in[0]` is strictly less than `in[1]`,  `out` should be `1`. Otherwise, `out` should be `0`.

```rust
template LessThan(n) {
    assert(n <= 252);
    signal input in[2];
    signal output out;

    component n2b = Num2Bits(n+1);

    n2b.in <== in[0]+ (1<<n) - in[1];

    out <== 1-n2b.out[n];
}
```

## Exercise 5 - Selector

-   参数：`nChoices`
-   输入信号：`in[nChoices]`, `index`
-   输出：`out`

要求：输出 `out` 应该等于`in[index]`。 如果 `index` 越界（不在 `[0, nChoices)` 中），`out` 应该是 `0`。

```rust
include "CalculateTotal.circom"

template QuinSelector(choices) {
    signal input in[choices];
    signal input index;
    signal output out;
    
    // Ensure that index < choices
    component lessThan = LessThan(4);
    lessThan.in[0] <== index;
    lessThan.in[1] <== choices;
    lessThan.out === 1;

    component calcTotal = CalculateTotal(choices);
    component eqs[choices];

    // For each item, check whether its index equals the input index.
    for (var i = 0; i < choices; i ++) {
        eqs[i] = IsEqual();
        eqs[i].in[0] <== i;
        eqs[i].in[1] <== index;

        // eqs[i].out is 1 if the index matches. As such, at most one input to
        // calcTotal is not 0.
        calcTotal.in[i] <== eqs[i].out * in[i];
    }

    // Returns 0 + 0 + 0 + item
    out <== calcTotal.out;
}
```

