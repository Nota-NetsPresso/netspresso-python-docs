Compression Params Validator
============================

PR_L2
-----
- Number of values: 1
- Type: Float (e.g., [0.2])
- Range: 0.0 < ratio ≤ 1.0

PR_GM
-----
- Number of values: 1
- Type: Float (e.g., [0.2])
- Range: 0.0 < ratio ≤ 1.0

PR_NN
-----
- Number of values: 1
- Type: Float (e.g., [0.2])
- Range: 0.0 < ratio ≤ 1.0

PR_ID
-----
- Number of values: (Number of Out Channels - 1)
- Type: Int (e.g., [1, 2, 3, 5])
- Range: 0 < channels ≤ Number of Out Channels

FD_TK
-----
- Number of values: 2 (In Rank, Out Rank)
- Type: Int (e.g., [2, 2])
- Range: 0 < rank ≤ (Number of In Channels or Number of Out Channels)

FD_CP
-----
- Number of values: 1 (Rank)
- Type: Int (e.g., [2])
- Range: 0 < rank ≤ min(Number of In Channels or Number of Out Channels)

FD_SVD
------
- Number of values: 1 (Rank)
- Type: Int (e.g., [2])
- Range: 0 < rank ≤ min(Number of In Channels or Number of Out Channels)
