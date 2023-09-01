# Counterfactuals with Structural Causal Models (SCMs)
Counterfactual analysis offers a way to answer "what-if" questions critically important in many domains where running RCT experiments is not possible.
Below is an outline of the algorithm one can use to run simple coutnerfactual analysis on an isolated set of causally related variables.

### Pre-requisits
Basic knoweldge of SCMs and do-calculus is required.

### Motivation
The basic counterfactual entity in structural models is the sentence: `"Y would be y had X been x"`
To model an action `do(X = x)` one performs a "mini-surgery" on the causal model, that is, a minimal change necessary for establihing the antecedent `X=x`, while leaving the rest of the model intact.
Aim for a small set of equations. Identify the least important features that have a direct impact on the target and remove them from the analysis
Run counterfactuals with SCMs - note the counterfactuals effects will be propagated through the graph.

### Building blocks
* Features (`pd.DataFrame`) and target (`pd.Series`)
* Original causal graph, DAG (`nx.graph` created from the list of edges)
* Counterfactual features (`pd.DataFrame)
* Table of errors (`pd.DataFrame`)
* Learned functions 
* Modified causal graph after interventions (`nx.graph`)

### Input
* Original features (`pd.DataFrame`)
* Original target (`pd.Series`)
* List of edges to encode the causal graph 
* Counterfactual features(`pd.DataFrame`)

### Algorithm
1. Build a causal graph as `nx.graph` (check it is a DAG)
2. Each node in a graph is assigned original data as its attribute
3. Use the data to learn functions and compute errors – they are assigned to nodes in the DAG as attributes
4. Derive the list of the intervened variables by comparing original and counterfactual data
5. Do intervention: create a modified causal graph w.r.t. do-calculus rules (copy the initial DAG and remove causal links)
6. Assign counterfactual data to respective nodes in the altered causal graph
7. Propagate the interventions by traversing the causal graph and applying the functions and errors learned at step 3 on counterfactual data
8. Estimate the effect on the target
9. Profit ✨
