# Molecule Generation using GraphRNN
This project is an implementation of GraphRNN, a deep learning model for molecular graph generation.
This approach represents molecules directly as graphs:
-	Nodes → Atoms
-	Edges → Chemical bonds

## Dataset
Dataset used:
- ZINC250K: A dataset mainly used for drug discovery due to the molecules' optimal QED score. A subset of 5000 molecules was sampled for training.
 
Supported atom types:
C, N, O, F, P, S, Cl, Br, I

Supported bond types:
- 0 = No Bond
- 1 = Single Bond
- 2 = Double Bond
- 3 = Triple Bond
- 4 = Aromatic Bond


## Molecular Graph Representation
Each molecule is converted into:

### Node Feature Matrix
Shape:
(MAX_NODES,NUM_ATOM_TYPES)


### Adjacency Matrix
Shape:
(MAX_NODES,MAX_NODES)


## Model Architecture
The model consists of two recurrent networks:
### Node RNN
Predicts atom types sequentially.

Input:
Node Features

Output:
Atom Type Distribution

### Edge RNN
Predicts bond connections between the current atom and all previously generated atoms.

Input:
Current Node Hidden State + Previous Edge Information

Output:
Bond Type Distribution

## Training
Cross Entropy Loss is used for Node and Edge Prediction.

## Molecule Generation
During sampling:
1.	Generate atom type
2.	Generate bond types to previous atoms
3.	Enforce chemical valence constraints
4.	Enforce graph connectivity
5.	Convert generated graph back to a molecule

## Results
Results obtained on 100 generated molecules:
| Metric | Score |
|----------|----------|
| Validity   | 97%  |
| Uniqueness  | 85.6%   | 
| Novelty  | 100%  |
| Average QED  | 0.22

## Inference
Strengths:
-	High validity and novelty
-	Direct graph-based representation
-	Learns atom and bond generation jointly

Limitations:
-	Generated molecules often have low QED scores
-	Large hydrophobic structures are common
-	Aromatic ring generation remains challenging

While achieving high validity (~97%), generated molecules exhibit low drug-likeness due to high molecular weight and hydrophobicity, highlighting limitations of unconstrained generative models.

High novelty indicates strong generative diversity, but combined with low QED, it suggests the model generates out-of-distribution molecules that are structurally valid but not drug-like.

We sample from the model’s learned probability distribution, but since the process is stochastic and unconstrained, it can generate valid yet suboptimal molecules.

## Tech Stack
- PyTorch
- RDKit
- NumPy
- Pandas
- HuggingFace Dataset (yairschiff/zinc250k)
	
