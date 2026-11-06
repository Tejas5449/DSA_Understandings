# Best Time to Buy and Sell Stock

**Difficulty:** Easy  
**Topic:** Array, Greedy, One-Pass Algorithm  
**Commonly Asked In:** Coding interviews

---

## 1. Problem Statement

You are given an array `prices` where `prices[i]` represents the price of a stock on the `i-th` day.

You want to maximize your profit by choosing:

- One day to buy the stock.
- A different day in the future to sell the stock.

Return the maximum profit you can achieve.

If you cannot make any profit, return `0`.

**Important:** You must buy before you sell.

### Example 1

**Input:**
```java
prices = [7, 1, 5, 3, 6, 4]
```

**Output:**
```java
5
```

**Explanation:**

- Buy on day 2 at price `1`.
- Sell on day 5 at price `6`.
- Profit = `6 - 1 = 5`.

### Example 2

**Input:**
```java
prices = [7, 6, 4, 3, 1]
```

**Output:**
```java
0
```

**Explanation:**

The stock price continuously decreases. No profitable transaction is possible.

### Example 3

**Input:**
```java
prices = [2, 4, 1]
```

**Output:**
```java
2
```

**Explanation:**

Buy at `2` and sell later at `4`.

---

## 2. Approach 1: Brute Force

### Concept

Check every possible buying day and every selling day after it.

Calculate the profit for each pair:

\[
Profit = SellingPrice - BuyingPrice
\]

Keep track of the maximum profit.

### Java Code

```java
public class StockBruteForce {

    public static int maxProfit(int[] prices) {

        int maxProfit = 0;

        for (int i = 0; i < prices.length; i++) {

            for (int j = i + 1; j < prices.length; j++) {

                int profit = prices[j] - prices[i];

                maxProfit = Math.max(maxProfit, profit);
            }
        }

        return maxProfit;
    }

    public static void main(String[] args) {

        int[] prices = {7, 1, 5, 3, 6, 4};

        System.out.println(maxProfit(prices));
    }
}
```

### Dry Run

**Input:**

```java
prices = [7, 1, 5, 3, 6, 4]
```

| Buy Price | Sell Price | Profit |
|---:|---:|---:|
| 7 | 1 | -6 |
| 7 | 5 | -2 |
| 7 | 3 | -4 |
| 7 | 6 | -1 |
| 7 | 4 | -3 |
| 1 | 5 | 4 |
| 1 | 3 | 2 |
| 1 | 6 | 5 |
| 1 | 4 | 3 |
| 5 | 3 | -2 |
| 5 | 6 | 1 |
| 5 | 4 | -1 |
| 3 | 6 | 3 |
| 3 | 4 | 1 |
| 6 | 4 | -2 |

The maximum profit is `5`.

### Complexity Analysis

**Time Complexity: O(n²)**

We use two nested loops to check all valid buy-and-sell combinations.

**Space Complexity: O(1)**

Only a few variables are used.

---

## 3. Approach 2: Optimized One-Pass Approach

### Concept

Instead of checking every combination, maintain two variables:

1. `minPrice`: The minimum stock price seen so far.
2. `maxProfit`: The maximum profit calculated so far.

For every day:

1. Check whether today's price is lower than the minimum price.
2. If yes, update `minPrice`.
3. Otherwise, calculate today's possible profit.
4. Update `maxProfit` if today's profit is higher.

Formula:

\[
Profit = CurrentPrice - MinimumPrice
\]

### Java Code

```java
public class StockOptimized {

    public static int maxProfit(int[] prices) {

        int minPrice = Integer.MAX_VALUE;

        int maxProfit = 0;

        for (int i = 0; i < prices.length; i++) {

            if (prices[i] < minPrice) {

                minPrice = prices[i];

            } else {

                int profit = prices[i] - minPrice;

                maxProfit = Math.max(maxProfit, profit);
            }
        }

        return maxProfit;
    }

    public static void main(String[] args) {

        int[] prices = {7, 1, 5, 3, 6, 4};

        System.out.println(maxProfit(prices));
    }
}
```

---

## 4. Detailed Dry Run

**Input:**

```java
prices = [7, 1, 5, 3, 6, 4]
```

Initial values:

```java
minPrice = Integer.MAX_VALUE;
maxProfit = 0;
```

| Day | Price | Minimum Price | Current Profit | Maximum Profit |
|---|---:|---:|---:|---:|
| 1 | 7 | 7 | 0 | 0 |
| 2 | 1 | 1 | 0 | 0 |
| 3 | 5 | 1 | 4 | 4 |
| 4 | 3 | 1 | 2 | 4 |
| 5 | 6 | 1 | 5 | 5 |
| 6 | 4 | 1 | 3 | 5 |

Final result:

```java
5
```

**Understanding the iterations:**

- Day 1: Price is `7`. It becomes the minimum price.
- Day 2: Price is `1`. Update the minimum price to `1`.
- Day 3: Price is `5`. Profit is `5 - 1 = 4`.
- Day 4: Price is `3`. Profit is `3 - 1 = 2`. Maximum remains `4`.
- Day 5: Price is `6`. Profit is `6 - 1 = 5`. Maximum becomes `5`.
- Day 6: Price is `4`. Profit is `4 - 1 = 3`. Maximum remains `5`.

Therefore, the maximum profit is `5`.

---

## 5. Why Does This Approach Work?

The key observation is that the best buying price for a particular selling day is the lowest price that appeared before that day.

We do not need to remember every previous price.

We only need the minimum price seen so far.

For example:

```java
prices = [7, 1, 5, 3, 6, 4]
```

When we reach price `6`, the lowest previous price is `1`.

Therefore:

```java
profit = 6 - 1 = 5;
```

This is the best profit possible when selling on that day.

Repeating this process for every day guarantees that we consider the best possible selling opportunity.

---

## 6. Alternative Optimized Code

You may also write the solution by calculating the profit before updating the minimum price.

```java
public static int maxProfit(int[] prices) {

    int minPrice = Integer.MAX_VALUE;

    int maxProfit = 0;

    for (int price : prices) {

        int profit = price - minPrice;

        maxProfit = Math.max(maxProfit, profit);

        minPrice = Math.min(minPrice, price);
    }

    return maxProfit;
}
```

This version also works because `minPrice` represents the lowest price from previous days when the profit is calculated.

For the first day, subtracting `Integer.MAX_VALUE` produces a negative value, so `maxProfit` remains `0`. Then the minimum price is updated.

Both optimized implementations have the same complexity.

---

## 7. Complexity Analysis

| Approach | Time Complexity | Space Complexity |
|---|---|---|
| Brute Force | O(n²) | O(1) |
| One Pass | O(n) | O(1) |

**Why is the optimized solution O(n)?**

- We traverse the array only once.
- Each iteration performs constant-time comparisons and arithmetic.
- We use only two variables.

Therefore:

\[
Time = O(n), \quad Space = O(1)
\]

---

## 8. Edge Cases

| Input | Expected Output | Explanation |
|---|---:|---|
| `[7, 1, 5, 3, 6, 4]` | 5 | Buy at 1, sell at 6 |
| `[7, 6, 4, 3, 1]` | 0 | Prices continuously decrease |
| `[1, 2]` | 1 | Buy at 1, sell at 2 |
| `[5]` | 0 | Only one day |
| `[2, 2, 2, 2]` | 0 | No price difference |
| `[3, 8, 1, 9]` | 8 | Buy at 1, sell at 9 |

---

## 9. Common Interview Mistakes

1. **Selling before buying:** The selling day must always come after the buying day.
2. **Initializing maximum profit incorrectly:** Start with `0`, because negative profit is not useful.
3. **Updating minimum price incorrectly:** Track the minimum price seen so far, not the minimum price of the entire array.
4. **Using nested loops unnecessarily:** The one-pass approach is more efficient.
5. **Confusing this problem with multiple transactions:** This problem allows only one buy-and-sell transaction.

---

## 10. Interview Questions

**Q1. Why do we initialize `minPrice` with `Integer.MAX_VALUE`?**

So that the first stock price will always be smaller and will correctly initialize the minimum price.

**Q2. Why do we initialize `maxProfit` with zero?**

Because we are allowed not to make a transaction. If every transaction results in a loss, the answer should be zero.

**Q3. Why is the optimized solution O(n)?**

Because the array is traversed once, and each iteration performs constant-time operations.

**Q4. Can we use a HashMap for this problem?**

A HashMap is unnecessary. We only need the minimum previous price and maximum profit, so two variables are sufficient.

**Q5. What is the difference between this problem and unlimited stock transactions?**

- This problem allows only one buy-and-sell transaction.
- The unlimited-transactions version allows multiple transactions, subject to its specific rules.

**Q6. Can the optimized approach work with negative stock prices?**

Yes. The minimum-price and profit calculations still work mathematically, although ordinary stock-price constraints generally use non-negative values.

---

## 11. Practice Tasks

Try these variations yourself:

- [ ] Return the buying day and selling day along with the maximum profit.
- [ ] Find the minimum price and maximum price in one traversal.
- [ ] Solve the same problem using a prefix-minimum array.
- [ ] Solve the unlimited stock transactions variation.
- [ ] Explain why the brute-force solution takes O(n²) time and the optimized solution takes O(n).

---

## 12. Key Takeaways

- Track the minimum price seen so far.
- Calculate profit using the current price minus the minimum previous price.
- Update the maximum profit whenever a better opportunity is found.
- One traversal is sufficient.
- The optimized solution uses O(n) time and O(1) extra space.

**Most important pattern to remember:**

```java
int minPrice = Integer.MAX_VALUE;
int maxProfit = 0;

for (int price : prices) {

    maxProfit = Math.max(maxProfit, price - minPrice);

    minPrice = Math.min(minPrice, price);
}
```

This running-minimum pattern is useful in many array optimization problems.