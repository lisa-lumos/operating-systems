# 18. Beyond Physical Memory: Policies
How to decide which page to evict. Our goal in picking a replacement policy is to minimize the number of cache misses.

This decision is made by the replacement policy of the system, which usually follows some general principles, but also includes certain tweaks to avoid corner-case behaviors.

The optimal policy replaces the page, that will be accessed furthest in the future. It serves only as a comparison point.

































