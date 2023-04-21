# 第1课 课后作业

## 第1题 
Yes, the proof would still be zero knowledge if you could pick arbitrary pairs of nodes to check, as long as the prover is still able to convince the verifier that they know a valid 3-coloring of the graph without revealing the actual coloring. The essential properties of a zero-knowledge proof are completeness, soundness, and zero-knowledge.

In the original protocol where the verifier randomly selects an adjacent pair of nodes to check, prover cannot cheat by changing the colors of non-adjacent nodes in different rounds. If the verifier could pick arbitrary pairs of nodes, the proof would still be zero knowledge as long as the prover can still convince the verifier without revealing the actual coloring. Since the verifier would still only learn whether the selected pair of nodes has different colors or not, they would not gain any additional information about the actual 3-coloring. But this might affect the soundness of the protocol, the prover cannot convince the verifier with a non-negligible probability if the prover does not know a valid 3-coloring. A malicious prover might be able to exploit this by carefully manipulating the colors of the graph to pass the checks for non-adjacent nodes. To maintain soundness, it would be necessary to modify the protocol to ensure that a malicious prover still cannot convince the verifier with a non-negligible probability.

## 第2题
The correct equation for the confidence is 1 - (1/2)^n, where n is the number of trials run. This equation comes from the fact that each time the verifier checks an adjacent pair of nodes, there is a 1/2 chance that the prover would be able to pass the check by guessing the correct color pair if they do not actually know a valid 3-coloring. After n independent trials, the probability that the prover would be able to pass all of them by guessing is (1/2)^n. Therefore, the confidence that the prover actually knows a valid 3-coloring is 1 - (1/2)^n.

The confidence calculation in this zero-knowledge proof does not rely on prior probabilities. Instead, it directly computes the probability of the prover being able to deceive the verifier based on the number of trials run, assuming no prior knowledge about the prover's honesty or knowledge of a valid 3-coloring.


