# Primitive-Aware Aggregation Notes

## Auto-Usable Primitives

AAW, CFH, and IF with a symbolic free argument are counted as clean/auto-usable.
This follows the exploit-aware model that assumes address disclosure and controllable kernel memory for pointer-related primitives.
Non-symbolic IF is treated as double-free-like and remains subject to normal object matching.
CVW, AVW, CAW, and OUW remain subject to field/layout matching.

Auto-usable primitive sites: 276
Breakdown by primitive: {"AAW": 166, "FPD": 83, "IF": 27}
Breakdown by reason: {"arbitrary_address_write": 166, "control_flow_hijack": 83, "symbolic_free_arg": 27}

## Strict Object Matching

For primitives that still require object matching, constrained path offsets are matched in strict mode.
A matched object offset is accepted only when it lands in an attacker-controllable data region.
In the current object database, this mostly means elastic spray buffers such as msg_msg/user_key_payload-style payload areas.

## AVW/CVW Destination Recovery

Old AVW/CVW endstate pickles store the written value expression, not the write destination expression.
For offset-aware matching, the aggregation keeps an AVW/CVW primitive site only if at least one duplicate path provides a recoverable destination source:

- paired same-instruction AAW/CAW/OUW primitive
- memwrite fullconstraint from the same instruction
- primitive text log for the same primitive

Sites without any of those sources are excluded from the paper numbers instead of being treated as failures.

Excluded primitive sites: 13
Excluded duplicate paths: 268
Breakdown by primitive: {"AVW": 6, "CVW": 7}

Cases with exclusions:
- 3694e28 (big_test4): 3 primitive sites, 27 paths
- 541ce19 (new_bugs-1-2): 1 primitive sites, 3 paths
- 6db30c7 (big_test2): 4 primitive sites, 24 paths
- f31660c (big_test3): 5 primitive sites, 214 paths
