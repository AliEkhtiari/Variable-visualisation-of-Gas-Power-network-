# Variable-visualisation-of-Gas-Power-network-
# This code is created by A. Ekhtiari to visulaise gas networks conisdering their operational varialbles such as Pressure and flow rate.
# This version can be employed to the power grids as well to demonstrate the voltage/Amper and Power variables 
# Download the essential input files to run the code
# You might need to upgrade "networkx" and "matplotlib" libraries to get the results correclty

import itertools
import copy
import networkx as nx
import pandas as pd
import matplotlib.pyplot as plt

edgelist = pd.read_csv("Edges.csv")
print(edgelist.head(10))
nodelist = pd.read_csv('GasNode.csv')
print(nodelist.head(10))
g = nx.DiGraph()
for i, elrow in edgelist.iterrows():
    g.add_edge(elrow[0], elrow[1], attr_dict=elrow[2:].to_dict())
print(elrow[0])  # node1
print(elrow[1])  # node2
print(elrow[2:].to_dict())  # edge attribute dict

# Add node attributes
for i, nlrow in nodelist.iterrows():
    #g.add_nodes_from(nlrow[1:].to_dict())
    g.node[nlrow['id']] = nlrow[1:].to_dict()
    
color_map = []
cololist = nodelist.p

for plrow in cololist:
    if (plrow >= 69.5) and (plrow <= 70):
        color_map.append('firebrick')
    elif (plrow >= 69) and (plrow < 69.5):
        color_map.append('red')
    elif (plrow >= 68.5) and (plrow < 69):
        color_map.append('salmon')
    elif (plrow >= 68) and (plrow < 68.5):
        color_map.append('red')
    elif (plrow >= 67) and (plrow < 68):
        color_map.append('mistyrose')

#g.node[nlrow['id']] = plrow[1:].to_dict()
print(nlrow)
g.edges(data=True)[0:5] # this part is for number of columns of edges
node_positions = {node[0]: (node[1]['X'], node[1]['Y']) for node in g.nodes(data=True)}

#Preview of node_positions with a bit of hack (there is no head/slice method for dictionaries).
dict(list(node_positions.items())[0:5])

edge_colors = [e[2]['color'] for e in g.edges(data=True)]
edge_width = [e[2]['flow'] for e in g.edges(data=True)]
# Preview first 10
edge_colors[0:15]
edge_width[0:15]
plt.figure(figsize=(10, 8))
# nx.draw_networkx(g, pos=node_positions, edge_color=edge_colors, node_size=15, node_color='black', with_labels=True, arrows=True)
#options = {
 #   'node_color': 'blue',
  #  'node_size': 100,
   # 'width': 3,
    #'arrowstyle': '-|>',
    #'arrowsize': 12,
#}
# nx.draw_networkx(g, pos=node_positions, arrows=True, **options)
# nx.draw_networkx_edge_labels(G, pos ,edge_labels=edge_labels)
nx.draw(g, width=edge_width, pos=node_positions, node_color=color_map, node_size=700, with_labels=True, edge_color=edge_colors, edge_cmap=plt.cm.Reds)
plt.title('Graph Representation of Sleeping Giant Trail Map', size=15)
plt.show()


