- #paper/read ~ [[2021 CE]] ~ [[Embedding]], [[Modularity]]
	- **Evaluating Word Embeddings with Categorical Modularity**
	- https://arxiv.org/abs/2106.00877
	- https://github.com/enscma2/categorical-modularity
	- Mentioned papers:
		- [[Distributed Representations of Words and Phrases]]
		- [[GloVe]]
		- [[Enriching Word Vectors with Subword Information]]
		- [[Cross-lingual Word Embedding Models Survey]]
		- [[Word Translation Without Parallel Data]]
		- [[Human Brain Activity for Machine Attention]]
- ## Summary
	- Evaluation of embeddings may be _extrinsic_ (downstream task performance is measured) or _intrinsic_ (direct testing of how well embeddings capture [[Semantic Vector Space|Semantic]] or syntactic properties).
	- **Categorical modularity metric** employs 500 words drawn from [brain-based](https://pubmed.ncbi.nlm.nih.gov/27310469/) semantic categories. All words are translated into 29 [[Language|Languages]].
	- #### The technique
		1. Calculate some distance [[Function]] for all embedding pairs.
			- [[Cosine Similarity]] is used in the paper.
			- The resulting [[Matrix]] $M_D$ is _symmetrical_.
		2. For a given $k \in \mathbb{Z}_+$, build an adjacency [[Matrix]] $M_N$ for the resulting [[k-Nearest Neighbors, kNN|kNN]] [[Graph]].
			- This one is _asymmetrical_ though!
		3. Let $m$ be the total number of edges in the kNN graph.
			- To calculate it from $M_N$, let's count all the edges in the symmetrical version of the matrix and divide that by two: `m = np.sum(np.fmax(M_N, M_N.T)) // 2`.
		4. The fraction of the expected number of edges within the category $c$: $$a_c = \frac{1}{2m} \sum_{i, j} M_{N_{i,j}} \mathbb{1}(c_i = c)$$
		5. The fraction of edges that connect words of the same semantic category $c$: $$e_c = \frac{1}{2m} \sum_{i, j} M_{N_{i,j}} \mathbb{1}(c_i = c) \mathbb{1}(c_j = c)$$
		6. The overall modularity Q is calculated as follows: $$Q = \sum_{c} (e_c - a_c^2)$$
		7. Finally, it should be normalized by setting: $$Q_{max} = 1 - \sum_{c} a_c^2$$ $$Q_{norm} = \frac{Q}{Q_{max}}$$
		8. A higher value of $Q_{norm}$ indicates that a higher number of words that belong to the same categories are connected in the graph.
- ## Notes
	- Categorical modularity seems to reveal how well models map to the human [[Brain]].
		- This is especially true of [[Regression]] tasks such as [[Word Similarity]].
		- It may hint at how linguistic [[Information]] is encoded in the brain.
---
![[categorical-modularity-for-embeddings.pdf]]
