## 3 Combinational and Sequential Optimizations

# 3.1 Introduction to Optimization
Optimization in design means transforming logic/netlists into more efficient implementations without changing functionality.

- Area minimization (fewer gates, smaller die size).
- Power reduction (lower switching activity & leakage).
- Performance improvement (higher speed, meet timing).
- Testability and reliability (support DFT, reduce critical paths).

# 3.2 Combinational Logic Optimization

Works on pure logic (no flops).

## Constant Propagation
Constant propagation is a combinational logic optimization technique where signals or expressions that are known to have constant values are replaced with those constants throughout the logic. This reduces unnecessary logic and can simplify circuits.

## Boolean Logic Propagation
Boolean logic propagation is a combinational optimization technique where known logical relationships are used to simplify expressions and eliminate redundant logic. It leverages Boolean algebra rules to optimize circuits.

# 3.3 Sequential Logic Optimization

Works on designs with flip-flops / registers.

## State Optimization
State optimization is a sequential logic optimization technique aimed at reducing the number of states in a finite state machine (FSM) without changing its functional behavior. Fewer states mean simpler logic for next-state and output functions, leading to reduced area, power, and potentially faster timing.

## Retiming
Retiming is a sequential optimization technique where registers (flip-flops) in a circuit are repositioned across combinational logic to improve timing performance, area, or power without changing the functional behavior of the circuit.

## Sequential Logic Cloning
Sequential logic cloning is a sequential optimization technique where parts of combinational logic between registers are duplicated (cloned) to reduce fan-out, improve timing, or optimize physical placement, without changing the circuit’s functional behavior.

# Combinational vs Sequential Optimization

| Aspect         | Combinational Optimization             | Sequential Optimization                    |
|----------------|--------------------------------------|--------------------------------------------|
| Focus          | Pure logic simplification             | Registers + logic across cycles            |
| Main Benefit   | Area & power reduction, timing depth | Performance (timing closure, pipelining)  |
| Techniques     | Boolean simplification, const-prop    | Retiming, FSM state reduction, cloning     |
| When Applied   | During logic synthesis                | During advanced synthesis / physical opt  |




