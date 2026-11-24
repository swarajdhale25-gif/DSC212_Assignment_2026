# DSC212: Modularity on the Karate Club Graph
**Name:** Swaraj Ajayrao Dhale
**Roll no:** IMS25066
**Course:** DS212

## Introduction
In the early 1970s, Wayne Zachary spent two years watching a university karate club. He recorded who hung out with whom, giving a network of 34 members and 78 friendships. Then the instructor and the administrator fell out over money, and the club split in two. The question: **can we find that split using only the friendship graph?**

The method we use is modularity. It scores a division by how many edges fall inside groups compared with what you'd expect if edges were random but every person kept the same number of friends. I implemented the method from scratch with NumPy, following Newman (2006), and used NetworkX only for the graph, the node metrics, and an independent check of my modularity numbers.

## What the notebook does

1. Loads the graph and builds the adjacency and modularity matrices.
2. Splits the club in two using the sign of the leading eigenvector, and keeps splitting each group while the split actually improves modularity.
3. Draws the graph after every accepted split, with the same layout each time so changes are easy to see.
4. Tracks degree, betweenness, closeness and clustering for all 34 nodes, both in the whole graph and inside each node's own community.
5. Ends with a discussion and two appendices (the maths behind the method, and an exhaustive check on the final groups).

The historical club labels are only used at the end to check the answer. They never influence the partition.

## What I found

- Three splits give four communities, of sizes 7, 9, 12 and 6.
- Modularity goes 0 → 0.3715 → 0.3727 → 0.3934.
- The first split matches the real two-way split for 33 of 34 people. Node 8 lands on the other side.
- Local metrics tell a different story from global ones. Node 1's betweenness is about 0.05 in the whole graph but about 0.65 inside its own community.

## One thing I did differently from the handout

The handout restricts the modularity matrix `B` to a group and uses that directly for recursive splits. That leaves out the contribution the group already had before splitting, so it doesn't measure the real modularity gain. I used the corrected matrix `M_C = R − diag(R·1)` from Newman's paper, which keeps one global modularity score throughout. I also check the actual gain of every split instead of relying on the eigenvalue alone, because a positive eigenvalue doesn't guarantee that the sign split helps. One group (C01) is a good example: positive eigenvalue, negative gain, so the split is rejected. Appendix A has the working.

## References

- Newman, M. E. J. (2006). "Modularity and community structure in networks." *PNAS*, 103(23), 8577–8582.
- Zachary, W. W. (1977). "An Information Flow Model for Conflict and Fission in Small Groups." *Journal of Anthropological Research*, 33, 452–473.
- [NetworkX documentation](https://networkx.org/documentation/stable/)
