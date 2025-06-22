```python
G = nx.Graph()
for i in range(len(sentencas)):
    G.add_node(i, frase=sentencas[i], embedding=embeddings[i])

# Adiciona arestas com pesos baseados na distância cosseno
for i in range(len(sentencas)):
    for j in range(i + 1, len(sentencas)):
        dist = cosine_distances([embeddings[i]], [embeddings[j]])[0][0]
        G.add_edge(i, j, weight=dist)

# 🔍 Busca K vizinhos mais próximos
def knn_busca(grafo, idx_origem, k=3):
    distancias = []
    origem = grafo.nodes[idx_origem]['embedding']
    
    for node, dados in grafo.nodes(data=True):
        if node == idx_origem:
            continue
        dist = cosine_distances([origem], [dados['embedding']])[0][0]
        distancias.append((node, dist))
    
    vizinhos_proximos = sorted(distancias, key=lambda x: x[1])[:k]
    return [(grafo.nodes[idx]['frase'], dist) for idx, dist in vizinhos_proximos]
```