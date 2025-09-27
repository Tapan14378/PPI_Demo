# PPI_Demo

That's an excellent plan for sharing your work. The key to a good `README.md` file is using **Markdown formatting** to clearly explain the project's **purpose, methodology, and results**.

Here is a comprehensive `README.md` structure for your GitHub repository, complete with explanations of the code's working and the potential of the results.

***

# Protein-Protein Interaction (PPI) Link Prediction using Node2Vec

This repository contains a demonstration of using **Graph Neural Networks (specifically, Node2Vec embeddings)** combined with machine learning to predict novel, high-confidence interactions within the Homo sapiens (human) protein-protein interaction (PPI) network.

## 🌟 Project Potential

The primary output of this project is a $\mathbf{prioritized\ list\ of\ testable\ hypotheses}$—new protein pairs highly likely to interact but not yet documented in public databases.

* **Accelerated Discovery:** Replaces slow, broad laboratory screening with a focused list of the most probable interactions.
* **Functional Mapping:** Helps fill "gaps" in known biological pathways, such as finding a missing enzyme interaction in a metabolic process (e.g., RPIA and PGAM1).
* **Module Identification:** Uncovers new, tightly-knit functional protein groups (e.g., the WDR17 complex members).

***

## ⚙️ Methodology: The Node2Vec Workflow

The project follows a standard workflow for network embedding and link prediction:

### 1. Network Embedding (Node2Vec)

The core mechanism converts the messy, non-numerical graph structure into a clean, machine-readable format.

| Step | Mechanism | Purpose |
| :--- | :--- | :--- |
| **Random Walks** | The $\mathbf{Node2Vec}$ algorithm simulates thousands of random paths across the network. | Captures the structural context (neighborhood) of every protein. |
| **Vectorization** | Each protein is converted into a $\mathbf{64-dimensional\ vector\ (embedding)}$. | Proteins with similar function or network proximity receive numerically similar vectors. $\mathbf{\text{Vector\ Proximity \approx \text{Functional\ Similarity}}}$. |
| **Validation** | The model  distinguish between known links from non-links. | Confirms that the embeddings are a highly accurate representation of the underlying protein network structure. |

### 2. Link Feature Creation

The embeddings are transformed into features suitable for the classifier:

* **Edge Embedding:** The $64\text{D}$ vectors of two proteins ($u$ and $v$) are combined (e.g., via the $\mathbf{Hadamard\ product}$) to create a $\mathbf{single\ vector}$ that represents the $\mathbf{potential\ link}$ between $u$ and $v$.

### 3. Link Prediction

A supervised machine learning model learns the pattern of a valid link.

* **Training:** A **Random Forest Classifier** is trained on the edge embeddings of $\mathbf{known\ links}$ and $\mathbf{random\ non-links}$.
* **Discovery:** The trained classifier is applied to $\mathbf{all\ unobserved\ protein\ pairs}$ ($\approx 1.99$ million candidates in the demo).
* **Result:** Pairs with a $\mathbf{1.0\ score}$ are isolated as the most confident **new link predictions**.

***

## 🖼️ Code Working: Visualization Functions

The two primary visualizations serve distinct analytical goals:

### 1. `visualize_embeddings` (Original, Degree-based)

| Plot Purpose | Code Mechanism | Interpretation |
| :--- | :--- | :--- |
| **Structural Overview** | Plots $2,000$ proteins in the 2D $\mathbf{t-SNE}$ space. | Shows the overall clustering of the network into $\mathbf{functional\ modules}$. |
| **Color Coding** | Color is mapped to the protein's $\mathbf{Node\ Degree}$ (number of known neighbors). | $\mathbf{\text{Bright/Yellow}}$ colors highlight $\mathbf{\text{Hubs}}$ (highly connected proteins), providing a view of node importance. |

### 2. `visualize_predictions_on_embeddings` (Highlighting Discoveries)

| Plot Purpose | Code Mechanism | Interpretation |
| :--- | :--- | :--- |
| **Prediction Confirmation** | Plots all proteins, but specifically highlights the nodes involved. | Visually $\mathbf{validates}$ the predictions: the highlighted $\mathbf{blue\ nodes}$ are often found $\mathbf{clustered\ together}$ in the embedding space, confirming structural similarity. |
| **Color Coding** | **Predicted Nodes are Blue** and plotted with a larger marker; all others are grey. | **The proximity of the blue nodes is the visual evidence** that their network environments are so similar that the model is confident a link should exist. |

***

## 💾 Code Execution Summary

| Function | Output Sample | Role in Workflow |
| :--- | :--- | :--- |
| `G_demo = sample_subgraph(...)` | `Demo graph edges: 3485` | Creates the small, manageable network for training. |
| `evaluate_classifier(X, y)` | `AUC: 0.9979` | Trains the predictor and validates embedding quality. |
| **Prediction Logic** | `Total count of highly confident new links: 429` | Executes the prediction against all non-existent pairs and filters for maximum confidence. |
| `map_predictions_to_names(...)` | `protein1: WDR17, protein2: URB1` | Makes the output biologically interpretable for researchers. |
