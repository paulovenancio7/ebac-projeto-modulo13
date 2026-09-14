# ebac-projeto-modulo13
import matplotlib.pyplot as plt
import pandas as pd
import plotly.express as px
import seaborn as sns

# Carregar base de dados
df = pd.read_csv('MODULO7_PROJETOFINAL_BASE_SUPERMERCADO.csv', delimiter=';')

# 1 e 2. Estatísticas Descritivas
estatisticas = (
    df.groupby('Categoria')['Preco_Normal']
    .agg(['mean', 'median', 'std'])
    .round(2)
)
print(estatisticas)

# 3. Boxplot - Categoria 'lacteos'
plt.figure(figsize=(10, 5))
sns.boxplot(x=df[df['Categoria'] == 'lacteos']['Preco_Normal'], color='skyblue')
plt.title('Distribuição de Preço Normal - Categoria: Lacteos')
plt.xlabel('Preço Normal')
plt.tight_layout()
plt.savefig('grafico_1_boxplot_lacteos.png')
plt.close()

# 4. Gráfico de Barras - Média de Desconto por Categoria
media_desconto = (
    df.groupby('Categoria')['Desconto']
    .mean()
    .reset_index()
    .sort_values(by='Desconto', ascending=False)
)
plt.figure(figsize=(12, 6))
sns.barplot(data=media_desconto, x='Categoria', y='Desconto', palette='viridis')
plt.title('Média de Desconto por Categoria')
plt.xlabel('Categoria')
plt.ylabel('Média de Desconto')
plt.xticks(rotation=30)
plt.tight_layout()
plt.savefig('grafico_2_barras_desconto.png')
plt.close()

# 5. Mapa Interativo (Treemap)
df_mapa = (
    df.groupby(['Categoria', 'Marca'])['Desconto']
    .mean()
    .reset_index()
    .query('Desconto > 0')
)
fig = px.treemap(
    df_mapa,
    path=['Categoria', 'Marca'],
    values='Desconto',
    color='Desconto',
    color_continuous_scale='Blues',
    title='Média de Desconto por Categoria e Marca',
)
fig.write_image('grafico_3_mapa_interativo.png')
