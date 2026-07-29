# Fast & Slow pointers
- Two pointers start at the same position and move forward — `fast` moves at 2x speed (or with a fixed gap) compared to `slow`.
- This is a specialization of the [same-direction](../../two-pointers/concepts/same-direction.md) two-pointers variant.
- Core idea: if `fast` ever laps back around to meet `slow`, there's a cycle. If `fast` reaches the end first, there isn't one — like two runners on a track, one twice as fast as the other; they only meet again if the track loops.
## Variations
1. [cycle-detection](cycle-detection.md)
2. [cycle-entry-point](cycle-entry-point.md)