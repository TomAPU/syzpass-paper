# Primitive-Aware Aggregation Notes

## PM-Only Candidate Sites

AAW, CFH, and IF with a symbolic free argument are reported as PM-only candidate sites in the paper.
These sites follow the PM-enabled model that assumes address disclosure and attacker-controlled fake kernel objects for pointer-shaped requirements; the aggregation does not validate those prerequisites per bug.
Non-symbolic IF is treated as double-free-like and remains subject to normal object matching.
CVW and AVW use deterministic destination-offset field matching; CAW and OUW use field-satisfiability matching.

PM-only primitive sites: 276
Breakdown by primitive: {"AAW": 166, "CFH": 83, "IF": 27}
Breakdown by reason: {"arbitrary_address_write": 166, "control_flow_hijack": 83, "symbolic_free_arg": 27}
The remaining site accepted without neutralization in the PM-enabled table is one OM-clean concrete IF site; it is not part of the PM-only count.
Paper-facing totals: {"conservative_denominator": 1303, "counted": 781, "excluded_value_write_missing_destination": 522, "fail": 490, "om_avoid": 0, "om_clean": 1, "om_neutralization": 14, "pm_candidates": 291, "pm_sensitivity": 276, "syzpass_attributed": 14}

## Strict Object Matching

For primitives that still require object matching, constrained path offsets are matched in strict mode.
A matched object offset is accepted only when it lands in an attacker-controllable data region.
In the current object database, this mostly means elastic spray buffers such as msg_msg/user_key_payload-style payload areas.

## AVW/CVW Destination Availability

For offset-aware matching, the aggregation keeps an AVW/CVW primitive site only when the analyzed records determine its destination field.
The destination is then converted to a concrete vulnerable-object offset and checked directly against useful victim-object fields:

- paired same-instruction AAW/CAW/OUW primitive
- memwrite fullconstraint from the same instruction

Sites without any of those sources are excluded from the paper numbers instead of being treated as failures.

Excluded primitive sites: 522
Excluded duplicate paths: 19538
Breakdown by primitive: {"AVW": 283, "CVW": 239}

Cases with exclusions:
- 04419e3 (zhengchuancases0403): 5 primitive sites, 10 paths
- 0b1e853 (new_bugs-1-2): 17 primitive sites, 319 paths
- 10005f4 (big_test2): 7 primitive sites, 931 paths
- 10a9db4 (big_test1): 1 primitive sites, 3 paths
- 1966db2 (big_test2): 11 primitive sites, 218 paths
- 32799a3 (new_bugs-3-2): 22 primitive sites, 551 paths
- 34a0f26 (big_test3): 12 primitive sites, 160 paths
- 3538a6a (zhengchuancases0403): 3 primitive sites, 10 paths
- 3587cbb (big_test2): 37 primitive sites, 1594 paths
- 3694e28 (big_test4): 28 primitive sites, 246 paths
- 380acd1 (my3): 22 primitive sites, 729 paths
- 3a430af (zhengchuancases0403): 12 primitive sites, 2280 paths
- 3c53ee4 (big_test3): 2 primitive sites, 1071 paths
- 481a3ff (big_test1): 10 primitive sites, 104 paths
- 520f870 (big_test1): 15 primitive sites, 2559 paths
- 52bbc0a (my3): 7 primitive sites, 58 paths
- 52da8b3 (big_test4): 11 primitive sites, 119 paths
- 541ce19 (new_bugs-1-2): 2 primitive sites, 6 paths
- 61b1603 (big_test1): 1 primitive sites, 14 paths
- 6cf5652 (big_test2): 4 primitive sites, 121 paths
- 73c586d (new_bugs-1-2): 7 primitive sites, 118 paths
- 7dd7f2f (big_test3): 3 primitive sites, 33 paths
- 7e9494b (zhengchuancases0403): 2 primitive sites, 4 paths
- 8811381 (new_bugs-6): 16 primitive sites, 1033 paths
- 8aedc9a (new_bugs-1-2): 17 primitive sites, 185 paths
- 96414aa (big_test1): 3 primitive sites, 33 paths
- 98228e7 (zhengchuancases0403): 16 primitive sites, 68 paths
- 9f6c56c (zhengchuancases0403): 1 primitive sites, 2 paths
- a0ddc98 (big_test4): 6 primitive sites, 14 paths
- a7db908 (big_test1): 5 primitive sites, 304 paths
- aa6df9d (big_test3): 15 primitive sites, 565 paths
- b471b7c (new_bugs-6): 19 primitive sites, 1460 paths
- b8fe393 (big_test2): 7 primitive sites, 28 paths
- bbeb1c8 (zhengchuancases0403): 3 primitive sites, 9 paths
- bd39145 (big_test2): 1 primitive sites, 1 paths
- c041b4c (new_bugs-6): 19 primitive sites, 1460 paths
- c72da7b (big_test1): 9 primitive sites, 17 paths
- c8ae652 (big_test1): 14 primitive sites, 117 paths
- c90849c (big_test3): 15 primitive sites, 322 paths
- c96e4df (big_test3): 19 primitive sites, 944 paths
- d2c5e69 (big_test4): 30 primitive sites, 108 paths
- d8489a7 (zhengchuancases0403): 9 primitive sites, 48 paths
- e3cd8df (my3): 4 primitive sites, 6 paths
- e5db00b (big_test2): 9 primitive sites, 17 paths
- e84662c (big_test4): 1 primitive sites, 2 paths
- efae31b (big_test2): 1 primitive sites, 5 paths
- f31660c (big_test3): 42 primitive sites, 1532 paths
