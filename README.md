<H3 ALIGN=RIGHT> DATE:<H3>

<H1 ALIGN=CENTER> Experiment-1: Implementation of Bayesian Networks</H1>

### Name: Deekshitha K
### Register Number: 2305002005


## Aim:

To create a Bayesian Network for the given dataset in Python.

    
## Algorithm:

**Step 1:** Import necessary libraries: pandas, networkx, matplotlib.pyplot, Bbn, Edge, EdgeType, BbnNode, Variable, EvidenceBuilder, InferenceController.

**Step 2:** Set pandas options to display more columns.

**Step 3:** Read in weather data from a CSV file using pandas.

**Step 4:** Remove records where the target variable RainTomorrow has missing values.

**Step 5:** Fill in missing values in other columns with the column mean.

**Step 6:** Create bands for variables that will be used in the model (Humidity9amCat, Humidity3pmCat, and WindGustSpeedCat).

**Step 7:** Define a function to calculate probability distributions, which go into the Bayesian Belief Network (BBN).

**Step 8:** Create BbnNode objects for Humidity9amCat, Humidity3pmCat, WindGustSpeedCat, and RainTomorrow, using the probs() function to calculate their probabilities.

**Step 9:** Create a Bbn object and add the BbnNode objects to it, along with edges between the nodes.

**Step 10:** Convert the BBN to a join tree using the InferenceController.

**Step 11:** Set node positions for the graph.

**Step 12:** Set options for the graph appearance.

**Step 13:** Generate the graph using network

**Step 14:** Update margins and display the graph using matplotlib.pyplot.

---

## Program:

```
import networkx as nx
import matplotlib.pyplot as plt

from pybbn.graph.dag import Bbn
from pybbn.graph.edge import Edge, EdgeType
from pybbn.graph.node import BbnNode
from pybbn.graph.variable import Variable
from pybbn.pptc.inferencecontroller import InferenceController


# Humidity9am
H9am = BbnNode(
    Variable(0, "H9am", ["<=60", ">60"]),
    [0.4, 0.6]
)

# Humidity3pm | Humidity9am
H3pm = BbnNode(
    Variable(1, "H3pm", ["<=60", ">60"]),
    [
        0.8, 0.2,   # H9am <=60
        0.3, 0.7    # H9am >60
    ]
)

# Wind Gust Speed
W = BbnNode(
    Variable(2, "W", ["<=40", "40-50", ">50"]),
    [
        0.3, 0.4, 0.3
    ]
)

# RainTomorrow | Humidity3pm, WindGustSpeed
RT = BbnNode(
    Variable(3, "RT", ["No", "Yes"]),
    [
        0.9, 0.1,   # <=60, <=40
        0.7, 0.3,   # <=60, 40-50
        0.5, 0.5,   # <=60, >50

        0.6, 0.4,   # >60, <=40
        0.3, 0.7,   # >60, 40-50
        0.1, 0.9    # >60, >50
    ]
)



bbn = Bbn() \
    .add_node(H9am) \
    .add_node(H3pm) \
    .add_node(W) \
    .add_node(RT) \
    .add_edge(Edge(H9am, H3pm, EdgeType.DIRECTED)) \
    .add_edge(Edge(H3pm, RT, EdgeType.DIRECTED)) \
    .add_edge(Edge(W, RT, EdgeType.DIRECTED))




join_tree = InferenceController.apply(bbn)

print("Bayesian Network Created Successfully!")



for node in join_tree.get_bbn_nodes():
    print("\nNode:", node.variable.name)



graph, labels = bbn.to_nx_graph()

pos = {
    0: (-1, 1),
    1: (-1, 0),
    2: (1, 0),
    3: (0, -1)
}

plt.figure(figsize=(8, 6))

nx.draw(
    graph,
    pos,
    labels=labels,
    with_labels=True,
    node_size=4000,
    node_color="lightblue",
    font_size=10,
    font_weight="bold"
)

plt.title("Weather Bayesian Belief Network")
plt.axis("off")
plt.show()

```

---

## Output:

<img width="859" height="662" alt="image" src="https://github.com/user-attachments/assets/1132e435-9b5f-432a-a751-00323e453cd2" />


---

## Result:
   Thus, a Bayesian Network is generated using Python.
